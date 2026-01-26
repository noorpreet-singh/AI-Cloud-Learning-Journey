# AWS EC2 – Core Concepts & Practical Guide

A concise, structured guide to **AWS EC2 (Elastic Compute Cloud)** covering instance families, security fundamentals, AMIs, and a basic hands-on deployment mindset.

---

## 📌 Contents

- EC2 Instance Families  
- Instance Naming Convention  
- Key Pairs (SSH Access)  
- Security Groups (Virtual Firewall)  
- Amazon Machine Images (AMIs)  
- Best Practices Summary  

---

## 💻 EC2 Instance Families

EC2 provides different **instance families**, each optimized for specific workloads.

| Category | Family | Best For |
|-------|------|--------|
| **General Purpose** | T, M | Web apps, small databases |
| **Compute Optimized** | C | CPU-intensive workloads |
| **Memory Optimized** | R, X, Z | In-memory databases, analytics |
| **Storage Optimized** | I, D, H | High I/O, data warehousing |
| **Accelerated Computing** | P, G, F | ML, graphics, HPC |

### Common Examples

| Instance | Use Case |
|-------|---------|
| `t3.micro` | Free Tier, testing |
| `m5.large` | Web servers |
| `c5.xlarge` | Batch processing |
| `r5.2xlarge` | Redis, SAP HANA |
| `i3.4xlarge` | NoSQL, logs |
| `g4dn.xlarge` | GPU inference |

---

## 📐 EC2 Instance Naming Convention

Example: `m5.2xlarge`

Family Generation Size

m 5 2xlarge


### Family Letters

| Letter | Meaning |
|------|-------|
| T | Burstable (low cost) |
| M | Balanced |
| C | Compute optimized |
| R | Memory optimized |
| I | High IOPS storage |
| G / P | GPU |
| F | FPGA |

### Common Suffixes

| Suffix | Meaning |
|------|-------|
| `a` | AMD CPU |
| `g` | AWS Graviton (ARM) |
| `n` | Network optimized |
| `d` | Instance store (NVMe) |

---

## 🔑 Key Pairs (SSH Access)

Key pairs enable **secure login** to EC2 instances.

### How It Works

- **Public key** → stored on EC2  
- **Private key (.pem)** → downloaded once by user  

Your PC (private key) ──SSH──> EC2 (public key)


### Key Points

- One-time download (AWS never stores private key)
- Region-specific
- Required for Linux & Windows access

### Best Practices

- Use separate keys for dev / prod  
- `chmod 400 key.pem`
- Never commit keys to Git
- Rotate keys periodically

---

## 🛡️ Security Groups

Security Groups act as **stateful virtual firewalls** for EC2.

### Characteristics

- Allow rules only (no deny)
- Stateful (return traffic allowed automatically)
- Applied at instance level
- Changes take effect immediately

### Common Ports

| Service | Port |
|------|----|
| SSH | 22 |
| HTTP | 80 |
| HTTPS | 443 |
| MySQL | 3306 |
| PostgreSQL | 5432 |

### Example: Web Server SG

```yaml
Inbound:
  - HTTP (80) → 0.0.0.0/0
  - HTTPS (443) → 0.0.0.0/0
  - SSH (22) → Your IP only
Outbound:
  - All traffic allowed

```
## 📀 Amazon Machine Images (AMIs)

An AMI is a template containing:

- OS
- Software
- Configuration 
- Storage mapping

Used to launch EC2 instances consistently.
## AMI Lifecycle
Launch → Customize → Create AMI → Reuse → Scale
## Storage Type
| Type | Persistence | Start/Stop |
|------|----| -----|
| EBS-backed | Persistence | Supporeted |
| Instance Store | TEmporary | Not Supported |

---

### Best Practices Summary

EC2
- Choose instance based on workload

- Prefer Graviton (g) for cost efficiency

- Right-size instances regularly
Security
- Never expose SSH/RDP to 0.0.0.0/0

- Use security group references for tiered apps

- Apply least-privilege rules
AMIs
- Use versioned golden images

- Patch and rebuild monthly

- Delete unused AMIs & snapshots
