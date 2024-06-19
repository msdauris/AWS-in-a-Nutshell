# AWS-in-a-Nutshell
A quick demonstration of essential AWS resources, including public and private static websites on S3, NAT instances, and VPC configurations.

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
| Team 1         | Team1-User1    | Team1-User2   |           |           |
| Team 2         | Team2-User1    | Team2-User2   |           |           |

### CIDR Blocks, Availability Zones, and Subnets

| **Team**       | **CIDR Block**  | **Availability Zone** | **Public Subnet**     | **Private Subnet**    |
|----------------|-----------------|-----------------------|-----------------------|-----------------------|
| Demo Account   | 10.0.0.0/16     | us-east-1c            | 10.0.0.0/24           | 10.0.16.0/20          |
| Team 1         | 10.2.0.0/16     | us-east-1a            | 10.2.0.0/24           | 10.2.16.0/20          |
| Team 2         | 10.3.0.0/16     | us-east-1b            | 10.3.0.0/24           | 10.3.16.0/20          |

### Security Group Rules

| **Team**       | **Security Group**    | **Inbound Rules**                                                                                                                  | **Outbound Rules**            |
|----------------|-----------------------|------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| All Accounts   | Public SG             | Allow SSH (22) from your IP                                                                                                        | Allow all traffic (0.0.0.0/0) |
|                | Private SG            | Allow SSH (22) from Public SG                                                                                                      | Allow all traffic (0.0.0.0/0) |
|                | Nat SG                | Allow HTTP (80) from VPC CIDR<br>Allow HTTPS (443) from VPC CIDR<br>Allow SSH (22) from 0.0.0.0/0<br>Allow ALL ICMP-IPv4 from VPC CIDR | Allow all traffic (0.0.0.0/0) |

### Step by Step

| **Step** | **Description** | **Both Teams**  |
|----------|-----------------|-----------------|
| 1        | Login to AWS Console | Use provided credentials<br>Create users for the rest of your team<br>Attempt to create an user for another team |*|
| 2        | Create VPC | Create VPC with CIDR block as per table<br> Add VPC as a bookmark on your home page |
| 3        | Create Subnets | Create public subnet as per table<br>Create private subnet as per table<br>Keep your teams subnet with the same AZ<br>Enable Auto-assign public IP on public subnet |
| 4        | Create Route Tables | Public Route Table: add route to IGW<br>Associate with public subnet<br>Private Route Table<br>Associate with private subnet |*|
| 5        | Create Internet Gateway | Attach to Public route table<br>**HINT you'll need to edit the Route|*|
| 6        | Create Security Groups | Public & Private SG: See table<br>**TIP you can create these sg groups at same time as launching EC2  |*|
| 7        | Launch EC2 Instances | Attempt to launch a EC2 instance in different region than the one listed above<br>Launch 1 instance in each subnet, attach respective SG |
| 8        | Connect to EC2 Public instance | Connect via web console AND command line<br>**TIP Access Key required for CLI|
| 9        | Connect to EC2 Private instance from Public | Connect via web console and/or command line and try to access the internet! |*|
| 10       | Launch a NAT Instance | Launch a NAT instance as per table and connect to Private instance via Route Table <br>HINT Same CIDR as your teams VPC |
| 11       | Launch a NAT Gateway  | Launch a NAT gateway and connect to Private instance via Route Table |
| 12       | Create VPC Endpoint  | Create a VPC Endpoint<br>Type: Gateway and modify Route Table of Private |
| 13       | Terminate Network Connections  | Terminate the NAT Instance, NAT Gateway and Release Elastic IP |
| 14       | Create Public S3 Bucket | <br>Region: eu-west-1 |
| 15       | Create Private S3 Bucket | <br>Region: eu-west-1|
| 16       | Upload Website Files to Public Bucket | Upload Public.html |
| 17       | Upload Private Files to Private Bucket | Upload Private.html |
| 18       | Set Public Access Policy for Public Bucket | Enable public access |
| 19       | Set Bucket Policy for Private Bucket | Use IAM roles to restrict access |
| 20       | Enable Static Website Hosting on Public & Private Bucket | Use bucket properties to enable static website |
| 21       | Attach IAM Role to EC2 Instances | Attach created IAM role to access private bucket |
| 22       | Test Public Website Access | Access the public website URL of the other Team |
| 23       | Test Private Bucket Access via EC2 | SSH into EC2 Bastion Host, use AWS CLI to access private files |


