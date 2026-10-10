# 🚀 Day 16 — AWS Core Concepts

## 📌 Overview

Day 16 marks the beginning of Phase 3 of my 30-Day Cloud & DevOps Learning Journey.

After completing Linux Administration and Networking Fundamentals, I started learning Amazon Web Services (AWS).

Today, I explored the fundamentals of cloud computing, AWS global infrastructure, core services, security responsibilities, and pricing.

## 🎯 Learning Objectives

- Understand cloud computing and AWS.
- Learn AWS Regions, Availability Zones, and Edge Locations.
- Understand EC2, S3, EBS, IAM, and VPC.
- Learn the AWS Shared Responsibility Model.
- Understand AWS pricing and cost management.
- Explore the AWS Management Console.
- Learn basic AWS CLI commands.

---

## ☁️ 1. What Is Cloud Computing?

Cloud computing provides on-demand access to computing resources over a network.

These resources include:

- Virtual servers
- Storage
- Databases
- Networking
- Security services
- Monitoring tools

### Traditional IT vs Cloud Computing

| Traditional IT | Cloud Computing |
|---|---|
| Requires physical infrastructure purchases | Resources can be provisioned on demand |
| Hardware maintenance is managed locally | Cloud provider manages the underlying infrastructure |
| Capacity expansion can take time | Capacity can often be provisioned quickly |
| Physical infrastructure requires planning | Resources can be adjusted according to requirements |

---

## 🌐 2. AWS Global Infrastructure

AWS infrastructure includes Regions, Availability Zones, and Edge Locations.

### AWS Region

A Region is a separate geographic area containing AWS infrastructure.

Examples:

- Mumbai — `ap-south-1`
- Singapore — `ap-southeast-1`
- N. Virginia — `us-east-1`

Region selection depends on latency, service availability, compliance, data residency, resilience, and cost.

### Availability Zone

An Availability Zone is an isolated location within an AWS Region.

Deploying an application across multiple Availability Zones can improve resilience when combined with appropriate redundancy and failover mechanisms.

### Edge Location

Edge Locations support services such as Amazon CloudFront, which delivers content closer to users.

### Comparison

| Component | Purpose |
|---|---|
| Region | Geographic AWS infrastructure location |
| Availability Zone | Isolation and resilience within a Region |
| Edge Location | Content delivery closer to users |

---

## 🖥️ 3. Amazon EC2

EC2 stands for Elastic Compute Cloud.

It provides virtual servers called instances.

### Common use cases

- Hosting websites
- Running Linux servers
- Deploying applications
- Installing Apache or Nginx
- Running Docker containers
- Performing server administration
- Testing scripts and automation

### Important concepts

- AMI
- Instance type
- Instance state
- Key pair
- Security Group
- EBS volume
- Network interface
- Public and private IP addresses

---

## 🪣 4. Amazon S3

S3 stands for Simple Storage Service.

It is an object storage service.

### Common use cases

- Backups
- Log files
- Images and videos
- Static website assets
- Application files
- Data archives

### S3 structure

```text
Amazon S3
    |
    └── Bucket
          |
          ├── image.png
          ├── backup.zip
          ├── application.log
          └── report.pdf
```

An S3 bucket stores objects. Each object has a key, data, and associated metadata.

---

## 💾 5. Amazon EBS

EBS stands for Elastic Block Store.

It provides block storage commonly used with EC2 instances.

### Comparison: S3 vs EBS

| Feature | S3 | EBS |
|---|---|---|
| Storage type | Object storage | Block storage |
| Main purpose | Store objects and files | Provide block volumes |
| Common use | Backups, media, logs | Operating systems and application data |
| Access model | S3 API and supported interfaces | Attached and mounted as a block device |

---

## 🔐 6. AWS IAM

IAM stands for Identity and Access Management.

It helps manage access to AWS resources.

### Important components

| Component | Description |
|---|---|
| User | Identity that can represent a person or workload |
| Group | Collection of IAM users |
| Role | Identity that can be assumed to obtain temporary permissions |
| Policy | Defines allowed or denied actions |
| MFA | Adds another authentication factor |

### Principle of Least Privilege

Grant only the permissions required to perform a task.

For example, an application that only needs to read objects from a specific S3 bucket should not receive unrestricted access to every AWS service.

---

## 🌐 7. Amazon VPC

VPC stands for Virtual Private Cloud.

It provides a logically isolated virtual network for AWS resources.

### Important VPC components

- CIDR block
- Subnets
- Route tables
- Internet Gateway
- NAT Gateway
- Security Groups
- Network ACLs
- Network interfaces

### Example architecture

```text
AWS VPC: 10.0.0.0/16
          |
          ├── Public Subnet
          |       |
          |       └── Web Server
          |
          └── Private Subnet
                  |
                  └── Application Server
```

Subnet routing and associated network configuration determine whether resources have public or private connectivity.

---

## 🛡️ 8. AWS Shared Responsibility Model

AWS and its customers share responsibility for security.

### AWS responsibility — Security OF the cloud

AWS manages the underlying cloud infrastructure, including physical data centers, physical servers, and physical networking.

### Customer responsibility — Security IN the cloud

Customer responsibilities depend on the AWS services used and may include:

- IAM permissions
- Data protection
- Network configuration
- Application security
- Security Group rules
- Operating system patching on EC2
- Encryption settings

### Example

For an EC2 instance, the customer is generally responsible for maintaining and patching the guest operating system.

With managed services, AWS handles more of the underlying service maintenance, but customers still have configuration, data, and access-control responsibilities.

---

## 💰 9. AWS Pricing and Cost Management

Cloud resources may incur charges based on usage and the applicable pricing model.

### Important practices

- Review pricing before creating resources.
- Use appropriately sized resources.
- Configure billing alerts and budgets where available.
- Stop unused EC2 instances.
- Delete unneeded resources.
- Check for billable EBS volumes, snapshots, public IPv4 addresses, and NAT Gateways.
- Review usage through AWS Billing and Cost Management.

Stopping an EC2 instance does not necessarily stop all related charges.

Budgets and alerts help monitor spending but do not necessarily prevent additional charges.

---

## ⌨️ 10. AWS CLI Fundamentals

The AWS CLI allows users to interact with AWS services from a terminal.

### Check AWS CLI version

```bash
aws --version
```

### Inspect CLI configuration

```bash
aws configure list
```

### Check the active AWS identity

```bash
aws sts get-caller-identity
```

The identity command requires valid credentials and permission to call AWS STS.

### Security precautions

- Never commit AWS credentials to GitHub.
- Never share secret access keys in screenshots.
- Avoid using root credentials for routine CLI tasks.
- Prefer temporary credentials and IAM roles where appropriate.
- Protect local credential files.

---

## 🧪 11. Hands-on Practice

I explored the AWS Management Console and reviewed the following services.

### Task 1 — AWS Regions

- Identified the Region selector.
- Explored available Regions.
- Learned how Region selection affects regional resources.

### Task 2 — EC2

- Reviewed the EC2 dashboard.
- Explored instance concepts.
- Learned about AMIs, instance types, key pairs, and Security Groups.

### Task 3 — S3

- Reviewed the S3 console.
- Learned about buckets and objects.
- Explored storage concepts.

### Task 4 — IAM

- Reviewed IAM users, groups, roles, and policies.
- Studied the principle of least privilege.

### Task 5 — VPC

- Reviewed VPCs, subnets, route tables, and gateways.
- Connected VPC concepts with previously learned networking fundamentals.

### Task 6 — Cost Management

- Reviewed AWS billing and cost-management concepts.
- Learned how to reduce the risk of unexpected charges.

Note: This introductory lab focuses on console exploration and does not require launching billable resources.

---

## 🎯 12. Interview Questions

### Q1. What is AWS?

AWS is a cloud computing platform that provides computing, storage, networking, database, security, and monitoring services.

### Q2. What is an AWS Region?

A Region is a separate geographic area containing AWS infrastructure.

### Q3. What is an Availability Zone?

An Availability Zone is an isolated location within a Region designed to support resilient architectures.

### Q4. What is EC2?

EC2 provides virtual servers called instances.

### Q5. What is S3?

S3 is an object storage service.

### Q6. What is EBS?

EBS provides block storage commonly used with EC2 instances.

### Q7. What is IAM?

IAM manages identities and permissions for accessing AWS resources.

### Q8. What is VPC?

VPC provides a logically isolated virtual network in AWS.

### Q9. Explain the Shared Responsibility Model.

AWS manages the underlying cloud infrastructure, while customers manage security responsibilities within the cloud according to the services they use and their configurations.

### Q10. How can you reduce unexpected AWS charges?

Review pricing, monitor usage, configure budgets and alerts, and stop or delete unneeded resources while checking for related resources that may continue to incur charges.

---

## 📸 13. Screenshots

Recommended screenshot structure:

```text
screenshots/
├── 01-aws-region-selector.png
├── 02-ec2-dashboard.png
├── 03-s3-dashboard.png
├── 04-iam-dashboard.png
└── 05-vpc-dashboard.png
```

Screenshots should demonstrate actual learning activities.

Before uploading them publicly, remove or hide account IDs, email addresses, credentials, and other sensitive information.

---

## 💡 14. Key Takeaways

- Cloud computing provides on-demand access to computing resources.
- AWS offers compute, storage, networking, security, and monitoring services.
- Regions and Availability Zones are important for application deployment and resilience.
- EC2 provides virtual servers.
- S3 provides object storage.
- EBS provides block storage.
- IAM manages identities and permissions.
- VPC provides logically isolated virtual networking.
- AWS and customers share security responsibilities.
- Cost monitoring and credential protection are essential cloud practices.

---

## 🚀 Day 16 Completed

Today I learned AWS Core Concepts and explored the AWS Management Console.

This knowledge provides the foundation for learning IAM, EC2, VPC, AWS networking, cloud security, and monitoring in the upcoming days.

Next: Day 17 — AWS IAM: Users, Groups, Roles, Policies, and Least Privilege.
