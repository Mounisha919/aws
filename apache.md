Perfect! Let’s go **step by step** on how to make your **EC2 instance (VM) serve a webpage** — for both **Linux** and **Windows**. This is exactly what you’d do in real life or in the AWS Solutions Architect Associate exam practical scenario.

---

# 🌐 How to Make Your EC2 Instance a Web Server

---

## **Step 1: Launch an EC2 Instance**

1. Go to **AWS Console → EC2 → Launch Instance**
2. Select **AMI**:

   * Linux: Amazon Linux 2 / Ubuntu
   * Windows: Windows Server
3. Choose **Instance Type**: `t2.micro` (free tier) is fine.
4. Configure Network:

   * Make sure **auto-assign public IP** is enabled
   * Use default VPC or custom VPC
5. Configure Security Group:

   * **Inbound rules:**

     * Linux: TCP 22 (SSH) from your IP
     * Windows: TCP 3389 (RDP) from your IP
     * HTTP: TCP 80 from 0.0.0.0/0 (to allow anyone to access the webpage)
   * Optional HTTPS: TCP 443
6. Launch instance with **Key Pair** (for SSH/RDP access)

---

## **Step 2: Connect to Your Instance**

* **Linux:** SSH

  ```bash
  ssh -i "your-key.pem" ec2-user@<EC2-Public-IP>
  ```
* **Windows:** RDP

  * Use `mstsc` → enter public IP → login with Administrator password (retrieved via key pair)

---

## **Step 3: Install Web Server**

### **A) Linux (Apache)**

```bash
# Update packages
sudo yum update -y   # Amazon Linux
sudo apt update -y   # Ubuntu

# Install Apache
sudo yum install httpd -y   # Amazon Linux
sudo apt install apache2 -y # Ubuntu

# Start the service
sudo systemctl start httpd
sudo systemctl enable httpd
```

### **B) Windows (IIS or Apache)**

* **Option 1: IIS (built-in web server)**

  1. Go to **Server Manager → Add Roles and Features → Web Server (IIS)**
  2. Install and start the IIS service
  3. Default document root: `C:\inetpub\wwwroot`
* **Option 2: Apache (manual install)**

  1. Download Apache Windows binaries ([Apache Lounge](https://www.apachelounge.com/download/))
  2. Extract → Install as service → Start service
  3. Document root: `C:\Apache24\htdocs`

---

## **Step 4: Create Your Web Pages**

### **Linux**

```bash
cd /var/www/html

# Create homepage
sudo nano index.html
```

```html
<html>
  <body>
    <h1>Welcome to My Website</h1>
  </body>
</html>
```

```bash
# Optional second page
sudo nano about.html
```

```html
<html>
  <body>
    <h1>About My Website</h1>
  </body>
</html>
```

### **Windows (IIS or Apache)**

* IIS: `C:\inetpub\wwwroot\index.html` & `about.html`
* Apache: `C:\Apache24\htdocs\index.html` & `about.html`

---

## **Step 5: Configure Security Group for HTTP**

* EC2 → Security Groups → Inbound → Add rule:

  * Type: HTTP
  * Protocol: TCP
  * Port: 80
  * Source: 0.0.0.0/0
* Optional: HTTPS → TCP 443

> Without this step, your public IP won’t serve web pages.

---

## **Step 6: Access Your Web Page**

* Open browser → `http://<EC2-Public-IP>/index.html`
* Second page → `http://<EC2-Public-IP>/about.html`

> Tip: Default Apache/IIS welcome page is served if you visit `http://<EC2-Public-IP>` without specifying a file.

---

## **Step 7: Optional — Configure Custom Domain**

1. Buy a domain (Route53 or any provider)
2. Create A record pointing to your EC2 public IP
3. Access your web page using `http://yourdomain.com`

---

## **Step 8: Optional — Enable HTTPS**

* Use **AWS Certificate Manager (ACM)** + **Elastic Load Balancer**
* Or manually configure **self-signed certificate** in Apache/IIS

---

## **Step 9: Persistence & Best Practices**

* Always **enable auto-start** of Apache/IIS on reboot
* Use **Elastic IP** instead of changing public IP (if instance restarts)
* Restrict SSH/RDP access to your IP
* Monitor traffic via **CloudWatch / VPC Flow Logs**

---

## ✅ **Summary Table**

| Step | Action                         | Purpose                  |
| ---- | ------------------------------ | ------------------------ |
| 1    | Launch EC2                     | Create VM with public IP |
| 2    | Connect (SSH/RDP)              | Access instance          |
| 3    | Install Apache/IIS             | Install web server       |
| 4    | Create index.html & about.html | Web pages content        |
| 5    | Configure SG HTTP/HTTPS        | Allow public access      |
| 6    | Access via browser             | Test website             |
| 7    | Optional: Domain               | Access with friendly URL |
| 8    | Optional: HTTPS                | Secure connection        |
| 9    | Enable service auto-start      | Persistent web server    |

---

If you want, I can **make a full lab-style step-by-step AWS guide** where you:

1. Launch an EC2
2. Install Apache/IIS
3. Create two web pages
4. Configure security groups
5. Access from public IP

…all **with screenshots and commands exactly like a Solutions Architect exam scenario**.

Do you want me to do that next?
Perfect! Let’s cover **Apache HTTP Server (httpd) on AWS EC2 Windows/Linux** in complete detail, including installation, creating web pages, configuration, and access. I’ll explain it in a **step-by-step, exam-ready, 5-year experience level** style.

---

# 🌐 Apache HTTP Server — Complete Guide

Apache HTTP Server (httpd) is the **most popular web server**, used to serve web pages over HTTP/HTTPS. In AWS, it’s commonly deployed on EC2 instances.

---

## **1) Apache Key Concepts**

| Concept                | Explanation                                                         |
| ---------------------- | ------------------------------------------------------------------- |
| **httpd**              | Apache server daemon/service                                        |
| **Document Root**      | Directory where website files are stored (`/var/www/html` on Linux) |
| **Port**               | Default HTTP port 80, HTTPS 443                                     |
| **Configuration File** | Apache settings (`httpd.conf` or `/etc/httpd/conf/httpd.conf`)      |
| **Virtual Hosts**      | Allows multiple websites/domains on a single server                 |

---

## **2) Install Apache HTTP Server**

### **A) On Linux (Amazon Linux / Ubuntu / CentOS)**

**Amazon Linux 2 / CentOS**

```bash
# Update packages
sudo yum update -y

# Install Apache
sudo yum install httpd -y

# Start Apache
sudo systemctl start httpd

# Enable auto-start on boot
sudo systemctl enable httpd

# Check status
sudo systemctl status httpd
```

**Ubuntu / Debian**

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

---

### **B) On Windows EC2**

1. Download Apache HTTP Server for Windows: [Apache Lounge](https://www.apachelounge.com/download/)
2. Extract to `C:\Apache24\`
3. Open **Command Prompt as Admin**
4. Install as service:

```cmd
cd C:\Apache24\bin
httpd.exe -k install
httpd.exe -k start
```

5. Open `http://localhost` in browser to verify.

---

## **3) Configure Security Groups to Allow HTTP/HTTPS**

* **Inbound Rule:** TCP, port 80 (HTTP) and 443 (HTTPS), source `0.0.0.0/0` (public web access)
* If only your IP should access: source = your public IP `/32`.

---

## **4) Default Document Root**

| OS      | Document Root        |
| ------- | -------------------- |
| Linux   | `/var/www/html`      |
| Windows | `C:\Apache24\htdocs` |

All web pages go inside this folder.

---

## **5) Create Two Web Pages**

### **A) Linux Example**

```bash
# Go to document root
cd /var/www/html

# Create index.html
sudo nano index.html
```

```html
<html>
  <head><title>Home Page</title></head>
  <body>
    <h1>Welcome to my Apache Server!</h1>
  </body>
</html>
```

```bash
# Create about.html
sudo nano about.html
```

```html
<html>
  <head><title>About Page</title></head>
  <body>
    <h1>About My Website</h1>
    <p>This is the about page.</p>
  </body>
</html>
```

### **B) Windows Example**

* Save `index.html` and `about.html` in `C:\Apache24\htdocs\`.

---

## **6) Access the Web Pages**

1. Open browser and enter EC2 public IP or domain:

   * Home page: `http://<EC2-Public-IP>/index.html`
   * About page: `http://<EC2-Public-IP>/about.html`

> Tip: For default page, `http://<EC2-Public-IP>` automatically serves `index.html`.

---

## **7) Change Apache Configuration**

### **A) Configuration File**

* Linux: `/etc/httpd/conf/httpd.conf` (CentOS/Amazon Linux), `/etc/apache2/apache2.conf` (Ubuntu)
* Windows: `C:\Apache24\conf\httpd.conf`

### **B) Common Configurations**

| Setting         | Example / Purpose                                 |
| --------------- | ------------------------------------------------- |
| DocumentRoot    | Change folder where web files reside              |
| Listen          | Change HTTP port (default 80)                     |
| ServerName      | Set domain/IP name                                |
| Directory Index | Change default page order (index.html, home.html) |
| Enable modules  | LoadModule ssl_module modules/mod_ssl.so          |

**Example: Change Document Root**

```conf
DocumentRoot "/var/www/mywebsite"
<Directory "/var/www/mywebsite">
    AllowOverride All
    Require all granted
</Directory>
```

* After any change, **restart Apache**:

```bash
sudo systemctl restart httpd    # Linux
httpd.exe -k restart             # Windows
```

---

## **8) Enable Virtual Hosts (Optional)**

* To host multiple websites on one EC2 instance.
* Linux Example:

```conf
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/example
</VirtualHost>

<VirtualHost *:80>
    ServerName test.com
    DocumentRoot /var/www/test
</VirtualHost>
```

* Windows: similar changes in `httpd-vhosts.conf`.
* Restart Apache after configuration.

---

## **9) Logs**

| Log        | Location                                                                   | Purpose                   |
| ---------- | -------------------------------------------------------------------------- | ------------------------- |
| Access log | Linux: `/var/log/httpd/access_log`, Windows: `C:\Apache24\logs\access.log` | Records all HTTP requests |
| Error log  | Linux: `/var/log/httpd/error_log`, Windows: `C:\Apache24\logs\error.log`   | Records server errors     |

> Exam Tip: “Why is my web page not loading?” → Check security group, Apache running status, and error logs.

---

## **10) Commands Summary**

| Action         | Linux                          | Windows                |
| -------------- | ------------------------------ | ---------------------- |
| Start          | `sudo systemctl start httpd`   | `httpd.exe -k start`   |
| Stop           | `sudo systemctl stop httpd`    | `httpd.exe -k stop`    |
| Restart        | `sudo systemctl restart httpd` | `httpd.exe -k restart` |
| Enable at boot | `sudo systemctl enable httpd`  | `httpd.exe -k install` |

---

## **11) Security Best Practices**

1. Open only required ports (80/443) in SG.
2. Keep Apache up-to-date.
3. Use HTTPS (mod_ssl + certificate from ACM or Let’s Encrypt).
4. Restrict sensitive files with `.htaccess`.
5. Avoid running as root — use `apache`/`www-data` user.

---

## ✅ **12) Quick Exam / Interview Tips**

* Apache = `httpd` service.
* Default port = 80 (HTTP), 443 (HTTPS).
* Security Group must allow inbound port 80/443.
* DocumentRoot is `/var/www/html` (Linux), `C:\Apache24\htdocs` (Windows).
* Create pages → access via `http://<Public-IP>/index.html`.
* Restart service after configuration changes.
* Use Virtual Hosts for multiple websites.

---

If you want, I can **create a step-by-step lab guide with screenshots** showing:

1. EC2 launch → Install Apache
2. Security group setup
3. Creating two web pages
4. Accessing them from public IP
5. Modifying configuration and restarting

This will be **exactly like a real exam / practical scenario**.

Do you want me to do that next?
