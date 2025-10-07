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

### **EC2 Pricing Models:**

* **On-Demand:** Pay for what you use. No upfront costs or long-term commitments.
* **Reserved Instances:** Pay upfront or partially upfront for a lower hourly rate over a 1- or 3-year period.
* **Spot Instances:** Bid for unused EC2 capacity at a significantly reduced price, but can be terminated by AWS.

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
