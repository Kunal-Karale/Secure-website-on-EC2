# Secure Web Server on EC2

## Project overview
This project demonstrates deploying and securing a static website on an AWS EC2 instance using Apache (httpd). It covers core cloud fundamentals: compute provisioning, network security, IAM best practices, and web server configuration — skills directly relevant to Cloud Support Engineer, Linux Administrator, and NOC Engineer roles.

## Architecture

```
Internet
   |
   v
[Security Group: 22, 80]
   |
   v
[EC2 Instance - Amazon Linux]
   |
   +-- Apache (httpd) Web Server
   |       |
   |       v
   |   /var/www/html/index.html
   |
   +-- IAM Role attached (no hardcoded credentials)
```

## Tools and services used
- **Amazon EC2** — compute instance hosting the web server
- **Security Groups** — network-level firewall (inbound rules restricted to SSH, HTTP, HTTPS)
- **IAM Role** — attached to the instance for secure, credential-free access to AWS services
- **Apache (httpd)** — web server software serving the static page


## Steps performed

### 1. Launch EC2 instance
- Amazon Linux 2 AMI selected
- Instance type: t3.micro (Free Tier eligible)
- Security Group configured with inbound rules:
  - Port 22 (SSH) — restricted to my IP
  - Port 80 (HTTP) — IPV4

### 2. Connect via SSH
```bash
ssh -i project-key.pem ec2-user@<13.232.29.44 >
ssh -i project-key.pem ec2-user@<13.201.22.232 >
```



### 3. Install Apache (httpd)
```bash
sudo yum update -y
sudo yum install httpd -y
```

### 4. Start and enable the service
```bash
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

### 5. Deploy the website
```bash
scp -i project-key.pem index.html ec2-user@<13.232.29.44 >: /var/www/html/

scp -i project-key.pem index.html ec2-user@<13.201.22.232 >: /var/www/html/

sudo mv /tmp/index.html /var/www/html/
sudo chown apache:apache /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html
```

### 6. Verify
- Opened `http://< 13.232.29.44 >` in browser
- Opened `http://< 13.201.22.232 >` in browser

- Confirmed the page loads correctly with the web server active

## Security measures applied
- Security Group restricted to only necessary ports (22, 80)
- SSH access limited to a specific IP rather than 0.0.0.0/0
- IAM role used instead of hardcoded access keys
- File permissions set correctly on web root

## Outcome
Successfully deployed a live, secured static website on an EC2 instance, demonstrating end-to-end skills across compute, networking, IAM, and web server administration.

## Possible future enhancements
- Add HTTPS using Let's Encrypt / Certbot or ACM + Application Load Balancer
- Map a custom domain using Route 53
- Automate provisioning using a Bash script or Terraform
- Add CloudWatch monitoring and alarms for the instance

---
**Author:** Kunal Karale
**GitHub:** github.com/Dhopati/Aws---projects
**LinkedIn:** linkedin.com/in/kunal-karale4001

