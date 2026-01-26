





## Launch Commands (via Console):
1. Selected AMI: Amazon Linux 2
2. Instance Type: t2.micro
3. Security Group: WebServer-SG
   - Inbound: SSH (22) from My IP
   - Inbound: HTTP (80) from 0.0.0.0/0

## SSH Connection:
# For Linux or Mac
chmod 400 week3-key.pem

# For Windows(powershell)
icacls "week3-key.pem" /reset
icacls "week3-key.pem" /grant:r "$($env:username):(R)"
icacls "week3-key.pem" /inheritance:r

ssh -i week3-key.pem ec2-user@<PUBLIC_IP>

## Web Server Setup:
sudo apt-get update(Install Latest updates)
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

## HTML Page:
Created /var/www/html/index.html with instance metadata

## Verification:
curl http://localhost
System working: http://<PUBLIC_IP> accessible

## After Successfully connection your windows desktsop with EC2 Instance 
<img width="1343" height="369" alt="Screenshot 2026-01-01 144625" src="https://github.com/user-attachments/assets/61dad1b6-adaa-41ba-9446-3e2e892fc3df" />






