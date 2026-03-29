# AWS Lab 02 — Build Your VPC and Launch a Web Server

**Series:** AWS Cloud Practitioner / DevOps Foundations Labs
**Difficulty:** Beginner | **Duration:** ~30 mins | **Region:** us-east-1 | **Services:** Amazon VPC, EC2, Internet Gateway, NAT Gateway

---

## Overview

This lab covers building a complete VPC network from scratch — including public and private subnets across two Availability Zones, routing configuration, security groups, and deploying a live web server into the network. This is the foundational networking skill for any DevOps or Cloud Engineer working on AWS.

---

## Architecture

- 1 VPC (lab-vpc) — CIDR: 10.0.0.0/16
- 2 Public Subnets — 10.0.0.0/24 (AZ-A) and 10.0.2.0/24 (AZ-B)
- 2 Private Subnets — 10.0.1.0/24 (AZ-A) and 10.0.3.0/24 (AZ-B)
- 1 Internet Gateway — for public subnet outbound internet access
- 1 NAT Gateway — for private subnet outbound internet access (no inbound exposure)
- 2 Route Tables — one public (via IGW), one private (via NAT)
- 1 Security Group — HTTP port 80 inbound only
- 1 EC2 Web Server — deployed in public subnet AZ-B with automated app install

---

## What I Did

- Created a VPC with public/private subnets, IGW, NAT Gateway, and route tables
- Added a second public and private subnet in a second AZ for high availability
- Created a Web Security Group with HTTP inbound rule
- Launched an EC2 web server into the VPC using a User Data script

---

## User Data Script
```bash
#!/bin/bash
# Install Apache Web Server and PHP
dnf install -y httpd wget php mariadb105-server
# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/.../lab-app.zip
unzip lab-app.zip -d /var/www/html/
# Turn on web server
chkconfig httpd on
service httpd start
```

---

## Routing Configuration

| Route Table       | Destination | Target           | Associated Subnets            |
|-------------------|-------------|------------------|-------------------------------|
| lab-rtb-public    | 0.0.0.0/0   | Internet Gateway | public1 (AZ-A), public2 (AZ-B)|
| lab-rtb-public    | 10.0.0.0/16 | local            | public1 (AZ-A), public2 (AZ-B)|
| lab-rtb-private   | 0.0.0.0/0   | NAT Gateway      | private1 (AZ-A), private2 (AZ-B)|
| lab-rtb-private   | 10.0.0.0/16 | local            | private1 (AZ-A), private2 (AZ-B)|

---

## Key Concepts Learned

**1. VPC and CIDR Blocks**
A VPC is your own isolated network within AWS. The /16 CIDR gives 65,536 IPs. Subnets carve out smaller ranges (/24 = 256 IPs each) for different purposes.

**2. Public vs Private Subnets**
The difference is purely routing — a public subnet points 0.0.0.0/0 to an Internet Gateway. A private subnet points to a NAT Gateway. This controls whether resources are reachable from the internet.

**3. Internet Gateway vs NAT Gateway**
The Internet Gateway enables two-way communication between your VPC and the internet. The NAT Gateway lets private resources make outbound requests without being directly reachable from the internet.

**4. Multi-AZ Architecture for High Availability**
Spreading subnets across two Availability Zones means your infrastructure survives an AZ failure — a foundational HA pattern used in production deployments.

**5. Security Groups**
By only allowing port 80, we applied least-privilege networking — a zero-trust principle where all traffic is blocked unless explicitly permitted.

---

## DevOps Relevance

| Lab Concept           | Real-World Application                                      |
|-----------------------|-------------------------------------------------------------|
| VPC + Subnet design   | Network isolation for microservices, multi-tier apps        |
| Public vs Private     | Web tier (public) vs DB/app tier (private) separation       |
| NAT Gateway           | Private EC2 instances pulling from package repos or S3      |
| Multi-AZ subnets      | Foundation for ALB, Auto Scaling Groups, RDS Multi-AZ       |
| Route tables          | Traffic engineering, VPN routing, Transit Gateway           |
| Security Groups       | Least-privilege access, microsegmentation                   |

---

## References

- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/)
- [Internet Gateways](https://docs.aws.amazon.com)
- [NAT Gateways](https://docs.aws.amazon.com)
- [Route Tables](https://docs.aws.amazon.com)
- [VPC Security Groups](https://docs.aws.amazon.com)
