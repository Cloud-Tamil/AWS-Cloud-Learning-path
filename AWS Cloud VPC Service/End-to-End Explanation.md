# 🚀 AWS VPC — Complete End-to-End Networking Architecture Guide

<p align="center">

[![AWS](https://img.shields.io/badge/AWS-VPC-FF9900?logo=amazonaws\&logoColor=white)](https://aws.amazon.com/vpc/)
[![Cloud](https://img.shields.io/badge/Cloud-Networking-blue)]()
[![Security](https://img.shields.io/badge/Security-Production--Ready-green)]()
[![Architecture](https://img.shields.io/badge/Architecture-High%20Availability-orange)]()

</p>

---

## 📖 Project Overview

This project demonstrates how to design and build a **Production-Grade AWS VPC Architecture** from scratch using:

### 🧠 What is a VPC?

A **Virtual Private Cloud (VPC)** is a logically isolated network inside AWS where you deploy cloud resources securely.

### 📊 Architecture Components Overview

| Component        | Purpose                      | Why It Is Important                  |
| ---------------- | ---------------------------- | ------------------------------------ |
| VPC              | Isolated AWS Network         | Provides secure cloud infrastructure |
| Subnet           | Logical network segmentation | Separates workloads                  |
| Public Subnet    | Internet-facing resources    | Hosts web servers/load balancers     |
| Private Subnet   | Internal resources           | Protects backend services            |
| Internet Gateway | Public internet access       | Enables inbound/outbound internet    |
| NAT Gateway      | Outbound-only internet       | Secure updates for private servers   |
| Route Table      | Traffic direction rules      | Controls packet flow                 |
| Security Group   | Stateful firewall            | Instance-level protection            |
| NACL             | Stateless subnet firewall    | Additional network filtering         |
| EC2              | Virtual server               | Hosts applications                   |
| RDS              | Managed database             | Secure database hosting              |

This architecture follows real-world cloud networking and security best practices used in enterprise environments.

---

## 🎯 Project Goals

The purpose of this project is to understand:

- ✅ AWS Networking Fundamentals
- ✅ Secure Cloud Architecture Design
- ✅ Public vs Private Networking
- ✅ Internet Connectivity in AWS
- ✅ Routing and Traffic Flow
- ✅ Security Isolation
- ✅ Multi-Tier Architecture
- ✅ Production Best Practices

---

## ❓ Why Do We Need a VPC?

Without a VPC:

- ❌ All resources would exist in a shared public environment
- ❌ No network isolation
- ❌ No traffic control
- ❌ Weak security boundaries

With a VPC:

- ✅ Full network isolation
- ✅ Better security
- ✅ Custom routing
- ✅ Controlled internet access
- ✅ Enterprise-grade cloud networking

---

## 🌐 Step 1 — Create VPC

### 📌 Purpose

We create a VPC to establish a secure and isolated cloud network.

### 🛠️ Configuration

```bash
VPC Dashboard → Create VPC
```

| Setting    | Value         |
| ---------- | ------------- |
| Name       | `My-VPC`      |
| CIDR Block | `10.0.0.0/16` |
| Tenancy    | `Default`     |

### ❓ Why `/16` CIDR?

```text
10.0.0.0/16
```

Provides:

* 65,536 total IP addresses
* Large scalability
* Space for future subnet expansion

Perfect for production environments.

### 📘 CIDR Deep Dive

| CIDR  | Total IPs | Usable IPs | Usage            |
| ----- | --------- | ---------- | ---------------- |
| `/16` | 65,536    | 65,531     | Large Networks   |
| `/24` | 256       | 251        | Standard Subnets |
| `/28` | 16        | 11         | Small Services   |

AWS reserves 5 IPs per subnet for internal networking purposes.

---

## 🌐 Step 2 — Create Subnets

### 📌 Purpose

Subnets divide the VPC into smaller secure sections.

This enables:

✅ Isolation
✅ Security
✅ Better architecture organization
✅ Controlled access

### 🛠️ Subnet Configuration

| Subnet             | CIDR          | AZ            | Purpose                  |
| ------------------ | ------------- | ------------- | ------------------------ |
| Public-Subnet      | `10.0.1.0/24` | `ap-south-1a` | Internet-facing services |
| Private-App-Subnet | `10.0.2.0/24` | `ap-south-1a` | Backend applications     |
| Private-DB-Subnet  | `10.0.3.0/24` | `ap-south-1b` | Database isolation       |

### ❓ Why Different Subnets?

| Subnet Type | Why We Need It                      |
| ----------- | ----------------------------------- |
| Public      | Allows users to access applications |
| Private App | Protects backend business logic     |
| Private DB  | Maximum database security           |

### ❓ Why Separate Availability Zones?

Using multiple AZs provides:

✅ High Availability
✅ Disaster Recovery
✅ Fault Tolerance
✅ Multi-AZ Database Support

---

## 🌍 Step 3 — Create Internet Gateway

### 📌 Purpose

The Internet Gateway allows public resources to communicate with the internet.

Without IGW:

❌ Public EC2 instances cannot access the internet
❌ Users cannot access hosted applications

### 🛠️ Configuration

```bash
Internet Gateways → Create
```

| Setting   | Value    |
| --------- | -------- |
| Name      | `My-IGW` |
| Attach To | `My-VPC` |

### 🌐 Internet Gateway Responsibilities

* Enables public internet access
* Supports inbound and outbound traffic
* Connects AWS VPC to the internet
* Highly available by AWS design

---

## 🚦 Step 4 — Configure Route Tables

### 📌 Purpose

Route tables determine where network traffic should go.

Think of route tables like:

> 🛣️ GPS navigation for packets.

### 🌐 Public Route Table

| Destination   | Target | Purpose                    |
| ------------- | ------ | -------------------------- |
| `10.0.0.0/16` | local  | Internal VPC communication |
| `0.0.0.0/0`   | IGW    | Internet access            |

### 🔒 Private Route Table

Initially:

| Destination   | Target |
| ------------- | ------ |
| `10.0.0.0/16` | local  |

Private subnets cannot access the internet yet.

### ❓ Why Separate Route Tables?

Because:

* Public subnets need internet access
* Private subnets should remain protected

This separation improves security.

---

## 🌐 Step 5 — Create NAT Gateway

### 📌 Purpose

Private servers often need outbound internet access for:

* Installing packages
* Downloading updates
* Accessing APIs

But they should NOT accept inbound internet traffic.

That is why we use a NAT Gateway.

### 🛠️ NAT Gateway Configuration

| Setting    | Value           |
| ---------- | --------------- |
| Name       | `My-NAT-GW`     |
| Subnet     | `Public-Subnet` |
| Elastic IP | Required        |

### ❓ Why NAT Gateway?

Without NAT Gateway:

❌ Private EC2 cannot install packages
❌ No software updates
❌ Cannot access external APIs

With NAT Gateway:

✅ Outbound internet works
✅ Inbound internet blocked
✅ Better security

### 🔄 NAT Traffic Flow

```text
Private EC2
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet
```

---

## 🔐 Step 6 — Create Security Groups

### 📌 Purpose

Security Groups act as virtual firewalls for EC2 and RDS resources.

They control:

* Allowed traffic
* Allowed ports
* Allowed sources

### 🌐 Web Security Group

### Why?

Allows public users to access the website.

| Port | Purpose          |
| ---- | ---------------- |
| 80   | HTTP             |
| 443  | HTTPS            |
| 22   | Secure Admin SSH |

### ⚙️ App Security Group

### Why?

Ensures only web servers can communicate with backend applications.

Prevents direct public access.

## 🗄️ Database Security Group

### Why?

Protects databases from direct internet access.

Only application servers can communicate with databases.

### 🔄 Security Group vs NACL

| Feature   | Security Group | NACL         |
| --------- | -------------- | ------------ |
| Stateful  | ✅ Yes          | ❌ No         |
| Stateless | ❌ No           | ✅ Yes        |
| Level     | Instance       | Subnet       |
| Rules     | Allow only     | Allow + Deny |

---

## 💻 Step 7 — Launch EC2 Instances

### 🌐 Web Server EC2

### 📌 Purpose

Hosts frontend/web applications accessible from the internet.

### User Data Script

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl enable httpd
systemctl start httpd

echo "<h1>Web Server in Public Subnet</h1>" > /var/www/html/index.html
```

## ⚙️ App Server EC2

### 📌 Purpose

Runs backend application logic securely inside private subnet.

No direct public access allowed.

### User Data Script

```bash
#!/bin/bash
yum update -y
yum install -y python3

echo "App Server Ready" > /home/ec2-user/status.txt
```

---

## 🗄️ Step 8 — Create Amazon RDS

### 📌 Purpose

Amazon RDS provides managed database hosting.

Benefits:

✅ Automated backups
✅ High Availability
✅ Managed patching
✅ Better scalability
✅ Security isolation

### 🛠️ RDS Configuration

| Setting          | Value     |
| ---------------- | --------- |
| Engine           | MySQL 8.0 |
| Public Access    | No        |
| Security Group   | DB-SG     |
| Backup Retention | 7 Days    |

---

### 🔐 Security Architecture

| Layer           | Protection            |
| --------------- | --------------------- |
| VPC             | Network isolation     |
| Subnets         | Segmentation          |
| Route Tables    | Traffic control       |
| Security Groups | Instance firewall     |
| NACLs           | Subnet firewall       |
| Private Subnets | Hidden infrastructure |

### 🧪 Connectivity Testing

| Test                           | Expected Result |
| ------------------------------ | --------------- |
| Public EC2 → Internet          | ✅ Works         |
| Private EC2 → Internet via NAT | ✅ Works         |
| Internet → Private EC2         | ❌ Blocked       |
| App → Database                 | ✅ Allowed       |
| Internet → Database            | ❌ Blocked       |

### 🚀 Production Best Practices

### ✅ High Availability

* Multi-AZ subnets
* NAT Gateway per AZ
* Load Balancers
* Auto Scaling

---

### ✅ Security

* Least privilege access
* Restrict SSH
* Private databases
* Use AWS WAF
* Enable Flow Logs

---

### ✅ Cost Optimization

* Use VPC Endpoints
* Delete unused Elastic IPs
* Monitor NAT Gateway costs

---

## 🚫 Common Mistakes

| Mistake           | Risk                    |
| ----------------- | ----------------------- |
| Using default VPC | Weak security           |
| Open SSH to world | Attack risk             |
| Public database   | Data exposure           |
| No NAT redundancy | Single point of failure |

---

## 👨‍💻 Author

**Tamilselvan.M**

Cloud & DevOps Engineer
AWS | Networking | Linux | Infrastructure | Automation

---
