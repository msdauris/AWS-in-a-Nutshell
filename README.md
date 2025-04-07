# AWS-in-a-Nutshell V2
A quick demonstration of essential AWS resources, including public and private static websites on S3, NAT instances, and VPC configurations.

#AWS Account
https://730492949633.signin.aws.amazon.com/console

# AWS Region
eu-west-1 Ireland

# NAT Marketplace
VNS3 NATe (NAT Gateway Appliance)
Free
by Cohesive Networks
Amazon Machine Image

| **Team**       | **Users**      | **Users**     | **Users** | **Users** |
|----------------|----------------|---------------|-----------|-----------|
| Demo Account   | soyred_admin   |               |           |           |
| Team 1         | Team1-User1    | Team1-User2   |  etc      |    etc    |
| Team 2         | Team2-User1    | Team2-User2   |  etc      |    etc    |

### CIDR Blocks, Availability Zones, Subnets and Route Tables

| **Team**       | **CIDR Block**  | **Availability Zone** | **Public Subnet**     | **Private Subnet**    | **Public Route Table**                | **Private Route Table**                            |
|----------------|-----------------|-----------------------|-----------------------|-----------------------|---------------------------------------|----------------------------------------------------|
| Demo Account   | 10.0.0.0/16     | eu-west-1c            | 10.0.0.0/24           | 10.0.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |
| Team 1         | 10.2.0.0/16     | eu-west-1a            | 10.2.0.0/24           | 10.2.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |
| Team 2         | 10.3.0.0/16     | eu-west-1b            | 10.3.0.0/24           | 10.3.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |
| Team 3         | 10.4.0.0/16     | eu-west-1c            | 10.4.0.0/24           | 10.4.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |
| Additional     | 10.5.0.0/16     |                       | 10.5.0.0/24           | 10.5.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |


### Security Group Rules

| **Team**       | **Security Group**    | **Inbound Rules**                                                                                                                      | **Outbound Rules**            |
|----------------|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| All Accounts   | Public SG             | Allow SSH (22) from your IP (Restricts EC2 Connect) & Allow HTTP 80 from  (0.0.0.0/0)                                                  | Allow all traffic (0.0.0.0/0) |
|                | Private SG            | Allow SSH (22) from Public SG/Private IP of Public aka Bastion Host                                                                    | Allow all traffic (0.0.0.0/0) |
|                | Nat SG                | Allow HTTP (80) from VPC CIDR<br>Allow HTTPS (443) from VPC CIDR<br>Allow SSH (22) from VPC CIDR<br>Allow ALL ICMP-IPv4 from 0.0.0.0/0 | Allow all traffic (0.0.0.0/0) |

### Step by Step

# 🧪 AWS Hands-On Workshop Guide

A step-by-step collection of hands-on tasks for participants across all workshop sessions.

---

## 1️⃣ **Intro + Console Customization (15–20 mins)**

### 🎯 Goals:
- Familiarize participants with AWS Console
- Customize UI and use bookmarks

### ✅ Tasks:
- Change console theme (dark/light)
- Add bookmarks for: IAM, EC2, S3, CloudWatch
- Explore:
  - Service search
  - Region selection dropdown

---

## 2️⃣ **IAM & Permissions (45–60 mins)**

### 🎯 Goals:
- Understand users, roles, and policies
- Practice least privilege
- Simulate access scenarios

### ✅ Tasks:
- From their **non-admin user**, create:
  - A new IAM user
  - A custom IAM role
  - A policy allowing **read-only S3 and EC2** actions
- Use **IAM Policy Simulator** to:
  - Test access to list S3 buckets
  - Explore deny vs allow behaviors
- (Optional) Attempt actions with insufficient permissions and fix using policy updates

📝 *Tip:* Pre-create an “elevated” role participants can assume when stuck.

---

## 3️⃣ **EC2 + EBS + Web Server (60–75 mins)**

### 🎯 Goals:
- Launch and connect to EC2
- Set up a basic web server
- Attach and use EBS volumes

### ✅ Tasks:
- Launch EC2 (Amazon Linux 2, t2.micro)
- Create or import SSH key pair
- Connect to instance via SSH
- Install web server:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```
- Attach EBS volume via console
- Format and mount the volume:

```bash
sudo mkfs -t xfs /dev/xvdf
sudo mkdir /data
sudo mount /dev/xvdf /data
```
---

## 4️⃣ **S3 Buckets + Permissions (30 mins)**

### 🎯 Goals:
- Understand S3 bucket structure and access control
- Upload and retrieve files

### ✅ Tasks:
- Create a new S3 bucket (with a unique name)
- Upload a sample file (e.g., image, CSV, or text)
- Adjust permissions to:
- Allow public read (optional)
- Use bucket policies or object ACLs
- Access uploaded file via public URL (if permissions allow)
- Explore versioning, lifecycle rules, and storage classes

📝 Tip: Compare Standard vs Infrequent Access or Glacier classes.

---

## 5️⃣ **ELB + Auto Scaling Group (60 mins)**

### 🎯 Goals:
- Balance traffic and scale EC2 instances automatically

### ✅ Tasks:
- Create a Launch Template (with User Data to install Apache):
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
echo "<h1>Server: $(hostname)</h1>" > /var/www/html/index.html
```
Create an Auto Scaling Group (ASG):
- Use the Launch Template
- Attach to 2 public subnets
- Set scaling policies (manual or CPU-based)

Create an Application Load Balancer (ALB):
- Internet-facing
- Target group pointing to ASG

Test Load Balancer in browser:
- Refresh to see different instance hostnames
- Kill one instance → watch ASG recover

---
## 6️⃣ **RDS (45 mins)**

### 🎯 Goals:
- Launch a managed database and connect to it securely

### ✅ Tasks:
Launch an RDS (MySQL or Postgres) instance:
- Use free-tier options
- Public access (yes) for easy connection
- Ensure security group allows port 3306 (MySQL) or 5432 (Postgres)

Connect from EC2 or a SQL client:
```sql
mysql -h <endpoint> -u admin -p
```
Run sample SQL:
```sql
CREATE DATABASE demo;
USE demo;
CREATE TABLE users (id INT, name VARCHAR(50));
INSERT INTO users VALUES (1, 'Alice'), (2, 'Bob');
SELECT * FROM users;
```
📝 Tip: Remind participants not to store real credentials/data.

## 7️⃣ **Athena + S3 Querying (30–45 mins)**

### 🎯 Goals:
- Use Athena to query CSV/JSON in S3 without provisioning servers

### ✅ Tasks:
Upload a sample CSV to S3:
- Example: user_id,name,role

Open AWS Glue Data Catalog:
- Create a database (e.g., workshop_data)
- Define a table using crawler or manually (CSV format)

Use Athena:
- Point to the Glue table
- Run sample queries:

```sql
SELECT * FROM workshop_data.users LIMIT 10;
```
📝 Tip: Show them how Athena charges per query scanned (watch the data size!).


---
## 🔚 **Wrap-Up (15–20 mins)**

✅ Tasks:
Recap key services used:
- IAM, EC2, EBS, S3, ELB, ASG, RDS, Athena

Share:
- Challenges faced
- Cool things learned

Optional:
- Issue certificates
- Invite feedback via Google Form or Jamboard
- Discuss next steps (certification paths, sandbox ideas)
---

---🧾 Optional Facilitator Setup
🔧 CloudTrail Setup (for auditing & troubleshooting)
Navigate to CloudTrail → Trails → Create Trail
Name: workshop-trail
- Select or create an S3 bucket for log delivery
- Enable for all regions
- Enable Management events (read & write)
- (Optional) Turn on Insights to track unusual activity
- Click Create – logging begins automatically

📝 Why? Helps troubleshoot issues (e.g., failed permissions) and gives visibility into student activity.
---ssh -T git@github.com


---✅ Final Checklist (Participant Self-Eval)
- [ ] Customized AWS Console (theme + bookmarks)
- [ ] Created IAM user, role, and policy
- [ ] Simulated access with IAM Policy Simulator
- [ ] Launched EC2 instance and connected via SSH
- [ ] Installed web server (Apache/Nginx)
- [ ] Attached EBS volume and mounted it
- [ ] Created and managed S3 bucket
- [ ] Used S3 permissions and tested access
- [ ] Set up ELB + Auto Scaling Group
- [ ] Verified load balancing and scaling behavior
- [ ] Deployed RDS and connected to it
- [ ] Ran SQL queries on RDS
- [ ] Queried structured S3 data using Athena
