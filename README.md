# AWS Secure 3-Tier Architecture

This project demonstrates the design and implementation of a secure 3-tier architecture on AWS using EC2, RDS, and VPC networking. The architecture enforces strict network isolation and least privilege access to protect backend resources.

---

## Table of Contents
- [Overview](#overview)
- [Architecture Design](#architecture-design)
- [Technologies Used](#technologies-used)
- [Implementation Steps](#implementation-steps)
- [Security Configuration](#security-configuration)
- [Commands Executed](#commands-executed)
- [Database Setup and Testing](#database-setup-and-testing)
- [Screenshots Breakdown](#screenshots-breakdown)
- [Key Learnings](#key-learnings)
- [Conclusion](#conclusion)

---

## Overview

The objective of this project is to build a secure cloud architecture where:

- A web server is publicly accessible  
- A database is isolated in private subnets  
- Communication between layers is strictly controlled  
- Security groups enforce least privilege access  

This reflects real-world production architecture patterns used in cloud environments.

---

## Architecture Design

The architecture consists of:

- A custom VPC with defined CIDR range  
- A public subnet hosting the EC2 web server  
- Private subnets hosting the RDS database  
- Internet Gateway for external access  
- Route tables controlling traffic flow  
- Security groups acting as virtual firewalls  

### Traffic Flow
- Users → EC2 (public subnet)  
- EC2 → RDS (private subnet)  
- No direct internet access to RDS  

---

## Technologies Used

- Amazon EC2  
- Amazon RDS (MySQL)  
- Amazon VPC  
- Subnets & Route Tables  
- Internet Gateway  
- Security Groups  
- Nginx  
- SSH  
- MySQL CLI  

---

## Implementation Steps

### 1. VPC Setup

A custom VPC was created to define the network boundary for all resources.

![VPC Created](vpc-created.png)

---

### 2. Subnet Configuration

Subnets were created to enforce separation between public and private resources.

- Public subnet → hosts EC2  
- Private subnets → host RDS  

![Public Subnet](public-subnet-a.png)  
*Public subnet used for internet-facing resources*

![Private Subnet](private-subnet-a.png)  
*Private subnet used for backend services*

![Subnets Overview](subnets-created.png)  
*All subnets configured within the VPC*

---

### 3. Internet Gateway

An Internet Gateway was attached to enable communication between the VPC and the internet.

![IGW Attached](igw-attached.png)

---

### 4. Route Table Configuration

A route table was configured to route internet traffic through the Internet Gateway and associated with the public subnet.

![Route Table](public-route-table.png)  
*Route table containing internet route*

![Route Table Config](public-rt-config.png)  
*Association between route table and public subnet*

---

### 5. EC2 Web Server Deployment

An EC2 instance was launched in the public subnet and configured as a web server using Nginx.

- SSH access enabled  
- HTTP traffic allowed  
- Custom web page deployed  

![EC2 Instance Running](ec2-instance-running.png)  
*EC2 instance successfully running*

![Web Server Running](ec2-web-working.png)  
*Nginx successfully installed and serving traffic*

![Custom Web Page](nginx-custom-page.png)  
*Custom web page confirming server functionality*

---

### 6. RDS Subnet Group Creation

A DB subnet group was created using private subnets to ensure the database is not publicly accessible.

![DB Subnet Group](rds-db-subnet-group-created.png)

---

### 7. RDS Database Deployment

A MySQL RDS instance was deployed with:

- Public access disabled  
- Placement in private subnets  
- Secure authentication configured  

![RDS Database Created](rds-create-database.png)

---

### 8. Security Group Configuration

Security groups were configured following least privilege principles.

#### EC2 Security Group
- Allows inbound:
  - SSH (22)
  - HTTP (80)

#### RDS Security Group
- Allows inbound:
  - MySQL (3306)
  - Only from EC2 security group

![Security Group Rules](rds-security-group-configured.png)  
*Initial security group configuration*

![Least Privilege Enforcement](rds-security-group-least-privilege.png)  
*Restricted access allowing only EC2 to connect to RDS*

---

## Security Configuration

This architecture enforces:

- Private database deployment  
- No public access to RDS  
- Strict inbound rules via security groups  
- Network segmentation using subnets  
- Controlled internal communication  

---

## Commands Executed

### EC2 Setup (Nginx)

```bash
sudo dnf update -y
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
