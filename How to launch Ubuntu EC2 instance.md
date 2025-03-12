# How to create First EC2 Instance

How to launch Ubuntu EC2 Instance

- click on Launch instance
- Name of instance
- Choose Application and OS Image(Amazon Machine Image) : Ubuntu
- Architecture: 64 bit x 86
- Instance type: t2.micro
- Select key pair or create new one
- Network setting : keep as it is VPC , Subnet , Auto Assign public IP enable
- Create or select Security Group

Security Group: SSH Port 20

Security Group HTTP Port 80

- Launch instance

Let’s break down how to **launch a website on an EC2 Ubuntu instance** — step by step! 🚀

---

### 🌐 **Step 1: Connect to your EC2 instance**

Use SSH to access your instance:

```bash
Open an SSH client.

Locate your private key file. The key used to launch this instance is Ubuntu-key.pem

Run this command, if necessary, to ensure your key is not publicly viewable.
chmod 400 "Ubuntu-key.pem"

Connect to your instance using its Private IP:
172.31.30.131

Example:

ssh -i "Ubuntu-key.pem" ubuntu@172.31.30.131
```

---

### 🏗️ **Step 2: Install Apache (or NGINX)**

For a simple web server, let’s use Apache:

```bash

sudo apt update
sudo apt install apache2 -y

```

Start and enable Apache:

```bash

sudo systemctl start apache2
sudo systemctl enable apache2

```

---

### 📂 **Step 3: Upload your website files**

Your website files (HTML, CSS, JavaScript) go into Apache’s root directory:

```bash

cd /var/www/html

```

You can use tools like **scp** to upload files from your local machine:

```bash

scp -i your-key.pem index.html ubuntu@your-ec2-public-ip:/var/www/html/

```

Or manually create a simple **index.html** to test:

```bash
sudo nano /var/www/html/index.html
sudo rm /var/www/html/index.html
sudo touch /var/www/html/index.html
sudo nano /var/www/html/index.html

```

Add a basic HTML file:

```html
html
CopyEdit
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My EC2 Website</title>
</head>
<body>
    <h1>Hello from EC2!</h1>
</body>
</html>

```

Save and exit (**Ctrl + X, Y, Enter**).

---

### 🔥 **Step 4: Adjust permissions**

Ensure Apache can serve your files:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

```

---

### 🌐 **Step 5: Allow HTTP/HTTPS traffic**

Make sure your EC2 security group allows traffic:

1. Go to the **EC2 Dashboard**.
2. Select your instance.
3. Click on the **Security** tab → **Edit inbound rules**.
4. Add rules:
    - **HTTP (80)** — Anywhere (0.0.0.0/0)
    - **HTTPS (443)** — Anywhere (0.0.0.0/0)

---

### 🌍 **Step 6: Test your website**

Now, open your browser and go to:

```
http://18.139.0.107

```

You should see your "Hello from EC2!" page.
