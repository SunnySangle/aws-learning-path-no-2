How to launch AWS Linux AMI EC2 Instance

Let’s go through the process of **launching a website on an AWS Linux AMI EC2 instance** step by step! 🚀

---

## 🌐 **Step 1: Connect to your EC2 instance**

1. Go to the **AWS Management Console**.
2. Navigate to **EC2 Dashboard** and find your instance.
3. Copy the **Public IPv4 address**.
4. Use SSH to connect:

```bash

Open an SSH client.

Locate your private key file. The key used to launch this instance is linux-key.pem

Run this command, if necessary, to ensure your key is not publicly viewable.
chmod 400 "linux-key.pem"

Connect to your instance using its Public DNS:
ec2-54-151-178-14.ap-southeast-1.compute.amazonaws.com

Example:

ssh -i "linux-key.pem" ec2-user@ec2-54-151-178-14.ap-southeast-1.compute.amazonaws.com

```

---

## 📦 **Step 2: Update packages**

Update the package manager:

```bash
sudo yum update -y

```

---

## 🏗️ **Step 3: Install and start Apache (httpd)**

Install the Apache web server:

```bash

sudo yum install httpd -y

```

Start Apache and enable it to launch on boot:

```bash

sudo systemctl start httpd
sudo systemctl enable httpd

```

---

---

## 📂 **Step 4: Upload your website files**

1. Navigate to the default Apache root directory:

```bash

cd /var/www/html

```

1. **Create a simple index.html**:

```bash

sudo nano index.html

```

1. **Add sample content:**

```html
html
CopyEdit
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to My AWS EC2 Website</title>
</head>
<body>
    <h1>Hello World! This is my website hosted on AWS EC2.</h1>
</body>
</html>

```

1. **Save the file** (Ctrl + X, Y, Enter).

---

## 🌐 **Step 5: Test your website**

1. Go to your browser and type:

```
http://54.151.178.14
```

1. You should see your "**Hello World! This is my website hosted on AWS EC2**" page.

---
