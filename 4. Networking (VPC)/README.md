# AWS VPC Networking – Core Components Explained

This document provides a **detailed, structured explanation** of the **core AWS VPC networking components**:
- VPC
- CIDR
- Subnets
- Internet Gateway (IGW)
- Route Tables 
- NAT Gateway

The explanations focus on **concept clarity, structure, and real-world usage**.

---

## 1. Virtual Private Cloud (VPC)


A **Virtual Private Cloud (VPC)** is a **logically isolated virtual network** within AWS where you can launch AWS resources in a **fully controlled networking environment**.

It acts as a **private data center network in the cloud**.

### Key Characteristics
- Defined using a **CIDR block**
- Isolated from other VPCs by default
- Region-specific resource
- Supports IPv4 and IPv6
- Allows complete control over networking components

### What You Control Inside a VPC
- IP address range
- Subnets
- Routing
- Internet connectivity
- Security boundaries


Example

VPC CIDR: 10.0.0.0/16
Total IP addresses: 65,536

--- 



## 2. CIDR (Classless Inter-Domain Routing)


**CIDR** defines the **IP address range** used by a VPC or subnet and determines **how many IP addresses are available**.

### 
CIDR Format

10.0.0.0/16 → Large network
10.0.1.0/24 → Smaller subnet


### CIDR Rule
- Lower prefix number = more IPs
- Higher prefix number = fewer IPs

 AWS CIDR Constraints
- VPC CIDR size: /16 to /28
- Subnet CIDR size: /16 to /28
- CIDR blocks must not overlap

---

## 3. Subnets

### Definition
A **Subnet** is a **logical subdivision of a VPC’s IP address range** where AWS resources are deployed.

Each subnet exists in **one Availability Zone (AZ)**.

### Purpose of Subnets
- Network segmentation
- High availability
- Security isolation
- Traffic control

---

### Types of Subnets

#### Public Subnet
A subnet that has a **route to the Internet Gateway**.

**Characteristics**
- Can host internet-facing resources
- Instances may have public IP addresses
- Common use cases:
  - Web servers
  - Load balancers
  - Bastion hosts

---

## 4. Internet Gateway (IGW)

### Definition
An **Internet Gateway (IGW)** is an AWS-managed component that enables **communication between resources in a VPC and the public internet**.

### Key Functions
- Enables inbound and outbound internet traffic
- Performs network address translation for public IPv4
- Horizontally scalable and highly available

### Important Conditions for Internet Access
For an EC2 instance to access the internet:
  - IGW must be attached to the VPC
- Subnet route table must route traffic to IGW
- Instance must have a public or Elastic IP
- Security Group and NACL must allow traffic

---

## 5. Route Tables

### Definition
A **Route Table** contains a set of rules that determine **how network traffic is directed** within a VPC.

Every subnet must be associated with **exactly one route table**.

---

### Route Table Components

| Component | Description |
|---------|-------------|
| Destination | IP range for traffic |
| Target | Where traffic is sent |

---

### Default Route
Every route table contains a **local route** that enables communication inside the VPC.


 ### Public Route Table Example

Destination Target

10.0.0.0/16 local

0.0.0.0/0 igw-xxxx

 ### Private Route Table Example
Destination Target

10.0.0.0/16 local

0.0.0.0/0 nat-xxxx


---

## 6. NAT Gateway (Network Address Translation)

### Definition
A **NAT Gateway** allows instances in **private subnets** to access the internet **without allowing inbound traffic from the internet**.

### Why NAT Gateway is Required
- Private resources need:
  - OS updates
  - Package downloads
  - External API access
- But must not be exposed publicly

---

### NAT Gateway Characteristics
- Deployed in a **public subnet**
- Requires an **Elastic IP**
- Supports only outbound connections
- Fully managed and highly available

---

### Traffic Flow Using NAT Gateway
Private EC2 → Route Table → NAT Gateway → IGW → Internet

---





