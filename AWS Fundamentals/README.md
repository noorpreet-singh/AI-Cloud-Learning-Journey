# AWS Fundamentals Guide

A comprehensive guide to AWS core concepts including Global Infrastructure, Shared Responsibility Model, and Billing essentials.

---

## 📍 AWS Global Infrastructure

AWS operates a vast global infrastructure designed for resilience, performance, and compliance.

### Key Components

| Component | Purpose & Resilience |
| :--- | :--- |
| **Region** | A geographic area hosting multiple AZs. Key for data sovereignty and compliance. |
| **Availability Zone (AZ)** | Isolated data centers within a Region. Provides fault tolerance and high availability. |
| **Edge Location** | Content delivery network (CDN) points to cache data for low latency access. |
| **Regional Edge Cache** | Intermediate caching layer between CloudFront edge locations and origin servers. |

### Why Regions Matter

| Consideration | Description |
| :--- | :--- |
| 🔒 **Compliance/Legal** | Meet data residency requirements (GDPR, HIPAA, etc.) |
| ⚡ **Latency** | Place resources closer to end users for better performance |
| 🛡️ **Disaster Recovery** | Isolate failures geographically for business continuity |
| 🌐 **Service Availability** | Not all AWS services are available in all regions |
| 💰 **Cost** | Pricing varies by region - choose strategically |

---

## 🏗️ Regions vs Availability Zones

### Architecture Overview
```
Region (e.g., us-east-1)
├── Availability Zone 1 (us-east-1a)
│   └── One or more data centers
├── Availability Zone 2 (us-east-1b)
│   └── One or more data centers
└── Availability Zone 3 (us-east-1c)
    └── One or more data centers
```

### Design Principles

| Concept | Details |
| :--- | :--- |
| **Region** | Cluster of multiple AZs in a geographic area (e.g., `us-east-1`, `eu-west-2`) |
| **AZ** | One or more discrete data centers with redundant power, networking, and connectivity |
| **Separation** | AZs are physically separated but connected via low-latency, high-bandwidth links |
| **Failover** | If one AZ fails, applications automatically failover to another AZ in the same region |
| **Best Practice** | ⚠️ **Always deploy across multiple AZs for high availability** |

---

## 🔐 Shared Responsibility Model

AWS security operates on a shared responsibility framework where both AWS and customers have distinct security obligations.

### AWS Responsibility: "Security OF the Cloud"
```
┌─────────────────────────────────────┐
│   AWS Manages Infrastructure        │
├─────────────────────────────────────┤
│ ✓ Physical infrastructure          │
│ ✓ Host OS & virtualization layer   │
│ ✓ Network infrastructure            │
│ ✓ Regions, AZs, Edge locations     │
│ ✓ Hardware & facility security      │
└─────────────────────────────────────┘
```

### Customer Responsibility: "Security IN the Cloud"
```
┌─────────────────────────────────────┐
│   You Manage Your Assets            │
├─────────────────────────────────────┤
│ ✓ Data encryption (at rest/transit)│
│ ✓ IAM configurations & policies     │
│ ✓ Guest OS & application security   │
│ ✓ Network traffic protection        │
│ ✓ Firewall & security group config │
│ ✓ Customer data management          │
└─────────────────────────────────────┘
```

### Visual Model

| Layer | Responsible Party |
| :--- | :---: |
| Customer Data | 👤 Customer |
| Application | 👤 Customer |
| Guest OS | 👤 Customer |
| AWS Services Configuration | 👤 Customer |
| **— Shared Responsibility Boundary —** | **— — —** |
| Hypervisor | ☁️ AWS |
| Network Infrastructure | ☁️ AWS |
| Physical Infrastructure | ☁️ AWS |

---

## 💳 Billing Basics

### Pay-As-You-Go Model

AWS follows a consumption-based pricing model with no upfront costs or long-term commitments.

| Resource Type | Billing Method | Example |
| :--- | :--- | :--- |
| **Compute** | Per second/hour | EC2 instances charged by runtime |
| **Storage** | Per GB-month | S3 storage at $0.023/GB/month |
| **Data Transfer** | Outbound only | Data OUT charges apply; inbound is FREE |
| **Requests** | Per API call | Lambda invocations, S3 GET requests |

### Cost Optimization Tips
```
💡 Key Principles:
   • Data transfer IN is always free
   • Data transfer OUT incurs charges
   • Inter-region transfers cost more than intra-region
   • Pricing varies significantly by region
   • Free Tier available for 12 months (with limitations)
```

### Free Tier Overview

| Service | Free Tier Limit |
| :--- | :--- |
| **EC2** | 750 hours/month (t2.micro or t3.micro) |
| **S3** | 5 GB storage, 20,000 GET requests |
| **Lambda** | 1M free requests/month |
| **RDS** | 750 hours/month (db.t2.micro) |

> ⚠️ **Important**: Always check region-specific pricing as costs vary significantly across geographic locations.

---

## 📚 Additional Resources

- [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS Pricing Calculator](https://calculator.aws/)
- [AWS Free Tier](https://aws.amazon.com/free/)

