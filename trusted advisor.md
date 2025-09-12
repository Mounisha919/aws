Perfect 👌 Let’s add a **Fault Tolerance** real-time example to complete the set. You’ll then have **Security + Cost + Fault Tolerance** cases handy for interviews.

---

# 🛡️ AWS Trusted Advisor

## 📌 What is AWS Trusted Advisor?

AWS Trusted Advisor is a **real-time guidance tool** that helps you follow **AWS best practices**.
It acts like a **cloud consultant**, continuously analyzing your AWS environment and giving **recommendations** to improve:

* **Cost Optimization** 💰
* **Performance** ⚡
* **Security** 🔒
* **Fault Tolerance** 🔄
* **Service Limits** 📊

---

## ⚡ When Do We Use It in Real-Time?

You use Trusted Advisor in **day-to-day cloud operations** to ensure your AWS environment is healthy, secure, and cost-efficient.

### ✅ Real-Time Scenarios:

* **Cost Saving**: Detect unused or underutilized EC2, RDS, or EBS and shut them down.
* **Security Monitoring**: Get alerts for open S3 buckets or unused IAM keys.
* **Operational Health**: Ensure databases have backups and are running in Multi-AZ.
* **Limit Management**: Avoid service outages by monitoring AWS resource limits.
* **Performance Tuning**: Right-size your EC2 instances to avoid over/under provisioning.

---

## 🛠️ How to Use AWS Trusted Advisor

### 1. **Access Trusted Advisor**

* Go to the **AWS Management Console** → search **“Trusted Advisor”**.
* Or use the **AWS CLI / SDK** for programmatic access.

### 2. **Run Checks**

* AWS runs **various checks** grouped under categories (Cost, Security, Performance, etc.).

---

## 🔍 Real-Time Example 1: Check if S3 Buckets are Public (Security)

* **Console**: Trusted Advisor → Security → **S3 Bucket Permissions Check**.
* **CLI**:

  ```bash
  aws s3api get-bucket-acl --bucket my-bucket-name
  ```

  If `"AllUsers"` with `READ/WRITE` → Bucket is public → Restrict it.

---

## 🔍 Real-Time Example 2: Detect Idle EC2 Instances (Cost Optimization)

* **Console**: Trusted Advisor → Cost Optimization → **Low Utilization EC2 Check**.

* Flags instances with **<10% CPU usage** for 14 days.

* **CLI** (check CPU utilization):

  ```bash
  aws cloudwatch get-metric-statistics \
      --metric-name CPUUtilization \
      --start-time 2025-09-01T00:00:00Z \
      --end-time 2025-09-12T23:59:59Z \
      --period 86400 \
      --namespace AWS/EC2 \
      --statistics Average \
      --dimensions Name=InstanceId,Value=i-0a12345abcde
  ```

  If consistently <10% → Instance is idle → Stop/terminate/downgrade.

---

## 🔍 Real-Time Example 3: Enable Multi-AZ for RDS (Fault Tolerance)

### Why?

If your RDS database is deployed in **single-AZ**, it’s a **single point of failure**. If that AZ goes down, your DB is unavailable.

### Using **AWS Management Console**

1. Go to **Trusted Advisor** → **Fault Tolerance** category.
2. Run **Amazon RDS Multi-AZ Check**.
3. Trusted Advisor will list DB instances that are **not Multi-AZ**.

👉 Example Result:

* DB instance `mydb-prod` flagged as **Single-AZ**.

**Action:**

* Modify RDS instance → Enable **Multi-AZ Deployment** → AWS automatically provisions a standby in another AZ.

### Using **AWS CLI**

```bash
aws rds describe-db-instances \
    --query 'DBInstances[*].{DB:DBInstanceIdentifier,MultiAZ:MultiAZ}'
```

👉 Example Output:

```json
[
  {
    "DB": "mydb-prod",
    "MultiAZ": false
  }
]
```

🚨 **Problem**: `MultiAZ` = false → Database has no failover.
✅ **Fix**: Modify the DB and enable **Multi-AZ**.

---

## 📊 Free vs Paid Checks

* **Free (all AWS accounts)** → Security & Service Limits.
* **Paid (Business/Enterprise Support Plans)** → Full set of 200+ checks (Cost Optimization, Performance, Fault Tolerance).

---

## ✅ Summary

* **Security Example** → Restrict public **S3 buckets**. 🔒
* **Cost Example** → Stop/resize **idle EC2 instances**. 💰
* **Fault Tolerance Example** → Enable **Multi-AZ for RDS** to avoid downtime. 🔄

---

Do you want me to also add **Performance example** (like checking service limits or right-sizing EC2) so you’ll have all **five categories** (Cost, Security, Performance, Fault Tolerance, Service Limits) covered?
