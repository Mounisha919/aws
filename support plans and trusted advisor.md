
# ☁️ AWS Support Plans

AWS offers **4 levels of support**, depending on how critical your workloads are.

---

### 🔹 1. **Basic Support (Free)**

* **Included for all AWS accounts**.
* Access to:

  * **24×7 customer service** (billing, account issues).
  * **Documentation, whitepapers, AWS forums**.
  * **Trusted Advisor – Core checks only** (Security + Service Limits).

👉 **Real-Time Example**

* You create an **S3 bucket** for testing. Accidentally, you make it **public**.
* Trusted Advisor (core check) alerts: *“Your bucket is public.”*
* You fix permissions → security risk avoided.

---

### 🔹 2. **Developer Support (Starts at \$29/month)**

* **Business-hours** email support from AWS engineers.
* Response times:

  * **General guidance** (<24 hrs).
  * **System impaired** (<12 hrs).
* Best for **development/testing environments**, not production.

👉 **Real-Time Example**

* Your developer team is building a web app.
* You face an error integrating **AWS Lambda with API Gateway**.
* Raise a ticket → AWS responds within business hours → issue fixed.

---

### 🔹 3. **Business Support (Starts at \$100/month)**

* **24×7 access** via phone, chat, email.
* Response times:

  * **Production system impaired** (<4 hrs).
  * **Production system down** (<1 hr).
* Includes **all Trusted Advisor checks** (Cost, Security, Performance, Fault Tolerance, Service Limits).
* **Infrastructure Event Management** for big launches/migrations.

👉 **Real-Time Example**

* You’re running an **e-commerce site**.
* During festival sales, traffic spikes → EC2 instances are hitting limits.
* Trusted Advisor warns: *“You’re close to EC2 limit.”*
* You request a limit increase → avoid downtime during peak sales.

---

### 🔹 4. **Enterprise Support (Starts at \$15,000/month)**

* For **mission-critical workloads**.
* Everything in Business, plus:

  * **Technical Account Manager (TAM)** – your personal AWS advisor.
  * **Concierge support team** for billing/security.
  * **Response times**:

    * Business-critical (<15 min).
* Custom **Well-Architected Reviews** and **Operations Reviews**.

👉 **Real-Time Example**

* You are a **bank** running core payment systems on AWS.
* Suddenly, your **production RDS database** goes down.
* With Enterprise Support → AWS responds in **15 min**, helps with failover, and TAM ensures long-term fixes.

---

# 🛡️ AWS Trusted Advisor

Think of it as your **cloud doctor** 🩺 – continuously checking your AWS account for issues.

It gives **recommendations** in **5 categories**:

---

### 1️⃣ **Cost Optimization 💰**

* Finds unused/underutilized resources.

👉 Example:

* EC2 instance running at <10% CPU for 14 days.
* Trusted Advisor flags it → You stop/resize → save \$\$ on monthly bill.

---

### 2️⃣ **Security 🔒**

* Detects security gaps.

👉 Example:

* Trusted Advisor alerts: *“IAM user has access key not used for 90 days.”*
* You delete/rotate the key → reduce security risk.

---

### 3️⃣ **Fault Tolerance 🔄**

* Ensures apps survive failures.

👉 Example:

* RDS instance is in **single AZ**.
* Trusted Advisor warns → You enable **Multi-AZ** → DB fails over automatically if one AZ goes down.

---

### 4️⃣ **Performance ⚡**

* Improves speed and efficiency.

👉 Example:

* EC2 instance is **t2.micro** but always at 90% CPU.
* Trusted Advisor recommends upgrading to **t3.medium** → app runs smoother.

---

### 5️⃣ **Service Limits 📊**

* Warns when close to AWS resource limits.

👉 Example:

* Your account allows **20 Elastic IPs**, you’ve used 18.
* Trusted Advisor flags it → You request a limit increase → no deployment failures later.

---

# 📊 Free vs Paid Trusted Advisor

* **Basic & Developer Support** → Only **2 checks** (Security + Service Limits).
* **Business & Enterprise Support** → Full **200+ checks** across all 5 categories.

---

# ✅ Interview-Ready Summary

* **Support Plans**:

  * **Basic** → Docs + Core checks.
  * **Developer** → Business-hours support.
  * **Business** → 24×7, all Trusted Advisor checks, production-ready.
  * **Enterprise** → TAM, concierge, 15-min critical response, mission-critical.

* **Trusted Advisor**:

  * Checks across **Cost, Security, Performance, Fault Tolerance, Limits**.
  * Examples:

    * Cost → Stop idle EC2.
    * Security → Restrict public S3.
    * Fault Tolerance → Enable Multi-AZ RDS.
    * Performance → Upgrade EC2 if under-provisioned.
    * Service Limits → Request increases before outage.

