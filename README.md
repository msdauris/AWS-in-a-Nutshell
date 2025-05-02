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

| **#** | **step** | **description**  |**details**|
|----------|-----------------|-----------------|------|
| 1        | Login to AWS Console | Change console theme (dark/light)<br>Add bookmarks for: IAM, EC2, S3, CloudWatch<br>Explore service search, region selection<br>|*|
| 2        | **Create Two Users**:<br>Create two users within the appropriate IAM demo groups: [Developers Group] <br>[QA Group] | Examine the policies attached to each group.<br>Simulate each user's access to various resources using the IAM Policy Simulator.|Developers should have permissions to **read/write to AWS CodeCommit** and deploy code to **Elastic Beanstalk or EC2** QA Engineers should have **read-only access to CodeCommit**, and permissions to view logs in **CloudWatch** and **X-Ray** for troubleshooting.|
| 3        | Use IAM Policy Simulator:<br>- Simulate each user's access to various resources using the **IAM Policy Simulator**.For Developers: Verify if they can **push code** to the repository.<br>For QA Engineers: Verify if they can access **CloudWatch logs** and troubleshoot issues. | **Test and Modify Permissions**: After simulating actions, if access is denied, adjust the IAM policies to provide the correct permissions.<br>For example, give the **QA Engineers** permission to access **CloudWatch** logs or allow **Developers** to push code to CodeCommit. |*|
| 4        | TEXT | TEXT |*|
| 5        | TEXT | **HINT** TEXT |*|

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
