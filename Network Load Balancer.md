✅ Excellent question — yes, **Network Load Balancer (NLB)** in AWS **can be assigned a static IP address**, and this is one of its **major advantages** over other load balancers like the **Application Load Balancer (ALB)**.

Let’s go through this in **detail, step-by-step**, so you understand it **like an AWS Solution Architect** 👇

---

## 🧩 1. Why Assign a Static IP to a Load Balancer?

Normally, when you create a load balancer (especially an **ALB**), it gets a **DNS name** (like `my-alb-123456.elb.amazonaws.com`) that resolves to **dynamic IPs**.
These IPs can **change** if AWS replaces infrastructure (e.g., scaling events, maintenance, failover).

But in some use cases — like **firewall whitelisting**, **third-party integrations**, or **on-premise connectivity** — you need **fixed, unchanging IP addresses**.

That’s where **Network Load Balancer (NLB)** helps.

---

## ⚙️ 2. Static IP Addresses in Network Load Balancer

When you create an **NLB**, AWS automatically allocates **one IP address per Availability Zone (AZ)** — but these can change if you delete and recreate the NLB.

To make them **truly static**, you can **assign your own Elastic IP (EIP)** to each AZ.

---

### 🔧 Steps to Assign Static IPs (Elastic IPs) to NLB:

1. **Go to EC2 Console → Load Balancers → Create Load Balancer → Network Load Balancer**
2. In the **Network Mapping** section:

   * Choose your **VPC** and **subnets** (for each AZ).
   * Under each subnet, you’ll see an option:

     ```
     [x] Assign a static IP address
     ```
   * Click **Assign Elastic IP**, then:

     * Choose an existing EIP **or**
     * Allocate a new one (AWS gives one per subnet).
3. Complete the remaining setup:

   * **Listeners** (typically TCP or UDP)
   * **Target groups**
4. After creation, your NLB will now have **static public IP addresses (EIPs)** — one per subnet.

✅ **Result:** These IPs will never change, even if AWS replaces NLB infrastructure internally.

---

## 🧠 3. Comparison – ALB vs NLB in IP Handling

| Feature    | Application Load Balancer (ALB) | Network Load Balancer (NLB)      |
| ---------- | ------------------------------- | -------------------------------- |
| Static IP  | ❌ Not supported                 | ✅ Supported                      |
| Elastic IP | ❌ Not supported                 | ✅ Can attach EIPs                |
| DNS Record | Yes (dynamic IPs behind DNS)    | Yes (can resolve to static EIPs) |
| Protocols  | HTTP/HTTPS/WebSocket            | TCP/UDP/TLS                      |
| Layer      | Layer 7                         | Layer 4                          |

---

## 💼 4. Example Use Case

Imagine you have a **corporate firewall** that only allows outgoing traffic to **whitelisted IPs**.
If you use an **ALB**, those IPs might change → connection breaks.
If you use an **NLB with EIPs**, you can whitelist those **fixed IPs**, ensuring stable connectivity.

---

## 🔐 5. Bonus — Using Private IPs for Internal NLB

If your **NLB** is **internal (not internet-facing)**, you can still assign **private static IPs** in your VPC subnets instead of EIPs.

This is useful for:

* Internal microservices
* Private communication between VPCs or on-prem

---

## 🧩 Summary:

| Concept              | Description                                            |
| -------------------- | ------------------------------------------------------ |
| **Static IPs**       | You can assign Elastic IPs (EIPs) to NLB for fixed IPs |
| **Per-AZ IP**        | Each AZ (subnet) has one EIP                           |
| **Why needed**       | For whitelisting, fixed endpoints, compliance          |
| **Not supported in** | ALB and Gateway Load Balancer                          |
| **Internal NLB**     | Can use private static IPs                             |

---

Would you like me to continue next with **NLB Listener Configuration & Target Group Algorithms (Round-robin, Least Connections, etc.)** — it’s the natural next topic in load balancing setup for AWS Solution Architect prep?
