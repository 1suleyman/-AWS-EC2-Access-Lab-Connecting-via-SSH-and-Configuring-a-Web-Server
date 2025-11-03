# 🔐 AWS EC2 Access Lab – Connecting via SSH and Configuring a Web Server

In this lab, I learned how to **securely access an EC2 instance using SSH**, manage key pair permissions, and perform server-side configurations such as **installing and customizing an NGINX web page** directly from the terminal.

---

## 📋 Lab Overview

**Goal:**

* Launch and connect to an EC2 instance using SSH
* Understand key pair–based authentication
* Install and start an NGINX web server
* Modify and verify the default web page served by the instance

**Learning Outcomes:**

* Use SSH commands with `.pem` key files
* Adjust key file permissions for secure access
* Install and manage packages using `yum`
* Edit files in Linux using `vi`
* Verify public web content hosted on an EC2 instance

---

## 🛠 Step-by-Step Journey

### Step 1 – Log In to AWS Console

Used the credentials provided in the lab to sign into the **AWS Management Console**.
✅ Successfully logged in.

---

### Step 2 – Understand SSH Access

**Concept:**
SSH (Secure Shell) provides secure, encrypted remote access to EC2 instances.
AWS uses **key pairs** for password-less authentication.

Example syntax:

```bash
ssh -i <path-to-key>/ec2-kk-key.pem ec2-user@<public-ip>
```

✅ Key pair used: `ec2-kk-key.pem`

---

### Step 3 – Launch EC2 Instance

**Configuration Details:**

| Setting        | Value           |
| -------------- | --------------- |
| Name           | `ec2-codecloud` |
| AMI            | Amazon Linux 2  |
| Instance Type  | `t2.micro`      |
| Key Pair       | `ec2-kk-key`    |
| Subnet         | us-east-1c      |
| Public IP      | Enabled         |
| Security Group | `codecloud-sg`  |
| Storage        | 10 GiB gp2      |
| Count          | 1               |

✅ Instance launched successfully.

---

### Step 4 – Verify Instance Details

* **Instance ID:** ends in `280`
* **Public IP:** `54.147.64.110`

✅ Confirmed instance details via EC2 Console.

---

### Step 5 – Connect to EC2 via SSH

**Step 1 – Fix Permissions**

```bash
chmod 400 ~/Downloads/ec2-kk-key.pem
```

**Step 2 – Connect Using SSH**

```bash
ssh -i ~/Downloads/ec2-kk-key.pem ec2-user@54.147.64.110
```

✅ Connection successful.
Once connected, retrieved host information:

```bash
hostname
```

Output:

```
ip-172-31-2-0.ec2.internal
```

---

### Step 6 – Install and Start NGINX

**Update system and install NGINX**

```bash
sudo yum update -y
sudo yum install nginx -y
```

**Start NGINX service**

```bash
sudo service nginx start
```

✅ Web server started successfully.

---

### Step 7 – Verify Default NGINX Page

Opened the public IP in a browser:

```
http://54.147.64.110
```

✅ Default NGINX page displayed: *“Welcome to NGINX.”*

---

### Step 8 – Modify the Default Web Page

**Location of default HTML file:**

```
/usr/share/nginx/html/index.html
```

**Edit the file:**

```bash
sudo vi /usr/share/nginx/html/index.html
```

Replace the content with:

```html
<html>
  <body>
    <h1>Welcome to CodeCloud Labs</h1>
  </body>
</html>
```

Save and exit:

```
ESC :wq
```

✅ File updated successfully.

---

### Step 9 – Verify Modified Web Page

Reloaded in browser:

```
http://54.147.64.110
```

✅ New page displayed:

> **Welcome to CodeCloud Labs**

---

### Step 10 – Exit SSH Session

To close the remote connection:

```bash
exit
```

✅ Connection closed cleanly.

---

## 🏁 End of Lab

### ✅ Key Commands Summary

| Task                   | Command                                                  |
| ---------------------- | -------------------------------------------------------- |
| Change key permissions | `chmod 400 ~/Downloads/ec2-kk-key.pem`                   |
| SSH into instance      | `ssh -i ~/Downloads/ec2-kk-key.pem ec2-user@<public-ip>` |
| View hostname          | `hostname`                                               |
| Update packages        | `sudo yum update -y`                                     |
| Install NGINX          | `sudo yum install nginx -y`                              |
| Start NGINX            | `sudo service nginx start`                               |
| Edit web page          | `sudo vi /usr/share/nginx/html/index.html`               |
| Exit SSH session       | `exit`                                                   |

---

### 💡 Notes / Tips

* `.pem` key files **must have restrictive permissions** (`chmod 400`) before use.
* The default EC2 Linux user is **`ec2-user`** for Amazon Linux.
* `sudo` is required for installing or modifying system files.
* Use `vi` commands like `dd`, `i`, and `:wq` for editing files directly in the terminal.
* Always **terminate instances** when done to avoid unnecessary charges.

---

### ✅ References

* [AWS EC2 SSH Access Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
* [NGINX Installation on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html)
* [Managing Key Pairs in AWS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
