### **What is EC2?**

Amazon EC2 is a **web service** that provides scalable compute capacity in the cloud. It's essentially a virtual machine (VM) that runs on AWS infrastructure, offering you complete control over your computing resources. EC2 allows you to run applications without having to manage the physical servers.

#### **Key Features of EC2:**

* **Scalable:** You can scale up or scale down the number of EC2 instances based on your needs.
* **Flexible:** Multiple instance types, operating systems (Linux/Windows), and configurations.
* **Pay-as-you-go:** You pay only for what you use, with no upfront costs.

---

### **EC2 Core Concepts:**

1. **Instances:**

   * **Virtual Machines (VMs)** that run on AWS infrastructure.
   * You can choose instance types based on performance needs (e.g., CPU, memory, storage).

2. **Instance Types:**

   * EC2 offers several **instance families** optimized for different use cases:

     * **General Purpose:** Balanced compute, memory, and networking (e.g., `t3`, `m5`).
     * **Compute Optimized:** More CPU power for compute-heavy tasks (e.g., `c5`).
     * **Memory Optimized:** More RAM for memory-intensive applications (e.g., `r5`).
     * **Storage Optimized:** High I/O performance for storage-intensive workloads (e.g., `i3`).
     * **GPU Instances:** For applications that require graphic processing units (e.g., machine learning).

3. **AMI (Amazon Machine Image):**

   * AMIs are templates that define the software configuration (OS, application server, apps).
   * AWS offers many pre-built AMIs, or you can create your own custom AMI.

4. **Key Pair:**

   * EC2 instances require a **key pair** for SSH access (for Linux instances) or RDP (for Windows instances).
   * A key pair consists of a private and a public key. You only download the **private key**.

5. **Security Groups:**

   * **Firewalls** for your EC2 instances.
   * Define which IPs and ports can access your instance.
   * Security groups can be customized for different levels of access.

6. **Elastic IP (EIP):**

   * A static IP address that you can associate with an EC2 instance.
   * Useful if you need a persistent IP address (e.g., for a web server).

---

### **How to Launch an EC2 Instance:**

1. **Step 1: Select an AMI:**

   * Go to the **EC2 Dashboard** > Launch Instance.
   * Choose a pre-built **Amazon Linux** or **Windows** AMI, or use a custom AMI if available.

2. **Step 2: Choose an Instance Type:**

   * Select an instance type (e.g., `t2.micro` for testing, or `m5.large` for general-purpose use).

3. **Step 3: Configure Instance:**

   * Set the number of instances you need.
   * Configure networking settings (which VPC, subnet, etc.).
   * Decide whether you want to enable **auto-assign public IP**.

4. **Step 4: Add Storage:**

   * Attach additional EBS volumes if needed.
   * Define volume types: `gp2` (general-purpose SSD), `io1` (provisioned IOPS), etc.

5. **Step 5: Configure Security Group:**

   * Add a security group rule to allow **SSH** (port 22) for Linux instances or **RDP** (port 3389) for Windows.
   * You can allow access from anywhere (0.0.0.0/0) or restrict to specific IP ranges.

6. **Step 6: Review and Launch:**

   * Review the configuration and click **Launch**.
   * Select or create a new **key pair** for access.

7. **Access Your Instance:**

   * Once your EC2 instance is running, you can connect to it via **SSH** (Linux) or **RDP** (Windows) using your private key or password.

---

### **Important EC2 Features:**

1. **Auto Scaling:**

   * EC2 can be set up to automatically scale based on traffic. For example, you can configure your application to launch more EC2 instances during peak traffic and scale down when the load decreases.

2. **Elastic Load Balancer (ELB):**

   * **ELB** can distribute incoming traffic across multiple EC2 instances to balance the load and ensure availability.

3. **Elastic Block Store (EBS):**

   * Persistent storage volumes attached to EC2 instances. EBS allows you to store your data even if the EC2 instance is terminated.

4. **Spot Instances:**

   * EC2 instances that allow you to bid for unused compute capacity at a lower price. Spot instances can be interrupted, so they're ideal for non-critical workloads.

5. **Reserved Instances:**

   * Reserved Instances allow you to commit to using EC2 for a 1- or 3-year term in exchange for a lower hourly rate.

6. **Elastic GPU:**

   * Attach a GPU to an EC2 instance to accelerate graphics-heavy applications (e.g., ML, gaming).

---

Absolutely! Below is the **expanded README** including details on **Dedicated Hosts**, **Dedicated Instances**, and **On-Demand Capacity Reservations**. These models offer more control over EC2 instances and their usage. They are especially useful for workloads that require compliance with specific regulatory requirements or the need for physical server isolation.

---

# **AWS EC2 Pricing Models - Detailed Explanation**

## **Introduction**

Amazon EC2 (Elastic Compute Cloud) provides a flexible and scalable computing environment in the cloud. To optimize your costs based on the nature of your workload, AWS provides **several pricing models** for EC2 instances: **On-Demand**, **Reserved Instances**, **Spot Instances**, **Dedicated Hosts**, **Dedicated Instances**, and **On-Demand Capacity Reservations**. Understanding these models will help you decide on the most cost-effective and efficient way to use EC2 instances for your workload.

## **1. On-Demand Instances**

### **Overview:**

**On-Demand Instances** are the most flexible pricing model in AWS EC2. You pay for the compute capacity by the second (with a minimum of 60 seconds) and are charged for the amount of time the instance is running. There are no upfront commitments or long-term contracts. This model is ideal for unpredictable workloads or short-term applications where flexibility is important.

### **Key Features:**

* **Pay-as-you-go:** You pay for compute time only when your instances are running.
* **No Upfront Costs:** No need to make any upfront payment.
* **Flexibility:** Ideal for workloads that cannot be predicted and need the ability to scale up or down at any time.
* **Termination Flexibility:** You can terminate instances anytime, with no long-term commitments.

### **Use Cases:**

* **Development and Testing:** When testing new features or configurations where the demand is unpredictable.
* **Short-term Applications:** Temporary projects or projects with sporadic usage.
* **Web Applications:** Use when traffic spikes are unpredictable, such as in an e-commerce site or marketing campaign.
* **Disaster Recovery:** Applications that require quick recovery without long-term commitments.

### **Cost Considerations:**

* On-demand pricing is the most expensive model on an hourly basis, as you are paying for the full flexibility of instant scalability and no long-term commitment.
* On-demand instances are typically more expensive compared to Reserved Instances or Spot Instances, but they provide unmatched flexibility.

### **Pricing Example:**

For an **m5.large** instance (2 vCPUs and 8GB RAM) in the US East (N. Virginia) region, the on-demand price might be approximately **$0.096 per hour**.

---

## **2. Reserved Instances (RIs)**

### **Overview:**

**Reserved Instances (RIs)** allow you to commit to a specific EC2 instance type, region, and availability zone over a term of **1 or 3 years** in exchange for a lower hourly rate. Reserved Instances are a cost-effective option for steady-state workloads that require a consistent level of compute capacity over a long period.

### **Key Features:**

* **Commitment to 1- or 3-Year Term:** You commit to a term (1 or 3 years), and in return, you receive a significant discount on the hourly rate.
* **Upfront Payment Options:** You can pay in full upfront, partially upfront, or with no upfront payment (for standard RIs). The more you pay upfront, the higher the discount.
* **Capacity Reservation:** You reserve capacity in the chosen region and availability zone.
* **Discounts:** Reserved Instances provide up to 75% discount over On-Demand pricing, depending on the payment option and term length.
* **Predictable Costs:** Since you know the pricing upfront, it’s easier to budget for workloads with predictable demand.

### **Types of Reserved Instances:**

1. **Standard RIs:**

   * Provide the largest discount, but offer the least flexibility.
   * Can be applied to specific instance types, regions, and availability zones.
   * Cannot be modified or exchanged (though you can sell them in the Reserved Instance Marketplace).
2. **Convertible RIs:**

   * Allow you to exchange your reserved instances for another instance type, region, or OS. Offers flexibility at a slightly lower discount than Standard RIs.
   * Great for environments where workload needs might evolve over time.
3. **Scheduled RIs:**

   * Reserved for specific time periods (e.g., specific days and hours of the week). This is ideal for predictable workloads that only run during certain hours of the day or week.

### **Cost Considerations:**

* Reserved Instances are a **long-term commitment**, so they make sense for applications that have predictable, steady-state usage.
* The discount depends on your upfront commitment:

  * **All Upfront (AURI):** Highest discount, but requires full payment upfront.
  * **Partial Upfront (PURI):** Lower discount than AURI, but you pay part of the cost upfront and the rest on an hourly basis.
  * **No Upfront (NURI):** Provides the lowest discount but doesn't require any upfront payment.
* For instance, for an **m5.large** instance, the **1-year, All Upfront Reserved Instance** could cost approximately **$510** (about $0.058 per hour).

---

## **3. Spot Instances**

### **Overview:**

**Spot Instances** allow you to bid for unused EC2 capacity at a **significantly reduced price**, often up to 90% lower than the On-Demand rate. These instances can be terminated by AWS with very short notice (usually 2 minutes), and they are ideal for **flexible workloads** that can tolerate interruptions.

### **Key Features:**

* **Bid for Unused Capacity:** You specify the maximum price you are willing to pay per hour, and if your bid meets or exceeds the Spot price, your instance is launched.
* **Terminated by AWS:** AWS can terminate Spot Instances at any time if they need the capacity back, which is why it’s important to have your workload designed to tolerate interruptions.
* **Cost Savings:** Spot Instances provide significant cost savings—up to 90% off the On-Demand price.
* **Interruptible Workloads:** Spot Instances are best suited for workloads that can handle interruptions, such as batch jobs, big data processing, CI/CD, or background processing.

### **Use Cases:**

* **Batch Processing:** For workloads such as data analytics, rendering jobs, or big data processing where a task can be paused and resumed later.
* **CI/CD Workflows:** For running automated tests, builds, and deployment pipelines that are not time-sensitive.
* **Machine Learning and AI:** For training large machine learning models that can handle interruptions.
* **High-Performance Computing (HPC):** When the computation can be distributed and paused.

### **Pricing Example:**

For an **m5.large** instance in **US East (N. Virginia)**, the Spot price might range from **$0.03 to $0.06 per hour**, depending on demand.

---

## **4. Dedicated Hosts**

### **Overview:**

A **Dedicated Host** is a physical server dedicated to your use. With Dedicated Hosts, you have full control over the instance placement and the underlying hardware, which is important for applications that require physical isolation for compliance or licensing reasons.

### **Key Features:**

* **Physical Server Isolation:** Unlike other EC2 instances, where multiple tenants share the same hardware, **Dedicated Hosts** provide physical isolation. This is required for some applications (e.g., certain licensed software that require dedicated physical resources).
* **Bring Your Own License (BYOL):** You can use your existing software licenses for Windows Server, SQL Server, and other software on a dedicated host, saving costs on licensing.
* **Control Over Instance Placement:** Dedicated Hosts allow you to control where instances are placed on the host and align them with your organizational needs.

### **Use Cases:**

* **Regulatory Compliance:** Some industries require that workloads run on dedicated hardware for security or compliance reasons.
* **Licensing Restrictions:** If your software vendor requires that you run on dedicated servers, you can use Dedicated Hosts to fulfill licensing requirements.
* **High-Performance Workloads:** For workloads that require physical isolation for security or performance.

### **Pricing Example:**

For a **m5.large** instance on a Dedicated Host, the pricing can range from **$0.096 per hour** for the instance, with an additional cost for the host itself.

---

## **5. Dedicated Instances**

### **Overview:**

**Dedicated Instances** are Amazon EC2 instances that run on hardware dedicated to a single customer. They are similar to EC2 instances but with the added benefit of running on physical hardware that is isolated from other AWS customers.

### **Key Features:**

* **Instance Isolation:** Dedicated Instances run on physical servers that are dedicated to your account, ensuring that you are not sharing the underlying hardware with other customers.
* **AWS Managed Infrastructure:** You do not manage the physical hardware, but you benefit from the isolation of Dedicated Instances.
* **Flexible Instance Type:** You can use any EC2 instance type with Dedicated Instances.

### **Use Cases:**

* **Dedicated Capacity:** Workloads that require physical hardware isolation but don’t require full control over the host (e.g., compliance, licensing).
* **Highly Sensitive Workloads:** Where isolation from other tenants is critical, but full control over the server isn’t necessary.

### **Pricing Example:**

Dedicated Instances are billed at **On-Demand prices** with additional charges for the physical hardware, depending on the instance type and region.

---

## **6. On-Demand Capacity Reservations**

### **Overview:**

**On-Demand Capacity Reservations** allow you to reserve EC2 capacity in a specific Availability Zone for any duration. You pay for the reserved capacity on an hourly basis, similar to On-Demand Instances, but you are guaranteed capacity for critical workloads


.

### **Key Features:**

* **Guaranteed Capacity:** You can reserve capacity for EC2 instances in specific Availability Zones, ensuring that the capacity is available when you need it.
* **Pay for Reserved Capacity:** You pay for the reserved capacity whether you use it or not.
* **No Long-Term Commitment:** Capacity reservations can be modified, and you can release them when no longer needed.

### **Use Cases:**

* **Critical Workloads:** If you have a workload that requires guaranteed capacity but does not fit the long-term commitment of Reserved Instances.
* **Capacity Planning:** For applications that require specific amounts of compute capacity and need to ensure availability during peak times.

### **Pricing Example:**

For a **c5.large** instance in **US East (N. Virginia)**, the On-Demand Capacity Reservation could cost approximately **$0.05 per hour**, in addition to the cost of the EC2 instance.

---

## **Conclusion**

Choosing the right EC2 pricing model depends on your workload’s **availability requirements**, **cost sensitivity**, and **flexibility needs**:

* **On-Demand Instances** are best for unpredictable, short-term, or variable workloads.
* **Reserved Instances** offer long-term savings for predictable workloads but come with a commitment.
* **Spot Instances** offer the best cost savings but should be used for fault-tolerant, interruptible workloads.
* **Dedicated Hosts** and **Dedicated Instances** offer physical isolation for compliance or licensing needs.
* **On-Demand Capacity Reservations** allow you to reserve EC2 capacity for critical applications with guaranteed availability.

This breakdown should provide you with a comprehensive understanding of each EC2 pricing model and guide you in selecting the best option for your workload.

---
Perfect — you want a **README-style full explanation about EC2 Tags** like we did for EC2 pricing models (deep, 5-years-experience level).
Below is your **complete “AWS EC2 Tags” detailed guide** — structured, interview-ready, and practical for real projects and the AWS Solutions Architect Associate exam 👇

---

# 🏷️ **AWS EC2 Tags — Complete Detailed Explanation**

## **1. Introduction**

In AWS, **tags** are metadata labels that you attach to AWS resources (like EC2 instances, volumes, security groups, etc.).
They consist of **key–value pairs** that help you organize, manage, and identify resources effectively across your AWS environment.

Think of tags like **sticky labels** you place on your resources — "Environment=Production", "Owner=DevOps", "CostCenter=Marketing", etc.

Tags are critical for:

* **Cost allocation**
* **Automation**
* **Resource organization**
* **Access control**
* **Operational governance**

---

## **2. Tag Structure**

Each tag is a **Key-Value Pair:**

| Attribute | Description                       | Example                                     |
| --------- | --------------------------------- | ------------------------------------------- |
| **Key**   | Identifier for the tag (required) | `Environment`, `Owner`, `Project`           |
| **Value** | Optional descriptive text         | `Production`, `Mounisha`, `TASK_MANAGEMENT` |

* Keys are **case-sensitive** and must be unique per resource.
* Each resource can have up to **50 tags**.
* Both **key and value** can be up to **128 and 256 Unicode characters**, respectively.

---

## **3. Why Tags Are Important**

### ✅ **a. Cost Management & Billing**

AWS allows you to **enable cost allocation tags** in the Billing Console.

* When you tag resources (e.g., `Project=TASK_MANAGEMENT`, `Team=DevOps`), AWS can show **cost breakdown per tag**.
* This helps identify **which project, team, or environment** consumes the most resources.
* Useful for **chargeback/showback models** in large organizations.

**Example:**

| Tag                     | Monthly Cost |
| ----------------------- | ------------ |
| Project=TASK_MANAGEMENT | $120         |
| Project=MarketingAPI    | $85          |

👉 Enable cost allocation tags in:
**Billing Console → Cost Allocation Tags → Activate user-defined tags**

---

### ✅ **b. Resource Organization**

AWS environments can quickly become large and complex.
Tags act as a **filtering mechanism** to group and locate resources easily.

**Example:**

* Find all EC2 instances in a particular project:

  ```
  Project=TASK_MANAGEMENT
  ```
* Identify environments:

  ```
  Environment=Production
  Environment=Staging
  Environment=Development
  ```

You can filter resources by tags in:

* **AWS Console**
* **AWS CLI**
* **Resource Groups**
* **AWS Config**

---

### ✅ **c. Automation**

Tags are used extensively in automation scripts and Infrastructure as Code (IaC) tools.

**Example use cases:**

* **Auto-scaling groups**: scale EC2 instances with specific tags.
* **Backup automation**: Lambda functions take snapshots of instances with tag `Backup=True`.
* **Termination protection**: Prevent deletion of instances with `Critical=True`.

**Example (Python – boto3):**

```python
import boto3
ec2 = boto3.resource('ec2')
instances = ec2.instances.filter(Filters=[{'Name': 'tag:Backup', 'Values': ['True']}])
for instance in instances:
    instance.create_image(Name=f"Backup-{instance.id}")
```

---

### ✅ **d. Access Control (IAM Policies)**

Tags can be integrated into **IAM policies** for **fine-grained access control**.

Example:

* Allow developers to manage only the resources they own:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "ec2:*",
    "Resource": "*",
    "Condition": {"StringEquals": {"aws:ResourceTag/Owner": "${aws:username}"}}
  }]
}
```

This ensures users can perform actions **only on resources they tagged** as their own.

---

### ✅ **e. Compliance & Security**

Tags play a key role in governance:

* Enforce tagging policies (using AWS Config or Service Control Policies)
* Identify **unapproved or untagged resources**
* Use **AWS Resource Groups Tagging API** for audit reports

Example: Automatically detect resources **without mandatory tags** using AWS Config rules like:

```
required-tags
```

→ Ensures every resource has tags such as:

```
Environment, Owner, CostCenter
```

---

## **4. Tagging Best Practices**

| Best Practice                                  | Description                                                                         |
| ---------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Use Consistent Naming Conventions**          | e.g., `Environment=Prod`, `Owner=Mounisha`, not `env=production`.                   |
| **Define Tagging Standards Organization-Wide** | Create a centralized tagging policy and enforce it via AWS Organizations or Config. |
| **Use Mandatory Tags**                         | Require certain tags for all resources (Environment, Owner, CostCenter).            |
| **Avoid Freeform/Ad-hoc Tags**                 | Keep naming consistent to avoid confusion.                                          |
| **Automate Tagging**                           | Use IaC (Terraform/CloudFormation) to automatically tag resources at creation.      |
| **Regularly Audit Tags**                       | Use AWS Config and Resource Groups to find missing or incorrect tags.               |
| **Enable Cost Allocation Tags**                | Turn on billing tags to track cost per tag in Cost Explorer.                        |
| **Use Case-Consistent Tag Keys**               | Avoid duplicates like `Owner` and `owner` — AWS treats them as different.           |

---

## **5. Tag Propagation**

Some AWS services **propagate tags automatically** to related resources.
Example:

* When you tag an **Auto Scaling Group (ASG)**, tags can propagate to instances launched by the ASG.

✅ You can enable:

> **“PropagateAtLaunch=True”** in your ASG tag settings.

```bash
aws autoscaling create-auto-scaling-group \
--auto-scaling-group-name WebASG \
--tags Key=Environment,Value=Production,PropagateAtLaunch=true
```

---

## **6. Tools for Managing Tags**

### **a. AWS Console**

You can view and edit tags directly in:

* EC2 Dashboard → Instances → Tags tab
* Resource Groups → Tag Editor

### **b. AWS CLI**

```bash
# Add a tag
aws ec2 create-tags --resources i-0abcd1234 --tags Key=Environment,Value=Production

# List all tags
aws ec2 describe-tags --filters "Name=resource-type,Values=instance"

# Delete a tag
aws ec2 delete-tags --resources i-0abcd1234 --tags Key=Environment
```

### **c. AWS Tag Editor**

Centralized UI in the console to **search, edit, and delete tags** across all regions.

**Path:**

> AWS Management Console → Resource Groups → Tag Editor

---

## **7. Real-Time Use Cases (Scenario Based)**

### **Scenario 1:** Cost Optimization

Your finance team wants to track which department uses the most EC2 resources.

* You add tags like:
  `Department=Finance`, `Department=HR`, `Department=DevOps`
* Then enable **Cost Allocation Tags** → use **AWS Cost Explorer** to view spend by department.

---

### **Scenario 2:** Automation with Lambda

You want to automatically back up only critical EC2 instances.

* Add tag: `Backup=True`
* A Lambda function runs daily → creates AMI snapshots for all instances with that tag.

---

### **Scenario 3:** Security Control

Your security team requires that all instances have a valid **Owner** tag.

* Use AWS Config’s **required-tags** rule.
* Any instance without the `Owner` tag is flagged **non-compliant**.

---

### **Scenario 4:** Access Control

Developers can only stop/start EC2 instances that they own.

* Each instance has `Owner=${aws:username}`.
* IAM policy uses tag-based condition to restrict permissions.

---

### **Scenario 5:** Environment Management

You maintain multiple environments — Dev, QA, and Prod.

* Tags:
  `Environment=Dev`, `Environment=QA`, `Environment=Prod`
* You can filter and manage them separately in the AWS console or CLI.

---

## **8. Tag Lifecycle Management**

AWS allows **tagging at creation time** for many services (like EC2, S3, Lambda, RDS).
This ensures no resource exists **without mandatory tags**.

You can enforce this using:

* **Service Control Policies (SCPs)** in AWS Organizations
* **AWS Config Rules**
* **CloudFormation Stack Policies**

Example SCP:

```json
{
  "Effect": "Deny",
  "Action": "ec2:RunInstances",
  "Resource": "*",
  "Condition": {
    "Null": {
      "aws:RequestTag/Environment": "true"
    }
  }
}
```

→ This denies EC2 instance creation if `Environment` tag is missing.

---

## **9. Limitations**

* Each resource supports up to **50 tags**.
* Keys and values are **case-sensitive**.
* Tags are **region-specific**.
* Not all services support tagging at creation time.
* **Tag quotas** differ slightly for certain services.

---

## **10. Summary Table**

| Purpose                    | Tag Example               | AWS Service Involved   |
| -------------------------- | ------------------------- | ---------------------- |
| Cost Tracking              | `CostCenter=1234`         | Billing, Cost Explorer |
| Environment Identification | `Environment=Production`  | EC2, Lambda, RDS       |
| Automation                 | `Backup=True`             | Lambda, EventBridge    |
| Security & Compliance      | `Owner=Mounisha`          | AWS Config             |
| Access Control             | `Owner=${aws:username}`   | IAM                    |
| Resource Grouping          | `Project=TASK_MANAGEMENT` | Resource Groups        |
| Application Management     | `App=Ecommerce`           | CloudFormation, ECS    |

---

## **11. Interview-Level Summary (5 Years Experience)**

> “In AWS, tagging is a fundamental governance and cost management mechanism.
> I always implement a **standardized tagging policy** across environments — typically covering Environment, Owner, Application, CostCenter, and Compliance tags.
> I enable **cost allocation tags** for financial reporting and enforce compliance through **AWS Config rules**.
> For automation, I integrate tags into Lambda functions (e.g., backup and cleanup automation) and for **access control**, I apply **tag-based IAM policies**.
> This ensures both operational efficiency and financial transparency across multi-account AWS environments.”

---



### **Lab: Launch EC2 Instance**

1. **Go to AWS EC2 Console:**

   * Open [AWS Management Console](https://aws.amazon.com/console/), navigate to EC2.

2. **Launch an Instance:**

   * Click **Launch Instance** to create a new EC2 instance.
   * Choose a free-tier eligible AMI, like Amazon Linux 2.
   * Select **t2.micro** (free tier eligible).
   * Configure your instance: leave defaults (you can change them later).
   * Add storage: Leave the default 8GB.
   * Add tags: Optional (e.g., Name = “MyFirstEC2”).
   * Configure the security group: Add SSH (port 22) and RDP (port 3389) as needed.
   * Select or create a new key pair: Download the private key (ensure to keep it safe).

3. **Connect to Your Instance:**

   * Once the instance is running, select it from the dashboard.
   * Use **SSH** (for Linux) or **RDP** (for Windows) to connect using the private key you downloaded.

---

### **Review & Questions:**

* Did you understand the EC2 instance launch process?
* Are you comfortable with configuring security groups, storage, and connecting to your instance?

Once you’ve done the lab, I’ll provide questions for a quick self-test!

Let me know if you need help with any step.
