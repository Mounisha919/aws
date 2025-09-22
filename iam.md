In **AWS**, an **Account ID Alias** is a friendly name that you can assign to your AWS account to make it easier to reference instead of using the 12-digit numeric **AWS Account ID**.

Here are the key points:

1. **AWS Account ID vs Account Alias**

   * **AWS Account ID:** A 12-digit number automatically assigned to your account (e.g., `123456789012`).
   * **Account Alias:** A human-readable string you create (e.g., `my-company-prod`) that can be used in the AWS Management Console URL for easier access.

2. **Purpose**

   * Makes URLs easier to remember, e.g., instead of:

     ```
     https://123456789012.signin.aws.amazon.com/console
     ```

     you can use:

     ```
     https://my-company-prod.signin.aws.amazon.com/console
     ```
   * Helps users identify the account more easily in multi-account setups.

3. **Creating or Modifying an Alias**

   * You can set an account alias through the **AWS Management Console**:
     `IAM → Dashboard → Account Alias → Create`
   * Or via **AWS CLI**:

     ```bash
     aws iam create-account-alias --account-alias my-company-prod
     ```
   * To delete an alias:

     ```bash
     aws iam delete-account-alias --account-alias my-company-prod
     ```

4. **Rules for Alias**

   * Must be unique across AWS globally.
   * Can include letters, numbers, hyphens (`-`).
   * Cannot start or end with a hyphen.

5. **Viewing your current Alias**

   ```bash
   aws iam list-account-aliases
   ```
In **AWS**, setting a **password policy** allows you to enforce rules for IAM users’ passwords, such as minimum length, complexity, expiration, and reuse prevention. This improves security by ensuring strong passwords. Here’s a detailed breakdown:

---

### 1️⃣ **Where to Set a Password Policy**

* **AWS Management Console:**
  `IAM → Account settings → Password policy`
* **AWS CLI:**
  You can use the `aws iam update-account-password-policy` command.

---

### 2️⃣ **Password Policy Options**

You can configure these options:

| Option                                    | Description                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| **Minimum password length**               | Set minimum characters (default 6, recommended ≥8).           |
| **Require at least one uppercase letter** | Ensures passwords have at least one capital letter.           |
| **Require at least one lowercase letter** | Ensures passwords have at least one small letter.             |
| **Require at least one number**           | Ensures passwords contain digits (0-9).                       |
| **Require at least one symbol**           | Ensures passwords include special characters like `!@#$%^&*`. |
| **Allow users to change password**        | Enable IAM users to update their own password.                |
| **Require password reset**                | Forces users to reset their password on next sign-in.         |
| **Prevent password reuse**                | Users cannot reuse last N passwords.                          |
| **Set password expiration**               | Force password change after N days.                           |

---

### 3️⃣ **Example: Setting a Password Policy via CLI**

```bash
aws iam update-account-password-policy \
    --minimum-password-length 12 \
    --require-symbols \
    --require-numbers \
    --require-uppercase-characters \
    --require-lowercase-characters \
    --allow-users-to-change-password \
    --max-password-age 90 \
    --password-reuse-prevention 5
```

This policy:

* Requires 12+ character passwords
* Must have uppercase, lowercase, numbers, and symbols
* Forces users to reset password every 90 days
* Prevents reuse of last 5 passwords

---

### 4️⃣ **Viewing Current Password Policy**

```bash
aws iam get-account-password-policy
```

---

### 5️⃣ **Best Practices**

* Enforce strong, complex passwords (uppercase, lowercase, numbers, symbols).
* Rotate passwords regularly (every 60–90 days).
* Enable multi-factor authentication (MFA) for better security.
* Prevent reuse of previous passwords to reduce risk.

---


