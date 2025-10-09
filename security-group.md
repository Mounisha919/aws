# 🔐 AWS Security Groups — Complete guide (exam + real-world, 5-yr level)

This is a single, focused reference to master **Security Groups** for the AWS Solutions Architect — Associate exam and real-world architecture. It covers concepts, behavior, limits, examples, best practices, troubleshooting, CLI/Terraform snippets, and exam-style scenarios.

---

## 1) What is a Security Group?

A **Security Group (SG)** is a virtual firewall that controls inbound and outbound traffic for EC2 instances (and many other AWS resources). SGs are attached to network interfaces (ENIs) and evaluate rules to decide whether to allow traffic. By default, **all traffic is denied** unless explicitly allowed. SGs are applied at the **instance (ENI) level**, not the subnet level. ([AWS Documentation][1])

---

## 2) Stateful vs. Stateless — the core behavior

* **Security Groups are stateful.** If an inbound packet is allowed, the response outbound traffic is automatically allowed (and vice-versa). You do **not** need a return rule. This is implemented with connection tracking. ([AWS Documentation][2])
* **Network ACLs (NACLs)** are **stateless** and require explicit rules for both directions. Use SGs for instance-level control and NACLs for subnet-level, stateless filtering.

**Exam tip:** If a question says “response traffic is allowed even though no outbound rule exists,” answer: **because SGs are stateful**.

---

## 3) How rules are evaluated

* There are **separate** rule sets for **Inbound** and **Outbound**.
* Rules are **allow-only** (there are no explicit deny rules). If no rule matches, traffic is denied. ([AWS Documentation][1])
* When deciding whether traffic is allowed, AWS **evaluates all rules** across **all security groups** attached to the instance — if *any* rule allows the traffic, it’s permitted. ([AWS Documentation][3])

---

## 4) What you can specify in a rule

* **Protocol:** TCP / UDP / ICMP / or `-1` (all).
* **Port or port range:** single port (22) or range (1024–65535).
* **Source / Destination:** can be a CIDR (e.g., `203.0.113.4/32`), an IPv6 CIDR, a prefix list, **or another Security Group** (by sg-id). When referencing another SG, the allowed traffic is based on the *private IPs* of the network interfaces associated with that SG. ([AWS Documentation][4])

**Important detail:** referencing a security group as a source does **not** magically import that SG’s rules — it simply allows traffic from interfaces associated with that SG. If you want mutual traffic, both SGs must have rules that allow traffic appropriately. ([AWS Documentation][4])

---

## 5) Security Groups & VPC peering / cross-account

* You **can** reference SGs across peered VPCs if the peering and region/account rules allow it (subject to restrictions). If peered across regions/accounts, check the docs — cross-region referencing has limitations. If referencing is not supported, use CIDR of the peer VPC. ([Stack Overflow][5])

---

## 6) Common practical patterns

* **Bastion + private instances:** SG for bastion allows SSH from your office IP; private app SG allows SSH only from bastion SG (reference bastion SG as source).
* **Load balancer + web tier + DB:** ALB SG allows 80/443 from internet; web SG allows 80 from ALB SG; DB SG allows 3306 only from web SG.
* **Service chaining:** Use SGs to model trust boundaries — smaller, role-based SGs are easier to reuse and audit.

---

## 7) Default behavior & defaults

* A new VPC has a **default security group** that allows inbound traffic from members of the same group and allows all outbound traffic. Instances launched with no specific SG are associated with the default SG. ([AWS Documentation][1])

---

## 8) Limits & quotas (know for exam & architecture)

* **Security groups per network interface (ENI):** default **5**, can be increased up to **16** (account/VPC quota). ([AWS Documentation][6])
* **Rules per security group:** default **60 inbound + 60 outbound** (varies by region/account and can be adjusted via support).
* **Total rules per ENI:** there is an overall rules quota (e.g., ENI rule limit * SG per ENI must not exceed a global max); AWS doc notes a combined max of rules per ENI (commonly 1,000). Always check your account limits. ([Repost][7])

> **Exam tip:** if you have many sources (hundreds of IPs), a single SG may hit the 60-rule limit. Solutions: use prefix lists, consolidate IP ranges, use fewer SGs per ENI, or use Network Firewall / NACL when appropriate.

---

## 9) Logging & monitoring

* **VPC Flow Logs** capture traffic flow and are the main way to audit and troubleshoot allowed/denied traffic for ENIs. Security Groups themselves do not produce logs — rely on VPC Flow Logs and CloudWatch. ([AWS Documentation][1])

---

## 10) Security Group vs Network ACL — quick compare

| Aspect    | Security Group                  | Network ACL                                       |
| --------- | ------------------------------- | ------------------------------------------------- |
| Stateful? | Yes (connection-tracking)       | No (stateless)                                    |
| Level     | Instance / ENI                  | Subnet                                            |
| Default   | Deny all, allow rules only      | Allow/Deny rules required for both directions     |
| Use for   | Instance access control         | Subnet-wide coarse control / add extra layer      |
| Best for  | Fine-grained per-instance rules | DDoS protection heuristics, subnet-level blocking |

---

## 11) Common mistakes & gotchas

* **Assuming SG source = public IP**: If you reference another SG, traffic is matched by the *private* IPs of the source ENIs — not their public Elastic IPs. If traffic flows through NAT or internet, use CIDR. ([Stack Overflow][8])
* **Relying on order**: SG rules are not ordered — they’re evaluated collectively; the first match concept doesn’t apply.
* **Opening 0.0.0.0/0** for management ports (SSH/RDP) — common insecure practice. Use hardened bastion or SSM Session Manager instead.
* **Hitting rule limits** — large numbers of IPs will exhaust per-SG rule counts.

---

## 12) How to create / edit Security Groups (console, CLI, IaC)

### Console (quick)

EC2 → Network & Security → Security Groups → Create security group → add Inbound/Outbound rules → attach to instance/ENI.

### AWS CLI examples

Create:

```bash
aws ec2 create-security-group --group-name web-sg --description "web servers" --vpc-id vpc-0123456789abcdef0
```

Add inbound (HTTP from anywhere):

```bash
aws ec2 authorize-security-group-ingress \
  --group-id sg-0abc1234def567890 \
  --protocol tcp --port 80 --cidr 0.0.0.0/0
```

Add inbound from another SG (allow SSH from bastion-sg):

```bash
aws ec2 authorize-security-group-ingress \
  --group-id sg-0abc1234def567890 \
  --protocol tcp --port 22 --source-group sg-0fedcba9876543210
```

Remove rule:

```bash
aws ec2 revoke-security-group-ingress ...
```

(Use fine-grained policies and audit change history.)

### Terraform (example)

```hcl
resource "aws_security_group" "web" {
  name        = "web-sg"
  description = "Allow HTTP and HTTPS"
  vpc_id      = var.vpc_id

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = { Environment = "prod" }
}
```

---

## 13) Best practices (must-know)

* **Least privilege:** only open required ports and sources (CIDR or SG).
* **Use SG references (security-group-as-source)** to avoid enumerating private IPs — simplifies policies and scaling. ([Stack Overflow][8])
* **Tag security groups** for owner, purpose, environment.
* **Use naming conventions**: `env-role-purpose` (ex: `prod-web-alb`).
* **Use prefix lists** for many IP ranges (AWS-managed or your own).
* **Automate SG management** with IaC and review changes via CI/CD.
* **Avoid 0.0.0.0/0** for management ports; prefer SSM, VPN, or bastion hosts.
* **Audit with VPC Flow Logs** and alert on unexpected traffic. ([AWS Documentation][1])

---

## 14) Troubleshooting checklist

If instance cannot talk to another host:

1. Verify SG **Inbound** rule allows the traffic on correct port/protocol.
2. Verify SG **Outbound** permits traffic (often default allows all).
3. If referencing a SG, ensure the **source instance** is actually associated with that SG and is in the same VPC/peering scope. ([AWS Documentation][4])
4. Check **Network ACLs** for subnet-level deny rules.
5. Confirm **route table** and Internet Gateway/NAT Gateway setup if traffic crosses to/from internet.
6. Check **VPC Flow Logs** to see accepted/rejected traffic. ([AWS Documentation][1])

---

## 15) Short exam-ready scenarios (with answer rationale)

**Q — A web tier must receive traffic only from an application tier. How to configure?**
A — Add inbound rule on web SG: allow port 80 from *app SG* (source = app-sg). Add outbound rule on app SG to allow port 80 to web SG or rely on stateful behavior if app initiates. Rationale: referencing SG avoids hardcoding IPs and scales as app instances change. ([Stack Overflow][8])

**Q — A developer can SSH from home but not from office. Why?**
A — Security Group likely allows SSH only from the home public IP CIDR. Office uses a different public IP — update SG or use bastion/SSM. (Exam favourite.)

**Q — You hit the 60-rule limit. What to do?**
A — Consolidate CIDRs, use prefix lists, split responsibilities across multiple SGs, or request quota increase. ([Repost][9])

---

## 16) Advanced topics & tips

* **Security Group references vs. CIDR:** referencing SG is evaluated by the private IPs of the source ENIs. For cross-subnet peering, referencing works when VPC peering supports it; otherwise use CIDR. ([AWS Documentation][10])
* **Service-linked security groups:** some AWS services create SGs on your behalf (e.g., RDS, EMR) — understand their behavior and tagging.
* **Connection tracking internals:** SGs maintain connection state for allowed flows (enables stateful behavior). ([AWS Documentation][2])

---

## 17) Quick reference (cheat sheet)

* Stateful ✅
* Default: deny inbound; allow outbound (default SG allows intra-group)
* Allow-only rules — no explicit deny
* Can specify CIDR, IPv6, prefix list, or SG as source/destination
* Common limits: SGs per ENI default **5** (up to 16), **60 inbound + 60 outbound** rules per SG, combined rules per ENI quota — check console if near limits. ([AWS Documentation][6])

---

If you want, I can:

* Generate a printable **one-page cheat sheet** with commands and common patterns.
* Walk you through a live lab: create SGs for a three-tier architecture (ALB → web → DB) with CLI/Terraform and show how traffic flows.
  Which would you like next?

[1]: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html?utm_source=chatgpt.com "Control traffic to your AWS resources using security groups"
[2]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-connection-tracking.html?utm_source=chatgpt.com "Amazon EC2 security group connection tracking"
[3]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html?utm_source=chatgpt.com "Amazon EC2 security groups for your EC2 instances"
[4]: https://docs.aws.amazon.com/vpc/latest/userguide/security-group-rules.html?utm_source=chatgpt.com "Security group rules - Amazon Virtual Private Cloud"
[5]: https://stackoverflow.com/questions/12465978/can-aws-security-groups-on-one-aws-account-reference-security-groups-in-another?utm_source=chatgpt.com "Can AWS security groups on one AWS account reference security ..."
[6]: https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html?utm_source=chatgpt.com "Amazon VPC quotas - Amazon Virtual Private Cloud"
[7]: https://repost.aws/knowledge-center/increase-security-group-rule-limit?utm_source=chatgpt.com "Increase security group rule quota in Amazon VPC"
[8]: https://stackoverflow.com/questions/45416882/aws-security-group-include-another-security-group?utm_source=chatgpt.com "AWS Security group include another Security Group - Stack Overflow"
[9]: https://repost.aws/articles/AR_rIppDrsRvKFHzb8LTjs3Q/optimizing-security-groups-in-aws-managing-growth-and-quota-constraints?utm_source=chatgpt.com "Optimizing Security Groups in AWS: Managing Growth and ..."
[10]: https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-security-groups.html?utm_source=chatgpt.com "Update your security groups to reference peer security groups"
