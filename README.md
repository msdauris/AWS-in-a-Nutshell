# AWS-in-a-Nutshell
A quick demonstration of essential AWS resources, including public and private static websites on S3, NAT instances, and VPC configurations.

### CIDR Blocks, Availability Zones, and Subnets

| **Team**       | **CIDR Block**  | **Availability Zone** | **Public Subnet**     | **Private Subnet**    |
|----------------|-----------------|-----------------------|-----------------------|-----------------------|
| Demo Account   | 10.0.0.0/16     | us-east-1c            | 10.0.0.0/24           | 10.0.16.0/20          |
| Team 1         | 10.2.0.0/16     | us-east-1a            | 10.2.0.0/24           | 10.2.16.0/20          |
| Team 2         | 10.3.0.0/16     | us-east-1b            | 10.3.0.0/24           | 10.3.16.0/20          |

### Security Group Rules

| **Team**       | **Security Group**    | **Inbound Rules**                                                                                             | **Outbound Rules**            |
|----------------|-----------------------|---------------------------------------------------------------------------------------------------------------|-------------------------------|
| Demo Account   | Public SG             | Allow HTTP (80) from 0.0.0.0/0<br>Allow HTTPS (443) from 0.0.0.0/0<br>Allow SSH (22) from your IP             | Allow all traffic (0.0.0.0/0) |
|                | Private SG            | Allow HTTP (80) from 10.0.0.0/16<br>Allow HTTPS (443) from 10.0.0.0/16<br>Allow SSH (22) from Public SG       | Allow all traffic (0.0.0.0/0) |
| Team 1         | Public SG             | Allow HTTP (80) from 0.0.0.0/0<br>Allow HTTPS (443) from 0.0.0.0/0<br>Allow SSH (22) from your IP             | Allow all traffic (0.0.0.0/0) |
|                | Private SG            | Allow HTTP (80) from 10.2.0.0/16<br>Allow HTTPS (443) from 10.2.0.0/16<br>Allow SSH (22) from Public SG       | Allow all traffic (0.0.0.0/0) |
| Team 2         | Public SG             | Allow HTTP (80) from 0.0.0.0/0<br>Allow HTTPS (443) from 0.0.0.0/0<br>Allow SSH (22) from your IP             | Allow all traffic (0.0.0.0/0) |
|                | Private SG            | Allow HTTP (80) from 10.3.0.0/16<br>Allow HTTPS (443) from 10.3.0.0/16<br>Allow SSH (22) from Public SG       | Allow all traffic (0.0.0.0/0) |



| **Step** | **Description** | **Team 1** | **Team 2** |
|----------|-----------------|------------|------------|
| 1        | Login to AWS Console | Use provided credentials | Use provided credentials |
| 2        | Create VPC | Create VPC with CIDR block as per table | Create VPC with CIDR block as per table |
| 3        | Create Subnets | Create public subnet as per table<br>Create private subnet as per table | Create public subnet as per table<br>Create private subnet as per table |
| 4        | Create Route Tables | Public Route Table: add route to IGW<br>Associate with public subnet | Public Route Table: add route to IGW<br>Associate with public subnet |
| 5        | Create Internet Gateway | Attach to Public route table | Attach to Public route table |
| 6        | Create Security Groups | Public SG: Allow HTTP, HTTPS, SSH | Public SG: Allow HTTP, HTTPS, SSH |
| 7        | Launch EC2 Instances | Launch in public subnet, attach public SG | Launch in public subnet, attach public SG |
| 8        | Create S3 Bucket | Name: team1-public<br>Region: Choose nearest | Name: team2-public<br>Region: Choose nearest |
| 9        | Create Private S3 Bucket | Name: team1-private<br>Region: Choose nearest | Name: team2-private<br>Region: Choose nearest |
| 10       | Upload Website Files to Public Bucket | Upload HTML, CSS, JS files | Upload HTML, CSS, JS files |
| 11       | Upload Private Files to Private Bucket | Upload files meant for private access | Upload files meant for private access |
| 12       | Set Public Access Policy for Public Bucket | Enable public access | Enable public access |
| 13       | Set Bucket Policy for Private Bucket | Use IAM roles to restrict access | Use IAM roles to restrict access |
| 14       | Enable Static Website Hosting on Public Bucket | Use bucket properties to enable static website | Use bucket properties to enable static website |
| 15       | Configure Bucket Policy for Public Access | Set policy allowing public read | Set policy allowing public read |
| 16       | Create IAM Role for Private Access | Create IAM role with S3 access | Create IAM role with S3 access |
| 17       | Attach IAM Role to EC2 Instances | Attach created IAM role to access private bucket | Attach created IAM role to access private bucket |
| 18       | Test Public Website Access | Access the public website URL | Access the public website URL |
| 19       | Test Private Bucket Access via EC2 | SSH into EC2, use AWS CLI to access private files | SSH into EC2, use AWS CLI to access private files |


