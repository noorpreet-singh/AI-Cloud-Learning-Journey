```markdown
# AWS EC2 (Elastic Compute Cloud)

Complete guide to AWS EC2 including instance types, security, AMIs, and hands-on web server deployment.

---

## 📋 Table of Contents

- [EC2 Instance Types](#-ec2-instance-types-the-compute-family)
- [Key Pairs](#-key-pairs)
- [Security Groups](#-security-groups)
- [Amazon Machine Images (AMIs)](#-amazon-machine-images-amis)
- [Hands-On: Launch EC2 & Host Web Page](#-hands-on-launch-ec2--host-web-page)

---

## 💻 EC2 Instance Types (The Compute Family)

EC2 offers various instance types optimized for different use cases. Choose based on your workload requirements.

### Instance Categories

| Category | Series | Use Case | Key Feature |
| :--- | :---: | :--- | :--- |
| **General Purpose** | M, T | Web servers, small databases | Balanced compute, memory, networking |
| **Compute Optimized** | C | Gaming, batch processing | High-performance processors |
| **Memory Optimized** | R, X, Z | In-memory databases, analytics | Fast performance for large datasets |
| **Storage Optimized** | I, D, H | NoSQL databases, data warehousing | High sequential read/write access |
| **Accelerated Computing** | P, G, F | Machine learning, graphics rendering | Hardware accelerators (GPUs, FPGAs) |

---

### 1️⃣ General Purpose (M Series)

```
┌─────────────────────────────────────┐
│      General Purpose Instances       │
├─────────────────────────────────────┤
│ Balance: CPU, Memory, Networking     │
│ Best For: Most workloads             │
└─────────────────────────────────────┘
```

| Instance Type | vCPUs | Memory | Use Case |
| :--- | :---: | :---: | :--- |
| **t3.micro** | 2 | 1 GB | Free Tier, development |
| **t3.small** | 2 | 2 GB | Small apps, testing |
| **m5.large** | 2 | 8 GB | Web servers, small DBs |
| **m5.xlarge** | 4 | 16 GB | Mid-tier applications |

**Common Use Cases:**
- Web application servers
- Small to medium databases
- Development/test environments
- Microservices

---

### 2️⃣ Compute Optimized (C Series)

```
┌─────────────────────────────────────┐
│     Compute Optimized Instances      │
├─────────────────────────────────────┤
│ Focus: High-performance processors   │
│ Best For: CPU-intensive workloads    │
└─────────────────────────────────────┘
```

| Instance Type | vCPUs | Memory | Performance |
| :--- | :---: | :---: | :--- |
| **c5.xlarge** | 4 | 8 GB | High compute |
| **c6g.2xlarge** | 8 | 16 GB | Graviton (ARM-based) |
| **c7g.4xlarge** | 16 | 32 GB | Latest generation |

**Common Use Cases:**
- Batch processing workloads
- Gaming servers
- High-performance web servers
- Scientific modeling
- Ad serving engines
- Video encoding

---

### 3️⃣ Memory Optimized (R/X/Z Series)

```
┌─────────────────────────────────────┐
│     Memory Optimized Instances       │
├─────────────────────────────────────┤
│ Focus: Large in-memory datasets      │
│ Best For: Memory-intensive apps      │
└─────────────────────────────────────┘
```

| Instance Type | vCPUs | Memory | RAM:vCPU Ratio |
| :--- | :---: | :---: | :---: |
| **r5.2xlarge** | 8 | 64 GB | 8:1 |
| **r6g.4xlarge** | 16 | 128 GB | 8:1 |
| **x1e.32xlarge** | 128 | 3,904 GB | ~30:1 |
| **z1d.6xlarge** | 24 | 192 GB | High frequency |

**Common Use Cases:**
- In-memory databases (Redis, Memcached)
- Real-time big data analytics
- High-performance databases (SAP HANA)
- Distributed web caches
- Genome assembly

---

### 4️⃣ Storage Optimized (I/D/H Series)

```
┌─────────────────────────────────────┐
│     Storage Optimized Instances      │
├─────────────────────────────────────┤
│ Focus: High IOPS, low latency        │
│ Best For: I/O intensive workloads    │
└─────────────────────────────────────┘
```

| Instance Type | vCPUs | Storage | IOPS Performance |
| :--- | :---: | :---: | :--- |
| **i3.4xlarge** | 16 | 3.8 TB NVMe SSD | Millions of IOPS |
| **d2.xlarge** | 4 | 6 TB HDD | High throughput |
| **h1.8xlarge** | 32 | 16 TB HDD | Sequential I/O |

**Common Use Cases:**
- NoSQL databases (Cassandra, MongoDB)
- Data warehousing
- Distributed file systems (HDFS)
- Log processing
- Elasticsearch clusters

---

### 5️⃣ Accelerated Computing (P/G/F Series)

```
┌─────────────────────────────────────┐
│   Accelerated Computing Instances    │
├─────────────────────────────────────┤
│ Focus: Hardware accelerators         │
│ Best For: ML, graphics, HPC          │
└─────────────────────────────────────┘
```

| Instance Type | Accelerator | Use Case |
| :--- | :--- | :--- |
| **p3.2xlarge** | NVIDIA V100 GPU | Deep learning training |
| **p4d.24xlarge** | NVIDIA A100 GPU | Large-scale ML |
| **g4dn.xlarge** | NVIDIA T4 GPU | Graphics, inference |
| **f1.2xlarge** | Xilinx FPGA | Custom hardware acceleration |
| **inf1.xlarge** | AWS Inferentia | ML inference |

**Common Use Cases:**
- Machine learning training/inference
- Graphics rendering
- Video transcoding
- Computational fluid dynamics
- Seismic analysis

---

### 📐 Instance Naming Convention

Understanding EC2 instance names:

```
    m5.2xlarge
    │ │  └────── Size (nano, micro, small, medium, large, xlarge, 2xlarge, etc.)
    │ └─────── Generation (1, 2, 3, 4, 5, 6, 7, etc.)
    └───────── Instance Family
```

#### Instance Family Letters

| Letter | Family | Purpose |
| :---: | :--- | :--- |
| **T** | General Purpose | Burstable performance |
| **M** | General Purpose | Balanced resources |
| **C** | Compute Optimized | High CPU performance |
| **R** | Memory Optimized | High memory capacity |
| **X** | Memory Optimized | Extreme memory (SAP HANA) |
| **Z** | Memory Optimized | High frequency + memory |
| **I** | Storage Optimized | High IOPS (NVMe SSD) |
| **D** | Storage Optimized | Dense HDD storage |
| **H** | Storage Optimized | High disk throughput |
| **P** | Accelerated | GPU for ML training |
| **G** | Accelerated | GPU for graphics |
| **F** | Accelerated | FPGA |

#### Additional Suffixes

| Suffix | Meaning | Example |
| :---: | :--- | :--- |
| **a** | AMD processors | m5a.large |
| **g** | AWS Graviton (ARM) | c6g.xlarge |
| **n** | Network optimized | c5n.large |
| **d** | Instance store (NVMe) | m5d.large |

---

## 🔑 Key Pairs

SSH credentials for secure access to EC2 instances.

### What are Key Pairs?

```
┌─────────────────────────────────────┐
│         Key Pair Structure           │
├─────────────────────────────────────┤
│ Public Key:  Stored on EC2 instance  │
│ Private Key: Downloaded by user      │
│ Purpose:     SSH authentication      │
└─────────────────────────────────────┘
```

### Architecture

```
Your Computer                    EC2 Instance
─────────────                    ────────────
Private Key (.pem)  ──SSH──>    Public Key
~/.ssh/my-key.pem               ~/.ssh/authorized_keys
```

### Key Features

| Feature | Description |
| :--- | :--- |
| **One-Time Download** | AWS doesn't store private key after creation |
| **Regional** | Key pairs are region-specific |
| **Immutable** | Cannot be changed once created (must create new) |
| **Format (Linux)** | PEM format (OpenSSH compatible) |
| **Format (Windows)** | PPK format (PuTTY) - convert from PEM |
| **Storage Location** | Public key in `~/.ssh/authorized_keys` on instance |

### Security Best Practices

```yaml
✅ DO:
  - Use different key pairs for different environments (dev/prod)
  - Store private keys in secure, encrypted location
  - Set restrictive permissions (chmod 400 key.pem)
  - Use key pairs with passphrases for extra security
  - Rotate keys periodically (every 90-180 days)
  - Delete unused key pairs

❌ DON'T:
  - Share private keys via email or chat
  - Commit keys to version control (Git)
  - Use same key pair across all environments
  - Store keys in public cloud storage
  - Leave keys with open permissions (chmod 777)
```

### Creating and Using Key Pairs

**Create via AWS Console:**
```
EC2 Dashboard → Key Pairs → Create Key Pair
├── Name: my-ec2-key
├── Type: RSA (most compatible)
└── Format: .pem (Linux/Mac) or .ppk (Windows)
```

**Create via AWS CLI:**
```bash
# Create new key pair
aws ec2 create-key-pair \
    --key-name my-ec2-key \
    --query 'KeyMaterial' \
    --output text > my-ec2-key.pem

# Set correct permissions
chmod 400 my-ec2-key.pem
```

**Convert PEM to PPK (for PuTTY):**
```
1. Open PuTTYgen
2. Load → Select .pem file
3. Save Private Key → Save as .ppk
```

---

## 🛡️ Security Groups

Virtual firewalls that control inbound and outbound traffic for EC2 instances.

### Definition

```
┌─────────────────────────────────────┐
│        Security Groups               │
├─────────────────────────────────────┤
│ Type:     Virtual firewall           │
│ State:    Stateful (auto-return)     │
│ Scope:    Region and VPC specific    │
│ Attach:   Multiple instances         │
└─────────────────────────────────────┘
```

### Key Characteristics

| Feature | Description |
| :--- | :--- |
| **Stateful** | Return traffic automatically allowed (no need for explicit outbound rules) |
| **Allow Only** | Can only create ALLOW rules (no DENY rules) |
| **Default Behavior** | All inbound ❌ blocked, all outbound ✅ allowed |
| **Immediate Effect** | Changes apply instantly to all associated instances |
| **Multiple Attachments** | One security group → many instances |
| **Chaining** | Can reference other security groups as sources |

### Rule Structure

```yaml
Type:        SSH, HTTP, HTTPS, Custom TCP, Custom UDP, All Traffic
Protocol:    TCP, UDP, ICMP, ICMPv6
Port Range:  Single port (22) or range (1024-65535)
Source:      CIDR block, security group ID, prefix list
Description: Optional human-readable description
```

### Common Port Numbers

| Service | Protocol | Port | Security Group Rule |
| :--- | :---: | :---: | :--- |
| **SSH** | TCP | 22 | Linux remote access |
| **RDP** | TCP | 3389 | Windows remote access |
| **HTTP** | TCP | 80 | Web traffic (unencrypted) |
| **HTTPS** | TCP | 443 | Web traffic (encrypted) |
| **MySQL** | TCP | 3306 | MySQL database |
| **PostgreSQL** | TCP | 5432 | PostgreSQL database |
| **SMTP** | TCP | 25 | Email (outbound) |
| **DNS** | UDP | 53 | Domain name resolution |
| **Custom** | TCP/UDP | Any | Application-specific |

### Example Configurations

#### Web Server Security Group

```yaml
Name: WebServer-SG
Description: Security group for public web servers

Inbound Rules:
  - Type: HTTP
    Protocol: TCP
    Port: 80
    Source: 0.0.0.0/0 (anywhere)
    Description: Allow public web traffic

  - Type: HTTPS
    Protocol: TCP
    Port: 443
    Source: 0.0.0.0/0 (anywhere)
    Description: Allow secure web traffic

  - Type: SSH
    Protocol: TCP
    Port: 22
    Source: 203.0.113.0/24 (your office IP)
    Description: Admin access from office only

Outbound Rules:
  - Type: All Traffic
    Protocol: All
    Port: All
    Destination: 0.0.0.0/0
    Description: Allow all outbound traffic
```

#### Database Security Group

```yaml
Name: Database-SG
Description: Security group for RDS MySQL database

Inbound Rules:
  - Type: MySQL/Aurora
    Protocol: TCP
    Port: 3306
    Source: sg-xxxxx (WebServer-SG)
    Description: Allow traffic from web servers only

Outbound Rules:
  - Type: All Traffic
    Protocol: All
    Port: All
    Destination: 0.0.0.0/0
```

### Security Group Best Practices

```yaml
🔒 Security Best Practices:

1. Principle of Least Privilege:
   - Only open ports that are absolutely necessary
   - Restrict source IPs as much as possible
   - Use specific CIDR ranges instead of 0.0.0.0/0

2. Never Use 0.0.0.0/0 for:
   - SSH (port 22) - use your IP or VPN
   - RDP (port 3389) - use your IP or VPN
   - Database ports (3306, 5432, etc.)

3. Naming Convention:
   - Use descriptive names: WebServer-SG, Database-SG
   - Include environment: Prod-WebServer-SG
   - Add descriptions to all rules

4. Regular Audits:
   - Review security groups quarterly
   - Remove unused security groups
   - Check for overly permissive rules
   - Use AWS Config for compliance

5. Reference Other Security Groups:
   - Allow traffic between tiers using SG IDs
   - Example: App tier → Database tier
   - No need for IP management
```

### Security Group vs. Network ACL

| Feature | Security Group | Network ACL |
| :--- | :---: | :---: |
| **Level** | Instance | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow + Deny |
| **Rule Processing** | All rules evaluated | Rules in order |
| **Applies To** | Instances with SG attached | All instances in subnet |

---

## 📀 Amazon Machine Images (AMIs)

Pre-configured templates containing the OS, application server, and applications needed to launch EC2 instances.

### What is an AMI?

```
┌─────────────────────────────────────┐
│      Amazon Machine Image (AMI)      │
├─────────────────────────────────────┤
│ Contains:                            │
│  • Operating System                  │
│  • Application Server                │
│  • Applications                      │
│  • Configuration                     │
│  • Storage mapping                   │
└─────────────────────────────────────┘
```

### AMI Characteristics

| Feature | Description |
| :--- | :--- |
| **Regional** | AMIs are region-specific (copy to other regions if needed) |
| **Template** | Blueprint for launching instances |
| **Reusable** | Launch unlimited instances from one AMI |
| **Shareable** | Can be shared across AWS accounts |
| **Versioning** | Create multiple versions as you update |

---

### AMI Types

#### 1️⃣ Amazon Quick Start AMIs

```
Official AWS-maintained images
├── Amazon Linux 2023
├── Amazon Linux 2
├── Ubuntu 22.04 LTS
├── Red Hat Enterprise Linux (RHEL)
├── SUSE Linux Enterprise Server
└── Windows Server (2016, 2019, 2022)
```

**Use When:** Starting fresh with standard OS

#### 2️⃣ AWS Marketplace AMIs

```
Third-party pre-configured solutions
├── WordPress (LAMP stack)
├── Jenkins CI/CD
├── GitLab
├── MongoDB
├── Nginx Plus
└── Commercial software (licensed)
```

**Use When:** Need pre-configured software stacks

#### 3️⃣ Community AMIs

```
User-contributed images
├── Community Ubuntu builds
├── Custom Linux distributions
├── Pre-configured dev environments
└── Specialized configurations
```

**Use When:** Looking for specific configurations
**⚠️ Warning:** Verify source before using

#### 4️⃣ Private AMIs

```
Your custom images
├── Company-standard builds
├── Golden images
├── Pre-configured applications
└── Compliance-hardened OS
```

**Use When:** Deploying standardized infrastructure

---

### AMI Lifecycle

```
┌─────────────────────────────────────────────────────────┐
│                    AMI Lifecycle                        │
└─────────────────────────────────────────────────────────┘

1. Launch Instance
   ↓
2. Customize (install software, configure)
   ↓
3. Create Image (snapshot of current state)
   ↓
4. Store AMI (registered in your account)
   ↓
5. Launch from Image (instant deployment)
   ↓
6. Repeat as needed (scale out)
```

### Creating Custom AMI

**Via AWS Console:**
```
1. Launch base instance (e.g., Amazon Linux 2)
2. SSH into instance and customize:
   - Install applications
   - Configure settings
   - Apply security hardening
3. EC2 Dashboard → Instances → Select instance
4. Actions → Image and templates → Create image
5. Provide:
   - Image name: MyApp-AMI-v1.0
   - Description: Production web server image
6. Create Image
7. Wait for AMI status: Available
```

**Via AWS CLI:**
```bash
# Create AMI from running instance
aws ec2 create-image \
    --instance-id i-1234567890abcdef0 \
    --name "MyApp-AMI-v1.0" \
    --description "Production web server" \
    --no-reboot

# Copy AMI to another region
aws ec2 copy-image \
    --source-region us-east-1 \
    --source-image-id ami-0abcdef1234567890 \
    --name "MyApp-AMI-v1.0" \
    --region us-west-2
```

---

### AMI Storage Types

| Storage Type | Description | Persistence | Stop/Start | Pricing |
| :--- | :--- | :---: | :---: | :--- |
| **EBS-backed** | Elastic Block Store volumes | ✅ Persistent | ✅ Allowed | Volume + snapshot storage |
| **Instance Store** | Ephemeral storage on host | ❌ Temporary | ❌ Not allowed | Included in instance price |

#### EBS-Backed AMI

```
Characteristics:
✅ Data persists when instance is stopped
✅ Can be stopped and started
✅ Faster boot time (EBS snapshot)
✅ Can change instance type
✅ Can enable termination protection

Use Cases:
• Production workloads
• Databases
• Stateful applications
```

#### Instance Store-Backed AMI

```
Characteristics:
⚠️ Data lost when instance stops/terminates
⚠️ Cannot stop instance (only reboot/terminate)
⚠️ Lost if underlying hardware fails
✅ Very high I/O performance
✅ No additional storage cost

Use Cases:
• Temporary data processing
• Caching layers   
• Scratch data
```

---

### AMI Best Practices

```yaml
🎯 Best Practices:

1. Golden Images:
   - Create standardized AMIs for each environment
   - Include security patches and hardening
   - Version AMIs (v1.0, v1.1, v2.0)
   - Document changes in descriptions

2. Regular Updates:
   - Rebuild AMIs monthly with latest patches
   - Test new AMIs before production deployment
   - Retire old AMIs after migration

3. Organization:
   - Use naming conventions: {app}-{env}-{version}
   - Example: webapp-prod-v1.2.3
   - Tag AMIs with metadata (owner, team, purpose)

4. Security:
   - Remove sensitive data before creating AMI
   - Encrypt EBS volumes
   - Set appropriate launch permissions
   - Regularly scan AMIs for vulnerabilities

5. Cost Management:
   - Delete unused AMIs
   - Delete associated snapshots
   - Use AMI lifecycle policies
   - Monitor AMI usage with tags
```

---

