# Scalable Web Application on AWS (ALB + Auto Scaling + SNS)

This project demonstrates how to deploy a **scalable and highly available web application on AWS** using core cloud infrastructure services such as **Application Load Balancer, Auto Scaling Group, Launch Templates, and Amazon SNS**.

The architecture is designed inside a **custom VPC using public subnets**, allowing the application to be accessible from the internet while distributing traffic across multiple EC2 instances.

---

## Project Architecture

User → Internet → Application Load Balancer → Target Group → EC2 Instances (Auto Scaling)

When traffic increases, **Auto Scaling automatically launches new EC2 instances**, and the **Application Load Balancer distributes traffic across healthy instances**.

Amazon **SNS notifications** provide alerts whenever scaling activities occur.

---

## Technologies Used

- AWS VPC
- EC2
- Launch Templates
- Auto Scaling Group
- Application Load Balancer (ALB)
- Target Groups
- Amazon SNS
- Nginx Web Server
- Linux (Amazon Linux)

---

## Infrastructure Components

### 1. Virtual Private Cloud (VPC)

A custom VPC was created to isolate the infrastructure and manage networking components.

### 2. Public Subnets

Public subnets host:

- Application Load Balancer
- EC2 Instances launched by Auto Scaling

---

### 3. Internet Gateway

The Internet Gateway enables communication between the VPC and the public internet, allowing users to access the web application.


---


### 4. Application Load Balancer (ALB)

The ALB distributes incoming HTTP traffic across multiple EC2 instances to improve availability and fault tolerance.



---

### 5. Auto Scaling Group (ASG)

The Auto Scaling Group automatically:

- Launches EC2 instances when traffic increases
- Terminates instances when demand decreases
- Replaces unhealthy instances


---

### 6. Launch Template

The Launch Template defines the configuration for EC2 instances including:

- Instance type
- AMI
- Security groups
- User data script


---

### 7. Amazon SNS Notifications

Amazon SNS is used to send **email notifications whenever scaling activities occur**, such as:

- Instance launch
- Instance termination
- Scaling events


---


## User Data Script

The EC2 instances automatically install **Nginx** and display the hostname to demonstrate load balancing.



```bash
#!/bin/bash
yum update -y
yum install -y nginx
systemctl start nginx
systemctl enable nginx

echo "<h1>Scalable Web App</h1>
<p><strong>Hostname:</strong> $(hostname)</p>
<p><strong>Instance IP:</strong> $(hostname -I)</p>" > /usr/share/nginx/html/index.html


