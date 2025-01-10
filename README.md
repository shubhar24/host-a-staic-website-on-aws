# Host a Static Website on AWS

## Project Overview

This project demonstrates how to host a static HTML web app on Amazon Web Services (AWS) utilizing various resources such as EC2 instances, VPC, Application Load Balancer, Auto Scaling Groups, Route 53, and more. The objective is to configure a highly available, scalable, and fault-tolerant architecture for hosting a simple static website.

## Architecture Overview

The architecture of the project includes the following components:

1. **Virtual Private Cloud (VPC)**: 
   - Configured with both public and private subnets across two different availability zones.
2. **Internet Gateway**:
   - Facilitates connectivity between VPC instances and the wider internet.
3. **Security Groups**:
   - Acts as a network firewall mechanism to control inbound and outbound traffic.
4. **EC2 Instances**:
   - Web servers positioned in private subnets for enhanced security.
   - EC2 Instance Connect Endpoint used for secure connections to assets.
5. **NAT Gateway**:
   - Allows EC2 instances in private subnets to access the internet.
6. **Application Load Balancer**:
   - Distributes web traffic evenly to a target group of EC2 instances.
7. **Auto Scaling Group**:
   - Automatically manages EC2 instances, ensuring website availability, scalability, and fault tolerance.
8. **SSL/TLS Certificate**:
   - Configured using AWS Certificate Manager for securing application communications.
9. **Route 53**:
   - Domain registration and DNS record configuration for the website.
10. **Simple Notification Service (SNS)**:
   - Sends alerts about activities within the Auto Scaling Group.

## Requirements

Before deploying this solution, ensure you have the following:

- An AWS account with necessary IAM permissions.
- AWS CLI configured on your local machine.
- Domain name registered with Route 53 (optional).

## Deployment Steps

### Step 1: Configure VPC and Subnets

1. Create a Virtual Private Cloud (VPC) with public and private subnets in at least two different availability zones for fault tolerance and high availability.
2. Attach an Internet Gateway to the VPC to allow external internet access for the public subnet.
3. Set up a NAT Gateway in the public subnet to enable private subnet instances to access the internet.

### Step 2: Setup EC2 Instances and Load Balancer

1. Deploy EC2 instances in the private subnet, ensuring web servers are isolated for better security.
2. Set up an Application Load Balancer (ALB) in the public subnet to distribute incoming web traffic across EC2 instances.
3. Create an Auto Scaling Group with your EC2 instances to automatically scale based on web traffic.

### Step 3: Install Apache and Deploy Web App

1. SSH into the EC2 instance and run the following script to install Apache HTTP Server, Git, and deploy the static website files from a GitHub repository.

#### EC2 Deployment Script:

```bash
#!/bin/bash
# Switch to the root user to gain full administrative privileges
sudo su

# Update all installed packages to their latest versions
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change the current working directory to the Apache web root
cd /var/www/html

# Install Git
yum install git -y

# Clone the project GitHub repository to the current directory
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git

# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/

# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws

# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd

# Start the Apache HTTP Server to serve web content
systemctl start httpd
```

### Step 4: Configure SSL and Security

1. Secure your application using AWS Certificate Manager (ACM) for SSL certificates.
2. Set up HTTPS to ensure secure communication between users and your web servers.

### Step 5: DNS Configuration

1. Register your domain in Route 53.
2. Configure a DNS record pointing to your Application Load Balancer (ALB) to route traffic to the EC2 instances.

### Step 6: Set Up SNS Notifications

1. Create an SNS topic for monitoring your Auto Scaling Group's activities.
2. Set up SNS notifications to alert you about any changes to the scaling of EC2 instances.

## GitHub Repository

The project files are available in this GitHub repository:

[GitHub Repository Link](https://github.com/aosnotes77/host-a-static-website-on-aws)

## Conclusion


---

Let me know if you need any further modifications or additional details for the README!
