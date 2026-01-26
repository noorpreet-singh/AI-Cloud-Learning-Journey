<img width="1024" height="1024" alt="Copilot_20260126_141904" src="https://github.com/user-attachments/assets/2d6e6f35-3612-4433-8f0b-b9b4dbe7ea36" />



```
Here’s the visual diagram you asked for — it lays out the AWS VPC architecture with all the key components clearly labeled and connected.
```

### 🔑 What the Diagram Shows

- VPC (10.0.0.0/16): The overall IP range enclosing everything.

- Availability Zones (A & B): Each AZ has one public and one private subnet.

- Public Subnets (/24):
1. Route Table → Internet Gateway (IGW).

2. NAT Gateway with Elastic IP.

3. Bastion Host (optional).

- Private Subnets (/24):

1. Route Table → NAT Gateway.

2. Database Instance (AZ A).

3. Database Replica (AZ B).

- Internet Gateway (IGW): Provides internet access for public subnets.

- NAT Gateways: Allow private subnets to reach the internet securely.

- High Availability: NAT Gateways deployed in both AZs for redundancy.
