Day 19: AWS VPC — CIDR Blocks, Subnets, Route Tables & Internet Gateways

Overview

Day 19 of my 30-Day Cloud & DevOps Learning Journey focuses on Amazon VPC (Virtual Private Cloud) and the networking components used to build secure and reliable AWS architectures.

I explored VPC CIDR blocks, subnet design, route tables, Internet Gateways, AWS CLI commands, and common connectivity troubleshooting scenarios.

Learning Objectives

- Understand Amazon VPC and its purpose.
- Understand IPv4 CIDR blocks and address ranges.
- Differentiate public and private subnets.
- Understand route tables and default routes.
- Explain the role of an Internet Gateway.
- Explore VPC resources through the AWS Management Console.
- Use AWS CLI commands to inspect networking resources.
- Troubleshoot common VPC connectivity problems.

---

1. What Is Amazon VPC?

Amazon VPC is a logically isolated virtual network in AWS.

It allows us to control networking for AWS resources, including:

- IP address ranges
- Subnets
- Route tables
- Internet Gateways
- Network Access Control Lists (NACLs)
- Security Groups
- Network connectivity

Example

A typical web application architecture may use:

- Public subnet: Web server or public-facing load balancer
- Private subnet: Application server or database
- Internet Gateway: Internet connectivity for appropriately configured public resources
- Route tables: Traffic routing
- Security Groups: Resource-level traffic filtering

---

2. CIDR Blocks

CIDR stands for Classless Inter-Domain Routing.

It represents an IP address range using an address and prefix length.

Examples

CIDR Block| Total IPv4 Addresses
"10.0.0.0/16"| 65,536
"10.0.1.0/24"| 256
"10.0.1.0/26"| 64
"10.0.1.0/28"| 16

Formula

Total IPv4 addresses:

"2^(32 - prefix length)"

For example, for "10.0.1.0/24":

"2^(32 - 24) = 256 addresses"

AWS reserves five IPv4 addresses in each IPv4 subnet. Therefore, a "/24" subnet has 251 IPv4 addresses available for resources.

Private IPv4 Address Ranges

Private Range| CIDR
"10.0.0.0" – "10.255.255.255"| "10.0.0.0/8"
"172.16.0.0" – "172.31.255.255"| "172.16.0.0/12"
"192.168.0.0" – "192.168.255.255"| "192.168.0.0/16"

These ranges are commonly used for internal networks.

---

3. VPC and Subnet Architecture

Example network design:

Component| CIDR or Configuration
VPC| "10.0.0.0/16"
Public subnet| "10.0.1.0/24"
Private subnet| "10.0.2.0/24"
Local route| "10.0.0.0/16 → local"
Public default route| "0.0.0.0/0 → Internet Gateway"

The example illustrates how smaller subnet ranges fit inside a VPC's larger address range.

Each subnet belongs to one Availability Zone.

For high availability, production architectures commonly distribute resources across multiple Availability Zones.

---

4. Public vs. Private Subnets

Public Subnet

A subnet is considered public when its associated route table has a route to an Internet Gateway for internet-bound traffic.

Example:

"0.0.0.0/0 → igw-xxxxxxxx"

For direct IPv4 internet connectivity, an EC2 instance generally also needs a public IPv4 address or Elastic IP and appropriate security configuration.

Private Subnet

A private subnet does not have a direct route to an Internet Gateway for internet-bound traffic.

Its route table may contain:

"10.0.0.0/16 → local"

A private subnet can also use a NAT Gateway or other network path for specific outbound connectivity requirements.

Comparison

Feature| Public Subnet| Private Subnet
Direct route to Internet Gateway| Typically present| Not present for internet-bound traffic
Direct inbound internet access| Possible when correctly configured| Not through a direct IGW route
Typical resources| Public web servers, load balancers| Databases, internal application servers
Internet access| Through IGW when required settings are configured| May use NAT or another controlled network path

Important: A subnet's route table is a key factor in determining whether it is public. A public IP address alone does not make a subnet public.

---

5. Route Tables

A route table determines where network traffic is directed.

Each route includes a destination and a target.

Example Routes

Destination| Target| Purpose
"10.0.0.0/16"| "local"| VPC-internal routing
"0.0.0.0/0"| Internet Gateway| Default IPv4 route toward the internet

Important Concepts

- Every VPC has a main route table.
- Each subnet is associated with a route table.
- A subnet without an explicit association uses the main route table.
- Routes are selected using longest-prefix matching.
- A route does not override Security Groups or network ACLs.

The destination "0.0.0.0/0" represents all IPv4 destinations and is commonly used as a default route.

---

6. Internet Gateway (IGW)

An Internet Gateway enables communication between a VPC and the internet when the required network configuration is in place.

Requirements for Direct IPv4 Internet Connectivity

Depending on the traffic direction and use case, the configuration generally requires:

1. An Internet Gateway attached to the VPC.
2. A route to the Internet Gateway in the subnet's route table.
3. Suitable public IPv4 addressing for the instance.
4. Security Group rules that permit the traffic.
5. Appropriate network ACL rules.
6. Correct operating-system firewall and application configuration.

An Internet Gateway does not automatically make every resource in a VPC publicly accessible.

---

7. AWS CLI Commands

These commands inspect existing AWS resources. They require AWS CLI configuration and appropriate IAM permissions.

Verify AWS CLI

aws --version

Check the Current AWS Identity

aws sts get-caller-identity

List VPCs

aws ec2 describe-vpcs

List Subnets

aws ec2 describe-subnets

List Route Tables

aws ec2 describe-route-tables

List Internet Gateways

aws ec2 describe-internet-gateways

Display VPC CIDR Information

aws ec2 describe-vpcs \
  --query "Vpcs[*].[VpcId,CidrBlock,IsDefault,State]" \
  --output table

Display Subnet Information

aws ec2 describe-subnets \
  --query "Subnets[*].[SubnetId,VpcId,CidrBlock,AvailabilityZone,AvailableIpAddressCount]" \
  --output table

Display Route Information

aws ec2 describe-route-tables \
  --query "RouteTables[*].[RouteTableId,VpcId,Routes[*].[DestinationCidrBlock,GatewayId]]" \
  --output json

Display Internet Gateway Information

aws ec2 describe-internet-gateways \
  --query "InternetGateways[*].[InternetGatewayId,Attachments]" \
  --output json

---

8. VPC Troubleshooting

Scenario 1: EC2 Cannot Access the Internet

Possible causes:

- Missing default route
- Internet Gateway not attached
- No public IPv4 address for direct IPv4 connectivity
- Incorrect Security Group rules
- Network ACL restrictions
- Operating-system or application problems

Troubleshooting sequence:

1. Check the EC2 instance state.
2. Identify the VPC and subnet.
3. Inspect the subnet's associated route table.
4. Verify the Internet Gateway attachment.
5. Check public IP addressing.
6. Review Security Group outbound rules.
7. Review network ACL rules and return traffic.
8. Check the operating system and application.

Scenario 2: SSH Connection Fails

Check:

- Instance state and public IP
- Route table and Internet Gateway
- Security Group inbound TCP port 22
- Network ACL rules
- SSH service status
- Correct private key and login username

Scenario 3: Instances in the Same VPC Cannot Communicate

Check:

- Correct VPC and IP addresses
- Local route
- Security Group rules
- Network ACL rules
- Operating-system firewalls
- Application listening ports

Scenario 4: Private Subnet Cannot Access the Internet

A private subnet may need a controlled outbound network path, such as a NAT Gateway.

A typical IPv4 design uses a NAT Gateway in a public subnet, with the private subnet's default route directed to that NAT Gateway.

This topic is covered in more detail in Day 20.

---

9. Hands-on Lab

Lab 1: Inspect a VPC

1. Open the AWS Management Console.
2. Navigate to VPC.
3. Open Your VPCs.
4. Select an existing VPC.
5. Record the CIDR block and state.
6. Check whether it is the default VPC.

Lab 2: Inspect Subnets

1. Open Subnets.
2. Select a subnet.
3. Record its CIDR block.
4. Identify its Availability Zone.
5. Inspect its associated route table.

Lab 3: Inspect Route Tables

1. Open Route tables.
2. Select the route table associated with your subnet.
3. Inspect the Routes tab.
4. Identify the local route.
5. Check for a default route.
6. Inspect subnet associations.

Lab 4: Inspect Internet Gateways

1. Open Internet gateways.
2. Identify an Internet Gateway attached to your VPC, if available.
3. Check its VPC attachment.
4. Verify whether the route table contains a route to it.

Safety: This lab uses existing resources for inspection. Do not modify or delete production networking resources.

---

10. Common Interview Questions

1. What is Amazon VPC?
2. What is CIDR notation?
3. What is the difference between a VPC and a subnet?
4. What makes a subnet public?
5. What is a private subnet?
6. What is a route table?
7. What does "0.0.0.0/0" mean?
8. What is an Internet Gateway?
9. Can an EC2 instance have a public IP but still be inaccessible?
10. How many IPv4 addresses does AWS reserve in each subnet?
11. What is the main route table?
12. What is the difference between a route table and a Security Group?
13. Why should VPC CIDR ranges be planned carefully?
14. How would you troubleshoot an EC2 internet connectivity issue?

---

11. Key Takeaways

- Amazon VPC provides a logically isolated virtual network in AWS.
- CIDR blocks define IPv4 address ranges.
- Subnets divide a VPC into smaller IP address ranges.
- Each subnet belongs to one Availability Zone.
- Public and private subnet classification depends on routing configuration.
- Route tables determine where network traffic goes.
- Internet Gateways enable internet connectivity when the required configuration is present.
- Security Groups, network ACLs, IP addressing, and operating-system configuration also affect connectivity.
- AWS reserves five IPv4 addresses in each IPv4 subnet.

---

12. Screenshots

Save your own screenshots in the "screenshots/" folder.

Suggested filenames:

screenshots/
├── 01-vpc-dashboard.png
├── 02-vpc-cidr-details.png
├── 03-subnet-details.png
├── 04-route-table-routes.png
└── 05-internet-gateway.png

Only add screenshots you have actually captured. If a resource is unavailable, document that instead of fabricating a screenshot.

Before publishing, review screenshots for sensitive account information and redact details you do not want to make public.

---

13. Learning Progress

- [x] Studied VPC fundamentals
- [x] Reviewed CIDR blocks
- [x] Compared public and private subnets
- [x] Studied route tables
- [x] Learned Internet Gateway fundamentals
- [x] Reviewed AWS CLI inspection commands
- [x] Practised networking troubleshooting scenarios
- [ ] Capture AWS Console screenshots
- [ ] Commit and push Day 19 to GitHub

Learning Journey

Day 19 of my 30-Day Cloud & DevOps Learning Journey.

Next: Day 20 — AWS Networking, NAT Gateway, and Public/Private Subnet Connectivity.
