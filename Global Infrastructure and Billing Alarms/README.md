# 🚀 AWS Learning Journey: Phase 1 - Global Infrastructure

## Goal
To understand the foundational physical and logical components that make up the AWS global footprint.

## Core Concepts Learned

| Component | Purpose & Resilience |
| :--- | :--- |
| **Region** | A geographic area hosting multiple AZs. Key for data sovereignty. |
| **Availability Zone (AZ)** | Isolated data centers within a Region. Provides fault tolerance. |
| **Edge Location** | Content delivery network (CDN) points to cache data for low latency. |

## 💡 Key Deliverable: High Availability Model

Building highly available infrastructure requires spanning services across multiple AZs. If AZ-A fails, the application automatically continues running in AZ-B.

```mermaid
graph TD
    A[Region: us-east-1] --> B(AZ-A)
    A --> C(AZ-B)
    B --> D[EC2 Instance / Server 1]
    C --> E[EC2 Instance / Server 2]
    subgraph High Availability
        D
        E
    end
