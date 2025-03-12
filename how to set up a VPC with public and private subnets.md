Let’s go step by step on how to set up a VPC with public and private subnets on AWS EC2 and configure a NAT Gateway for private subnets so they can access the internet securely. Let’s roll! 🚀

🌐 Step 1: Create a VPC
Go to the AWS Management Console.
Search for VPC and open the VPC Dashboard.
Click Create VPC.
Configure the VPC:
Name: MyVPC
IPv4 CIDR block: 10.0.0.0/16
Leave other options as default.
Click Create VPC.
📦 Step 2: Create public and private subnets
In the VPC Dashboard, go to Subnets → Create Subnet.
For Public Subnet:
Name: PublicSubnet
VPC ID: Select your newly created VPC.
Availability Zone: Choose an AZ (e.g., us-east-1a).
IPv4 CIDR block: 10.0.1.0/24
Click Create Subnet.
For Private Subnet:
Name: PrivateSubnet
VPC ID: Select your VPC.
Availability Zone: Choose the same or another AZ (e.g., us-east-1b).
IPv4 CIDR block: 10.0.2.0/24
Click Create Subnet.
🌍 Step 3: Set up an Internet Gateway (for public access)
Go to Internet Gateways → Create Internet Gateway.
Name: MyInternetGateway
Click Create Internet Gateway.
Attach it to the VPC:
Select your VPC → Attach internet gateway.
🔥 Step 4: Configure route tables
Go to Route Tables → Create route table.
For the Public Subnet:
Name: PublicRouteTable
VPC: Select your VPC.
Click Create.
Add route to the Internet:
Select your new PublicRouteTable.
Click on the Routes tab → Edit routes → Add route.
Destination: 0.0.0.0/0
Target: Select your Internet Gateway.
Click Save changes.
Associate with Public Subnet:
Go to the Subnet associations tab.
Edit subnet associations.
Select your PublicSubnet → Save.
🏡 Step 5: Set up a NAT Gateway for Private Subnets
Go to NAT Gateways → Create NAT Gateway.
Name: MyNATGateway
Subnet: Select your PublicSubnet.
Elastic IP allocation:
Click Allocate Elastic IP → Create NAT Gateway.
Update route table for Private Subnet:
Go to Route Tables → Create route table.
Name: PrivateRouteTable
VPC: Select your VPC.
Click Create.
Add route for internet access via NAT:
Select your PrivateRouteTable.
Routes tab → Edit routes.
Destination: 0.0.0.0/0
Target: Select your NAT Gateway.
Click Save changes.
Associate Private Subnet:
Go to Subnet associations tab.
Edit subnet associations.
Select your PrivateSubnet → Save.
📡 Step 6: Launch EC2 Instances
Go to EC2 Dashboard → Launch instance.
Public EC2 Instance:
AMI: Choose Amazon Linux 2 AMI.
Instance type: t2.micro (free tier eligible).
VPC: Select your VPC.
Subnet: Choose the PublicSubnet.
Enable auto-assign public IP.
Key pair: Create or use an existing one.
Security group: Allow HTTP, HTTPS, and SSH traffic.
Private EC2 Instance:
Follow the same steps, but place the instance in the PrivateSubnet.
Disable auto-assign public IP.
🎯 Step 7: Test connectivity
Public EC2:
Get the Public IP from the EC2 console.
SSH into the public instance:
bash
Copy
Edit
ssh -i your-key.pem ec2-user@your-public-ip
Private EC2:
SSH from the public EC2 to the private EC2 using its Private IP:
bash
Copy
Edit
ssh -i your-key.pem ec2-user@your-private-ip
Test internet access from the Private EC2:
bash
Copy
Edit
ping google.com
If it works, your NAT Gateway is correctly routing traffic!

✅ Done! Now, you have:

A VPC with public and private subnets.
An Internet Gateway for the public subnet.
A NAT Gateway for the private subnet.
EC2 instances communicating securely.
