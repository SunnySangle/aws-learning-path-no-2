[Screenshot 2025-03-06 180504.png](https://www.notion.so/1b45e020b74f808f90fcd2e8c5b8477e?pvs=21)

Let’s go step by step on how to set up a **VPC with public and private subnets** on AWS EC2 and configure a **NAT Gateway** for private subnets so they can access the internet securely. Let’s roll! 🚀

---

## 🌐 **Step 1: Create a VPC**

1. Go to the **AWS Management Console**.
2. Search for **VPC** and open the **VPC Dashboard**.
3. Click **Create VPC**.
4. Configure the VPC:
    - **Name:** `MyVPC`
    - **IPv4 CIDR block:** `10.0.0.0/16`
    - Leave other options as default.
5. Click **Create VPC**.

---

## 📦 **Step 2: Create public and private subnets**

1. In the **VPC Dashboard**, go to **Subnets** → **Create Subnet**.
2. **For Public Subnet:**
    - **Name:** `PublicSubnet`
    - **VPC ID:** Select your newly created VPC.
    - **Availability Zone:** Choose an AZ (e.g., `us-east-1a`).
    - **IPv4 CIDR block:** `10.0.1.0/24`
    - Click **Create Subnet**.
3. **For Private Subnet:**
    - **Name:** `PrivateSubnet`
    - **VPC ID:** Select your VPC.
    - **Availability Zone:** Choose the same or another AZ (e.g., `us-east-1b`).
    - **IPv4 CIDR block:** `10.0.2.0/24`
    - Click **Create Subnet**.

---

## 🌍 **Step 3: Set up an Internet Gateway (for public access)**

1. Go to **Internet Gateways** → **Create Internet Gateway**.
2. **Name:** `MyInternetGateway`
3. Click **Create Internet Gateway**.
4. **Attach it to the VPC:**
    - Select your **VPC** → **Attach internet gateway**.

---

## 🔥 **Step 4: Configure route tables**

1. Go to **Route Tables** → **Create route table**.
2. **For the Public Subnet:**
    - **Name:** `PublicRouteTable`
    - **VPC:** Select your **VPC**.
    - Click **Create**.
3. **Add route to the Internet:**
    - Select your new **PublicRouteTable**.
    - Click on the **Routes tab** → **Edit routes** → **Add route**.
    - **Destination:** `0.0.0.0/0`
    - **Target:** Select your **Internet Gateway**.
    - Click **Save changes**.
4. **Associate with Public Subnet:**
    - Go to the **Subnet associations** tab.
    - **Edit subnet associations**.
    - Select your **PublicSubnet** → **Save**.

---

## 🏡 **Step 5: Set up a NAT Gateway for Private Subnets**

1. Go to **NAT Gateways** → **Create NAT Gateway**.
2. **Name:** `MyNATGateway`
3. **Subnet:** Select your **PublicSubnet**.
4. **Elastic IP allocation:**
    - Click **Allocate Elastic IP** → **Create NAT Gateway**.
5. **Update route table for Private Subnet:**
    - Go to **Route Tables** → **Create route table**.
    - **Name:** `PrivateRouteTable`
    - **VPC:** Select your **VPC**.
    - Click **Create**.
6. **Add route for internet access via NAT:**
    - Select your **PrivateRouteTable**.
    - **Routes tab** → **Edit routes**.
    - **Destination:** `0.0.0.0/0`
    - **Target:** Select your **NAT Gateway**.
    - Click **Save changes**.
7. **Associate Private Subnet:**
    - Go to **Subnet associations** tab.
    - **Edit subnet associations**.
    - Select your **PrivateSubnet** → **Save**.

---

## 📡 **Step 6: Launch EC2 Instances**

1. Go to **EC2 Dashboard** → **Launch instance**.
2. **Public EC2 Instance:**
    - **AMI:** Choose Amazon Linux 2 AMI.
    - **Instance type:** t2.micro (free tier eligible).
    - **VPC:** Select your VPC.
    - **Subnet:** Choose the **PublicSubnet**.
    - **Enable auto-assign public IP**.
    - **Key pair:** Create or use an existing one.
    - **Security group:** Allow **HTTP, HTTPS, and SSH** traffic.
3. **Private EC2 Instance:**
    - Follow the same steps, but place the instance in the **PrivateSubnet**.
    - Disable **auto-assign public IP**.

---

## 🎯 **Step 7: Test connectivity**

1. **Public EC2:**
    - Get the **Public IP** from the EC2 console.
    - SSH into the public instance:

```bash

ssh -i your-key.pem ec2-user@your-public-ip

```

1. **Private EC2:**
    - SSH from the **public EC2** to the **private EC2** using its **Private IP**:

```bash

ssh -i your-key.pem ec2-user@your-private-ip

```

1. **Test internet access from the Private EC2:**

```bash

ping google.com

```

If it works, your **NAT Gateway** is correctly routing traffic!


[Screenshot 2025-03-06 180504.png](https://www.notion.so/1b45e020b74f808f90fcd2e8c5b8477e?pvs=21)
