# Deploying a MERN Application on AWS using Terraform and Ansible

## Project Overview

This project demonstrates the deployment of a MERN (MongoDB, Express.js, React.js, Node.js) application on AWS using Infrastructure as Code (IaC) and Configuration Management tools.

### Objectives

* Provision AWS infrastructure using Terraform.
* Automate server configuration using Ansible.
* Deploy a MERN application.
* Configure secure networking using VPC, Subnets, Security Groups, NAT Gateway, and IAM Roles.
* Follow infrastructure automation best practices.

### MERN Application

Repository Used:

TravelMemory Application

https://github.com/UnpredictablePrashant/TravelMemory

---

# Architecture

## AWS Infrastructure

* Custom VPC
* Public Subnet
* Private Subnet
* Internet Gateway
* NAT Gateway
* Route Tables
* Web Server EC2 Instance
* Database Server EC2 Instance
* Security Groups
* IAM Roles

### Architecture Flow

The web server is publicly accessible while the database server remains isolated inside the private subnet.
Infrastructure Diagram
                    Internet
                        |
                Internet Gateway
                        |
          -----------------------------
          |                           |
     Public Subnet              NAT Gateway
          |                           |
          |                           |
   Web Server EC2                     |
          |                           |
          -----------------------------
                        |
                 Private Subnet
                        |
                MongoDB EC2 Server
                
---

# Part 1: Infrastructure Provisioning using Terraform

## AWS Configuration

Configured AWS CLI using:

aws configure

Configured:

* AWS Access Key
* AWS Secret Key
* Region: ap-south-1
* Output Format: json
<img width="970" height="207" alt="Screenshot 2026-06-06 at 4 44 16 PM" src="https://github.com/user-attachments/assets/1812525f-192a-4c21-8e18-b5c0cffc4071" />

---

## Terraform Files

### provider.tf

Configures AWS Provider.
<img width="973" height="356" alt="Screenshot 2026-06-06 at 4 45 19 PM" src="https://github.com/user-attachments/assets/2cf10c22-575c-4c28-89a3-22859c4f5474" />

### variables.tf

Stores reusable variables.
<img width="1936" height="510" alt="image" src="https://github.com/user-attachments/assets/09b740a5-8753-4b24-aeb9-62ac9232b48f" />

### vpc.tf

Creates:

* VPC
* Public Subnet
* Private Subnet
* Internet Gateway
* NAT Gateway
* Route Tables

<img width="973" height="182" alt="Screenshot 2026-06-06 at 4 45 42 PM" src="https://github.com/user-attachments/assets/c846bae9-29a7-4457-b64d-2c153c5140a3" />
<img width="973" height="623" alt="Screenshot 2026-06-06 at 4 46 03 PM" src="https://github.com/user-attachments/assets/f3de3936-4005-43f6-8b78-abfde31833cb" />

### security.tf

Creates:

#### Web Security Group

Allows:

* SSH (22) from My IP
* HTTP (80) from Anywhere
* HTTPS (443) from Anywhere

#### Database Security Group

Allows:

* MongoDB (27017) from Web Security Group
* SSH (22) from Web Security Group
<img width="973" height="473" alt="Screenshot 2026-06-06 at 4 50 11 PM" src="https://github.com/user-attachments/assets/ffe1c8ec-7302-4681-97b1-e64352f520b8" />

### iam.tf

Creates IAM Roles and Policies for EC2 instances.
<img width="972" height="90" alt="Screenshot 2026-06-06 at 4 50 58 PM" src="https://github.com/user-attachments/assets/4684b65e-2d34-4b83-93f9-7b030d066311" />

### ec2.tf

Creates:

* Web Server EC2 Instance
* Database EC2 Instance
<img width="1946" height="1268" alt="image" src="https://github.com/user-attachments/assets/b6e3ea16-949c-4a88-b180-51dc5e2f6849" />
<img width="972" height="299" alt="Screenshot 2026-06-06 at 10 27 37 PM" src="https://github.com/user-attachments/assets/ebaea9ea-e246-4292-a046-0e94e83a74b0" />

### outputs.tf

Outputs:

* Web Server Public IP
<img width="1932" height="151" alt="image" src="https://github.com/user-attachments/assets/fefbb9a7-7b8f-432b-9538-ea14fe1b728c" />

---

# Terraform Deployment

Initialize Terraform:

terraform init
<img width="1862" height="674" alt="image" src="https://github.com/user-attachments/assets/16feaea8-aeac-470e-af32-623d74d6c735" />

Validate Configuration:

terraform validate
<img width="1922" height="119" alt="image" src="https://github.com/user-attachments/assets/ed29d429-bbea-47a0-bb19-06d19e66dad3" />

Generate Execution Plan:

terraform plan
<img width="967" height="330" alt="Screenshot 2026-06-06 at 4 50 11 PM" src="https://github.com/user-attachments/assets/acc9c0af-41e9-4dd1-87b0-3d981c282fc6" />
<img width="1852" height="988" alt="image" src="https://github.com/user-attachments/assets/b17d43a6-119f-4248-885b-87ab31c93e63" />

Deploy Infrastructure:

terraform apply
<img width="974" height="636" alt="Screenshot 2026-06-06 at 5 06 44 PM" src="https://github.com/user-attachments/assets/395cfa02-027a-4acf-9f4c-b595e67208f2" />
<img width="971" height="655" alt="Screenshot 2026-06-06 at 5 07 06 PM" src="https://github.com/user-attachments/assets/8aca7667-6ea5-4078-843d-066d56d2fda0" />

---

# Part 2: Configuration Management using Ansible

## Inventory Configuration

Created inventory.ini containing:

[web]
<Web_Server_Public_IP>

[db]
<Database_Server_Private_IP>
<img width="932" height="311" alt="Screenshot 2026-06-06 at 10 43 03 PM" src="https://github.com/user-attachments/assets/b8f79899-c2cb-4bd1-922b-faa43aad5201" />

---

## Web Server Configuration

Ansible Playbook:

web.yml

Tasks:

* Install Node.js
* Install npm
* Install Git
* Clone TravelMemory Repository
* Install Dependencies
* Configure Environment Variables
* Start Backend Service
<img width="1846" height="1454" alt="image" src="https://github.com/user-attachments/assets/e14ec550-c7d6-4e88-a839-35fc50ea47d3" />

---

## Database Server Configuration

Ansible Playbook:

mongodb.yml

Tasks:

* Install MongoDB
* Enable MongoDB Service
* Secure MongoDB Configuration
* Create Database User
* Configure Bind IP
<img width="1862" height="1356" alt="image" src="https://github.com/user-attachments/assets/e88cbde1-c67d-4ca2-80a6-65a26bd3d121" />

---


# Security Implementation

## Network Security

### Web Server

Allowed:

* SSH (22) from Personal IP
* HTTP (80)
* HTTPS (443)
<img width="967" height="449" alt="Screenshot 2026-06-06 at 6 42 51 PM" src="https://github.com/user-attachments/assets/9db9a7b8-7925-4133-9fc3-a3024e553654" />
<img width="1229" height="699" alt="Screenshot 2026-06-06 at 10 51 49 PM" src="https://github.com/user-attachments/assets/68ea22a9-5210-46a3-accc-717ff63e484f" />

### Database Server

Allowed:

* MongoDB (27017) from Web Security Group
* SSH (22) from Web Security Group

Blocked:

* Direct Internet Access
<img width="1858" height="984" alt="image" src="https://github.com/user-attachments/assets/555338b0-e94c-4b3b-b65e-c41e56cbd007" />

---

## IAM Security

Implemented IAM Roles instead of hardcoded credentials.

Permissions granted only as required by application services.
<img width="970" height="87" alt="Screenshot 2026-06-06 at 4 57 10 PM" src="https://github.com/user-attachments/assets/593818ad-8f21-4e78-8d64-67b638080839" />

---

# Outputs

Terraform Output Example:

web_public_ip = xx.xx.xx.xx

Application URL:

http://<web_public_ip>
<img width="1289" height="666" alt="image" src="https://github.com/user-attachments/assets/fc29d067-bcc1-47bb-8db2-df7441a2516d" />
<img width="1475" height="746" alt="image" src="https://github.com/user-attachments/assets/d23d9624-3e06-4005-bdb5-bf7712dc31e8" />

---

# Screenshots

Include screenshots for:
<img width="1231" height="696" alt="Screenshot 2026-06-06 at 9 20 21 PM" src="https://github.com/user-attachments/assets/fba7246d-a685-490c-a190-08ca8982a2d7" />
<img width="973" height="541" alt="Screenshot 2026-06-06 at 6 45 51 PM" src="https://github.com/user-attachments/assets/f247cfe1-2a81-4bf1-867d-624ce22314f5" />
<img width="969" height="621" alt="Screenshot 2026-06-06 at 7 57 22 PM" src="https://github.com/user-attachments/assets/4af275e4-43f1-4d7c-bfbb-44db285d62f1" />
<img width="972" height="675" alt="Screenshot 2026-06-06 at 8 04 59 PM" src="https://github.com/user-attachments/assets/95018784-c4b9-4d9b-b9ad-e0f3516eba6b" />
<img width="972" height="592" alt="Screenshot 2026-06-06 at 8 07 04 PM" src="https://github.com/user-attachments/assets/2683fcd8-8e17-4f66-9883-11f04445c953" />
<img width="970" height="791" alt="Screenshot 2026-06-06 at 8 08 39 PM" src="https://github.com/user-attachments/assets/7ded295b-8ac9-4b13-bf5b-0d21a116ae6e" />

---

# Challenges Faced

* Configuring NAT Gateway routing.
* Establishing SSH communication through Ansible.
* Securing MongoDB in private subnet.
* Managing Security Group dependencies.
* Troubleshooting package installation issues.

---

# Learning Outcomes

Through this project I learned:

* Infrastructure as Code using Terraform.
* AWS Networking Fundamentals.
* EC2 Provisioning and Security.
* Configuration Management with Ansible.
* MERN Application Deployment.
* Security Best Practices in Cloud Infrastructure.
* Automation of Infrastructure and Application Deployment.

---

# Conclusion

This project successfully demonstrates automated deployment of a MERN stack application on AWS using Terraform for infrastructure provisioning and Ansible for server configuration. The solution follows cloud security best practices while maintaining scalability and automation.
