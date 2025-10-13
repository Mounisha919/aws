Got it — here’s how to turn **off Enhanced Security Configuration (ESC)** (usually the Internet Explorer ESC that Windows Server enables by default). I’ll give the simple GUI method (recommended) and a PowerShell script for automation, plus safety notes.

---

# Turn off IE Enhanced Security Configuration — GUI (recommended)

1. Sign in to the server as an **Administrator**.
2. Open **Server Manager** (Start → Server Manager).
3. Click **Local Server** in the left pane.
4. In the right-hand **Properties** pane find **IE Enhanced Security Configuration** (it will say **On**). Click the **On** link.
5. In the dialog that opens, set **Administrators** and/or **Users** to **Off** (choose according to what you need) and click **OK**.
6. Close all Internet Explorer windows and restart IE (or reboot if needed).

(Works on Windows Server 2012 / 2016 / 2019 / 2022; AWS image builders and other docs show the same GUI flow.) ([community.denodo.com][1])

---

# Turn off IE ESC — PowerShell (automatable)

Run PowerShell **as Administrator**. This common script toggles the ActiveSetup “IsInstalled” keys for IE ESC:

```powershell
# disable IE Enhanced Security Configuration (ESC)
function Disable-InternetExplorerESC {
    Write-Host "Disabling IE Enhanced Security Configuration (ESC)..."
    $AdminKey = "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}"
    $UserKey  = "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}"
    Set-ItemProperty -Path $AdminKey -Name "IsInstalled" -Value 0 -ErrorAction SilentlyContinue
    Set-ItemProperty -Path $UserKey  -Name "IsInstalled" -Value 0 -ErrorAction SilentlyContinue
    Stop-Process -Name explorer -Force
}
Disable-InternetExplorerESC
```

• Save as a `.ps1` and run as admin. This script is commonly used in admin scripts and examples; test in a lab before rolling out broadly. ([en.wikiversity.org][2])

---

# Older Windows Server (if Server Manager not available)

On very old versions you may need to go through **Control Panel → Add/Remove Programs → Add/Remove Windows Components** and uncheck the “Internet Explorer Enhanced Security Configuration” component — then finish the wizard. ([NinjaOne][3])

---

# Verify it’s off

Open **Server Manager → Local Server** and confirm the IE ESC property shows **Off**, or open IE and try the site that was previously blocked.

---

# Important security notes (please read)

* **Disabling IE ESC reduces server security.** Only turn it off temporarily or when you absolutely must (e.g., to download a newer browser or access an internal admin web UI). ([NinjaOne][3])
* Safer alternatives: add required admin sites to **Trusted Sites / Local intranet** zones, or install a modern browser (Edge/Chrome/Firefox) on the server and keep IE ESC **On**. ([Prajwal Desai][4])
* If you disable ESC centrally (GPO or image build), document and control it — don’t leave it disabled on production servers unnecessarily. ([Reddit][5])

---

If you want, I can:

* Give a one-liner PowerShell that reports the current ESC state on many servers, or
* Provide a Group Policy / registry method to manage this centrally (useful if you manage many servers).

Which of those would help you right now?

[1]: https://community.denodo.com/docs/html/browse/8.0/en/platform/installation/postinstallation_tasks/postinstallation_tasks_in_itpilot/disabling_internet_explorer_enhanced_security_configuration_in_microsoft_windows_server_2012?utm_source=chatgpt.com "Disabling Internet Explorer Enhanced Security ..."
[2]: https://en.wikiversity.org/wiki/PowerShell/Examples/Disable-InternetExplorerESC "PowerShell/Examples/Disable-InternetExplorerESC - Wikiversity"
[3]: https://www.ninjaone.com/blog/disable-internet-explorer-enhanced-security/?utm_source=chatgpt.com "Disable Internet Explorer Enhanced Security"
[4]: https://www.prajwaldesai.com/disable-ie-enhanced-security-on-windows-server/?utm_source=chatgpt.com "2 Best Ways to Disable IE Enhanced Security on Windows Server"
[5]: https://www.reddit.com/r/sysadmin/comments/12l4sub/central_management_of_ie_enhanced_security/?utm_source=chatgpt.com "Central Management of IE Enhanced Security ..."
---
Excellent question 👍 — this is **a common AWS admin ask**:
“How can I log in to my EC2 instance without using the key pair (`.pem` file) every time?”

Let’s go step-by-step — and I’ll cover **both Linux** and **Windows** instances.

---

## 🔐 Why you need the key pair in the first place

When you create an EC2 instance:

* **Linux:** AWS creates a user (like `ec2-user`, `ubuntu`, etc.) and puts the **public key** from your key pair into `~/.ssh/authorized_keys`.
* You then log in with the **private key (.pem)** file using SSH.

To avoid needing the key every time, you can **add a password** or **set up persistent user credentials** (like SSH or SSM).

---

# 🧩 Option 1: Use AWS Systems Manager (SSM Session Manager) — **Recommended**

✅ **No key pair, no SSH port, most secure option**

### Steps:

1. Go to the **EC2 instance** → **IAM Role** attached to it.

   * Ensure the role has **AmazonSSMManagedInstanceCore** policy.
2. In the EC2 instance’s **Security Group**, you can even **close port 22 (SSH)**.
3. Install the **SSM Agent** (it’s preinstalled on Amazon Linux 2 and Ubuntu 22+).
4. In the AWS Console:

   * Go to **Systems Manager → Session Manager → Start session**
   * Select your instance and click **Start session**.

You’ll get a **terminal directly in the browser**, or you can use the **AWS CLI**:

```bash
aws ssm start-session --target i-xxxxxxxxxxxxxxxxx
```

🎯 **Advantages:**

* No `.pem` key required
* Secure, auditable, works through IAM permissions
* You can control who logs in via IAM policies

---

# 💻 Option 2: Set up password-based SSH login (less secure, but common in dev)

If you prefer traditional SSH without keypair:

### On Linux instance

1. SSH in **once** using the `.pem` key:

   ```bash
   ssh -i mykey.pem ec2-user@<public_ip>
   ```

2. Set a password for the user:

   ```bash
   sudo passwd ec2-user
   ```

3. Edit the SSH configuration:

   ```bash
   sudo vi /etc/ssh/sshd_config
   ```

   Change:

   ```
   PasswordAuthentication no
   ```

   to:

   ```
   PasswordAuthentication yes
   ```

4. Restart SSH service:

   ```bash
   sudo systemctl restart sshd
   ```

5. Now you can log in **using username and password**:

   ```bash
   ssh ec2-user@<public_ip>
   ```

⚠️ **Important:**

* Not recommended for production servers.
* Use **strong passwords** and **restrict access via Security Group (IP whitelisting)**.

---

# 🪟 Option 3: For Windows EC2 instances

1. Connect once using the **RDP password** retrieved from the key pair.
2. Inside Windows, create a new **local user**:

   ```
   Computer Management → Local Users and Groups → Users → New User
   ```
3. Add that user to the **Administrators** group.
4. Now you can log in via **RDP** with username/password — no key pair needed.

---

# ⚙️ Option 4: Create a new user with its own key (for multiple users)

If you have a team or want permanent SSH access for your own system:

```bash
sudo adduser mounisha
sudo mkdir /home/mounisha/.ssh
sudo cp ~/.ssh/authorized_keys /home/mounisha/.ssh/
sudo chown -R mounisha:mounisha /home/mounisha/.ssh
```

→ Now you can SSH using your system’s key or password.

---

# 🛡️ Summary

| Method                  | Key Needed?  | Security   | Recommended for           |
| ----------------------- | ------------ | ---------- | ------------------------- |
| **SSM Session Manager** | ❌            | ✅✅✅ (Best) | Prod, secure environments |
| **Password-based SSH**  | ❌            | ⚠️ Medium  | Dev/test                  |
| **Extra SSH user key**  | ✅ (your own) | ✅          | Teams                     |
| **Windows user login**  | ❌            | ✅          | Windows servers           |

---

Would you like me to show **the easiest permanent method for your case** (Linux or Windows instance)?
That way, I can tailor exact commands and setup.
