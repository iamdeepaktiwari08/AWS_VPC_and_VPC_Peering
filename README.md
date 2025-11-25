# AWS VPC Peering Project – Test VPC & Production VPC (FULL GUIDE)

This project demonstrates how to design, configure, peer, and test connectivity between two isolated AWS VPC environments:
**Test VPC** and **Production VPC**, using VPC Peering.

The goal is to allow private communication between EC2 instances deployed in separate VPCs.

---

# 📌 Project Overview

In this project, we will:

✔ Create Two VPCs  
✔ Create Subnets  
✔ Deploy EC2 Instances in each  
✔ Attach Internet Gateways  
✔ Create Route Tables  
✔ Associate Subnets to Route Tables  
✔ Create VPC Peering Connection  
✔ Add Routes for Peering  
✔ Modify EC2 Security Groups  
✔ Test Connectivity via Ping  

---

# 🏗 Architecture

AWS Region
├── Test VPC (10.0.0.0/16)
│    ├── Public Subnet (10.0.1.0/24)
│    ├── Internet Gateway
│    ├── Public Route Table
│    └── EC2 Test Instance
│
└── Production VPC (10.1.0.0/16)
├── Public Subnet (10.1.1.0/24)
├── Internet Gateway
├── Public Route Table
└── EC2 Prod Instance

Connection between VPCs: **VPC Peering**

Private connectivity validated by **ICMP (ping)**.

---

# 🔧 Prerequisites

- AWS Account
- Basic networking knowledge
- Key Pair (.pem file)
- Ubuntu EC2 AMI
- VPC service enabled

---

# 🚀 Step-By-Step Guide

---

## STEP-1 — Create Test VPC

AWS Console → VPC → Create VPC

Configuration:

| Field | Value |
|--------|-----------|
| Name | test-vpc |
| IPv4 CIDR | 10.0.0.0/16 |
| Tenancy | default |

Create

---

## STEP-2 — Create Production VPC

Repeat same:

| Name | prod-vpc |
| IPv4 CIDR | 10.1.0.0/16 |

Create

---

---

# 🌐 SUBNET CONFIGURATION

## STEP-3 — Create Public Subnet in Test VPC

VPC → Subnets → Create Subnet

| Field | Value |
|--------|-----------|
| VPC | test-vpc |
| Name | test-public-subnet |
| CIDR | 10.0.1.0/24 |

Create

Enable auto assign IPv4:
- Edit subnet → Enable **Auto assign public IP**

---

## STEP-4 — Create Public Subnet in Prod VPC

| Field | Value |
|--------|-----------|
| VPC | prod-vpc |
| Name | prod-public-subnet |
| CIDR | 10.1.1.0/24 |

Enable auto-assign public IP.

---

---

# 🌍 INTERNET GATEWAY

## STEP-5 — Create IGW for Test VPC

VPC → Internet Gateways → Create

Name: test-igw  
Attach to **test-vpc**

---

## STEP-6 — Create IGW for Prod VPC

Name: prod-igw  
Attach to **prod-vpc**

---

---

# 🛣 ROUTE TABLE CONFIGURATION

## STEP-7 — Create Test Public Route Table

VPC → Route Tables → Create

Name: test-public-rt  
VPC: test-vpc

Add route:

| Destination | Target |
|--------------|-----------|
| 0.0.0.0/0 | test-igw |

Associate subnet:
- test-public-subnet

---

## STEP-8 — Create Prod Public Route Table

Name: prod-public-rt  
VPC: prod-vpc

Add route:

| Destination | Target |
|--------------|-----------|
| 0.0.0.0/0 | prod-igw |

Associate subnet:
- prod-public-subnet

---

---

# 🖥 STEP-9 — Launch EC2 Instances

## Test instance config

| Parameter | Value |
|-----------|--------|
| OS | Ubuntu |
| VPC | test-vpc |
| Subnet | test-public-subnet |
| Public IP | Enabled |
| SG | allow SSH + ICMP |

## Prod instance config

Same config but in prod-vpc.

---

---

# 🔐 STEP-10 — Configure Security Groups

## TEST EC2 Inbound Rules

| Type | Source |
|--------|----------|
| SSH | My IP |
| ICMP ALL | 10.1.0.0/16 |

## PROD EC2 Inbound Rules

| Type | Source |
|--------|----------|
| SSH | My IP |
| ICMP ALL | 10.0.0.0/16 |

---

---

# 🔁 STEP-11 — Create VPC Peering Connection

VPC → Peering → Create Peering

| Field | Value |
|--------|--------|
| Name | test-prod-peering |
| Requester VPC | test-vpc |
| Accepter VPC | prod-vpc |

Create  
Then → Accept request.

Status must become: **Active**

---

---

# 📡 STEP-12 — ADD ROUTES FOR PEERING

## TEST Route Table

(test-public-rt)

Add route:

| Destination | Target |
|--------------|-----------------------|
| 10.1.0.0/16 | peering-connection-ID |

---

## PROD Route Table

(prod-public-rt)

Add route:

| Destination | Target |
|--------------|-----------------------|
| 10.0.0.0/16 | peering-connection-ID |

---

---

# 🧪 STEP-13 — Connectivity Validation

Login to TEST EC2:
then ping test to prod private ip 

Expected:

✔ Replies  
✔ 0% packet loss  

# 📸 Screenshots to Include

- VPC list
- Subnets
- Route Tables
- IGWs
- VPC Peering page
- EC2 Instances list
- Ping result Terminal
- SG inbound rules

📂 **Screenshots Folder:**  
 ![image alt](https://github.com/iamdeepaktiwari08/AWS_VPC_and_VPC_Peering/tree/main/screenshots
 [
 ](https://github.com/iamdeepaktiwari08/AWS_VPC_and_VPC_Peering/blob/dfee89f9b3e33b480e0e0fd57fe03c8fe318b9a5/screenshots/01-vpc-list.png)
