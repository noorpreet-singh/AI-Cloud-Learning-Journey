# AWS S3 Storage & Static Website Hosting

This week focuses on **Amazon S3 (Simple Storage Service)**, AWS’s highly scalable object storage service, and how it is used to **store data and host static websites**.

The goal is to understand **S3 fundamentals**, **security concepts**, and deploy a **real static website** using S3.

---

## 🎯 Learning Objectives

By the end of this week, you will be able to:
- Understand how Amazon S3 works
- Create and manage S3 buckets and objects
- Control access using permissions
- Optimize storage using lifecycle rules
- Host a static website on S3

---

## 1️⃣ Amazon S3 (Simple Storage Service)

### What is S3?
Amazon S3 is an **object storage service** that allows you to store and retrieve **any amount of data** from anywhere on the internet.

### Key Characteristics
- Unlimited storage capacity
- Highly durable (99.999999999%)
- Highly available
- Cost-effective
- Secure by default

---

## 2️⃣ Core S3 Concepts

### 🪣 S3 Bucket

**Definition**  
A **bucket** is a **container** used to store objects in Amazon S3.

**Key Points**
- Bucket names must be **globally unique**
- Buckets are created in a **specific AWS region**
- Buckets act as the top-level namespace

**Example**

Amazon Resource Name (ARN)

arn:aws:s3:::noor-bucket-learn-100


---

### 📦 S3 Objects

**Definition**  
An **object** is the actual data stored in S3, consisting of:
- Object data (file)
- Metadata
- Unique key (object name)

**Examples**

<img width="1887" height="523" alt="Screenshot 2026-01-26 131020" src="https://github.com/user-attachments/assets/ec9822a6-fb4d-42e3-bfdd-4f3b0287d24a" />

---

## 3️⃣ S3 Permissions & Access Control

### Why Permissions Matter
S3 is **private by default**. Permissions define **who can access what**.

---

### 🔐 Bucket Policy

- JSON-based policy
- Applied at the bucket level
- Controls access for users, roles, or public access

**Example Use Case**
- Allow public read access for a static website

---

### 🔐 IAM Policy

- Attached to users or roles
- Controls access to S3 actions like:
  - `s3:GetObject`
  - `s3:PutObject`
  - `s3:ListBucket`

---

### 🔐 Public Access Block

- Prevents accidental public exposure
- Must be adjusted when hosting a public website
- Best practice: disable only what is required

---

## 4️⃣ S3 Storage Classes (Overview)

| Storage Class | Use Case |
|-------------|---------|
| Standard | Frequently accessed data |
| Intelligent-Tiering | Unknown access patterns |
| Standard-IA | Infrequent access |
| Glacier | Archival storage |

---

## 5️⃣ Lifecycle Rules

### What are Lifecycle Rules?
Lifecycle rules automate **transitioning or deleting objects** based on time.

### Why Use Them?
- Reduce storage cost
- Automatically move old data
- Enforce data retention policies

### Example Lifecycle Actions
- Move objects to Glacier after 30 days
- Delete objects after 365 days

---

