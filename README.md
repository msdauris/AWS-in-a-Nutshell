# AWS-in-a-Nutshell
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
| Additional     | 10.4.0.0/16     |                       | 10.4.0.0/24           | 10.4.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |
| Additional     | 10.5.0.0/16     |                       | 10.5.0.0/24           | 10.5.16.0/20          | 0.0.0.0/0 via Internet Gateway        | 0.0.0.0/0 via NAT Instance/Gateway or VPC Endpoint |


### Security Group Rules

| **Team**       | **Security Group**    | **Inbound Rules**                                                                                                                      | **Outbound Rules**            |
|----------------|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| All Accounts   | Public SG             | Allow SSH (22) from your IP (Restricts EC2 Connect) & Allow HTTP 80 from  (0.0.0.0/0)                                                  | Allow all traffic (0.0.0.0/0) |
|                | Private SG            | Allow SSH (22) from Public SG/Private IP of Public aka Bastion Host                                                                    | Allow all traffic (0.0.0.0/0) |
|                | Nat SG                | Allow HTTP (80) from VPC CIDR<br>Allow HTTPS (443) from VPC CIDR<br>Allow SSH (22) from VPC CIDR<br>Allow ALL ICMP-IPv4 from 0.0.0.0/0 | Allow all traffic (0.0.0.0/0) |

### Step by Step

| **Step** | **Description** | **Both Teams**  |
|----------|-----------------|-----------------|
| 1        | Login to AWS Console | Use provided credentials<br>Create users for the rest of your team<br>Attempt to create an user for another team |*|
| 2        | Create VPC | Create VPC with CIDR block as per table<br> Add VPC as a bookmark on your home page |
| 3        | Create Subnets | Create public subnet as per table<br>Create private subnet as per table<br>Keep your teams subnet with the same AZ<br>Enable Auto-assign public IP on public subnet |
| 4        | Create Route Tables | Public Route Table: add route to IGW<br>Associate with public subnet<br>Private Route Table<br>Associate with private subnet |*|
| 5        | Create Internet Gateway | Attach to Public route table<br>**HINT** you'll need to edit the Route|*|
| 6        | Create Security Groups | Public & Private SG: See table<br>**TIP** you can create these sg groups at same time as launching EC2  |*|
| 7        | Launch EC2 Instances | Attempt to launch a EC2 instance in different region than the one listed above<br>Launch 1 instance in each subnet, attach respective SG |
| 8        | Connect to EC2 Public instance | Connect via web console AND command line<br>**TIP** Access Key required for CLI|*|
| 9        | Connect to EC2 Private instance from Public | Connect via web console and/or command line and try to access the internet! |*|
| 10       | Launch a NAT Instance in public subnet | Launch a NAT instance as per table and connect to Private instance via Route Table<br>INFO Change Source / destination check must be stopped |*|>**TIP** USE VNS3 NATe (NAT Gateway Appliance)
| 11       | Launch a NAT Gateway  | Launch a NAT gateway and connect to Private instance via Route Table |
| 12       | Create VPC Endpoint  | Create a VPC Endpoint<br>Type: Gateway and add to PRIVATE subnet!!<br> Amend local region '''aws s3 ls --region eu-west-1'''<br>RUN '''aws s3 ls'''<br>**TIP** you may require DEMO IAM role or try aws configure with your access key ;)|
| 13       | Terminate Network Connections  | Terminate the NAT Instance, NAT Gateway and Release Elastic IP|
| 14       | Create Public S3 Bucket | <br>Region: eu-west-1 |
| 15       | Create Private S3 Bucket | <br>Region: eu-west-1|
| 16       | Enable Static Website Hosting on Public & Private Bucket | Use bucket properties to enable static website |
| 17       | Upload Website Files to Public Bucket | Upload Public.html |
| 18       | Upload Private Files to Private Bucket | Upload Private-team1.html<br>Upload Private-team2.html |
| 19       | Set Public Access Policy for Public Bucket | Enable public access but set policy - see demo |
| 20       | Set Bucket Policy for Private Bucket | Enable public access but set policy - see demo |*|
| 21       | Test Public Website Access | Access the public website URL of the other Team<br>**TIP** check Demo public bucket for policies  |
| 22       | Test Private Bucket Access via EC2 | SSH into EC2 Bastion Host, use AWS CLI to access private files<br>**TIP** check Demo private bucket for policies |
| 23       | Connect All three VPCs | **HINT** Peering connection<br>Edit Route tables<br>Try to access the private.html document of the other Team<br>What happens?  |*|
| 24       | Quickover view of VPC Flow logs | **TIP** View VPC details  |*|
| 25       | Clean up | Terminate your instances, gateways, release elastic ips, etc  |*|



