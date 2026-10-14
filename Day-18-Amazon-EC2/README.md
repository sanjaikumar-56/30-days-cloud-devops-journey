🚀 Day 18 — Amazon EC2

📌 Overview

Day 18 of my 30-Day Cloud & DevOps Learning Journey focuses on Amazon EC2 (Elastic Compute Cloud).

EC2 is an AWS compute service that provides virtual servers called instances.

Today, I studied EC2 fundamentals, AMIs, instance types, key pairs, Security Groups, EBS storage, instance lifecycle, and connectivity troubleshooting.

🎯 Learning Objectives

- Understand Amazon EC2.
- Learn about AMIs and instance types.
- Understand key pairs and SSH authentication.
- Explore Security Groups and network access.
- Differentiate public and private IP addresses.
- Understand EBS storage.
- Learn EC2 instance states.
- Troubleshoot SSH and application connectivity.
- Understand EC2 cost management.

---

🖥️ 1. What Is Amazon EC2?

Amazon EC2 stands for Elastic Compute Cloud.

It provides virtual servers that can run operating systems, applications, and services.

Common use cases

- Hosting websites.
- Running Linux servers.
- Deploying applications.
- Installing Apache and Nginx.
- Running Docker containers.
- Performing server administration.
- Testing scripts and automation.

EC2 architecture

AWS Account
    |
    ▼
VPC
    |
    ▼
Subnet
    |
    ▼
EC2 Instance
    |
    ├── Operating System
    ├── CPU and Memory
    ├── EBS Storage
    ├── Network Interface
    └── Security Group

---

💿 2. Amazon Machine Image (AMI)

An AMI is an image used to launch EC2 instances.

It provides the operating system and associated image configuration required to initialize an instance.

Examples

- Amazon Linux.
- Ubuntu Server.
- Red Hat Enterprise Linux.
- Windows Server.

AMI vs EC2

AMI| EC2 Instance
Image used to launch an instance| Virtual server created from an image
Defines the base operating system image and associated configuration| Runs applications and workloads
Can be used to launch multiple instances| Has its own instance state and configuration

---

⚙️ 3. EC2 Instance Types

An instance type determines the compute resources available to an instance.

These include virtual CPUs, memory, networking capabilities, and other characteristics.

Instance Family| Typical Use
General purpose| Balanced workloads
Compute optimized| CPU-intensive workloads
Memory optimized| Memory-intensive workloads
Storage optimized| High storage throughput or I/O
Accelerated computing| GPU or specialized hardware workloads

The instance type should match the application's performance and cost requirements.

---

🔑 4. EC2 Key Pairs and SSH

Key pairs are commonly used for SSH authentication to Linux EC2 instances.

A key pair contains:

- Public key.
- Private key.

The public key is configured for the instance, while the private key is kept securely by the authorized user.

SSH connection example

For Ubuntu:

ssh -i my-key.pem ubuntu@EC2_PUBLIC_IP

For many Amazon Linux images:

ssh -i my-key.pem ec2-user@EC2_PUBLIC_IP

Replace the placeholders with your actual key path and instance endpoint.

Private key permissions

On Linux:

chmod 400 my-key.pem

SSH troubleshooting

Enable verbose SSH output:

ssh -vvv -i my-key.pem ubuntu@EC2_PUBLIC_IP

Security precautions

- Never upload private keys to GitHub.
- Never share secret credentials.
- Verify the correct username for the selected AMI.
- Use the appropriate private key.
- Restrict SSH access to trusted sources.

---

🛡️ 5. Security Groups

A Security Group is a stateful virtual firewall associated with an EC2 network interface.

It controls inbound and outbound traffic.

Common ports

Port| Protocol| Purpose
22| TCP| SSH
80| TCP| HTTP
443| TCP| HTTPS

Example

Security Group
     |
     ├── TCP 22  → SSH
     ├── TCP 80  → HTTP
     └── TCP 443 → HTTPS

Only permit the traffic required by your application.

For SSH, prefer a rule restricted to your trusted public IP address or an approved administrative access path.

Important concepts

- Inbound rules.
- Outbound rules.
- Allowed source addresses.
- Destination ports.
- Stateful connection tracking.

Security Groups do not replace route tables, Network ACLs, or operating-system firewalls.

---

🌐 6. Public IP vs Private IP

An EC2 instance may have a private IP address and, depending on its configuration, a public IP address.

Private IP

Used for communication within private networks and connected networks.

Example:

10.0.1.25

Public IP

Provides a publicly routable endpoint when the networking configuration permits internet connectivity.

Example architecture

Internet
   |
   ▼
Internet Gateway
   |
   ▼
Public Subnet
   |
   ▼
EC2 Instance
   |
   ├── Public IP
   └── Private IP

A public IP alone does not guarantee internet connectivity. Routing, Security Groups, Network ACLs, and other configurations must also be correct.

---

💾 7. Amazon EBS

EBS stands for Elastic Block Store.

It provides block storage for EC2 instances.

Common use cases

- Operating system storage.
- Application data.
- Logs.
- Databases.
- Persistent file storage.

EC2 vs EBS

EC2| EBS
Provides compute capacity| Provides block storage
Runs the operating system and applications| Stores data on attached volumes
Has an instance lifecycle| Has a separate volume lifecycle and configuration

EBS volumes can persist independently of an instance, depending on their configuration.

---

🔄 8. EC2 Instance Lifecycle

Common EC2 states include:

- Pending.
- Running.
- Stopping.
- Stopped.
- Shutting-down.
- Terminated.

Lifecycle overview

Launch
  |
  ▼
Pending
  |
  ▼
Running
  |
  ├── Stop ──► Stopped
  |               |
  |               └── Start ──► Running
  |
  └── Terminate ──► Terminated

Stop vs Terminate

Stop| Terminate
Shuts down the instance| Permanently deletes the instance
Instance can generally be started again| Instance cannot be restarted
EBS data usually persists, subject to configuration| EBS volumes may be deleted depending on settings
Compute charges stop, but other charges may remain| Other associated resources may continue to incur charges

Always verify resource settings before stopping or terminating an instance.

---

🧪 9. Hands-on Practice

Task 1: Explore EC2

- Open the AWS Management Console.
- Navigate to EC2.
- Review the instance dashboard.
- Explore instance types and AMIs.

Task 2: Explore Security Groups

- Review inbound rules.
- Review outbound rules.
- Identify the purpose of SSH, HTTP, and HTTPS ports.

Task 3: Explore EBS

- Review available EBS volumes.
- Understand volume size, type, and attachment.
- Avoid deleting existing resources.

Task 4: Explore Instance Details

Review:

- Instance state.
- Instance type.
- AMI.
- VPC and subnet.
- Private IP.
- Public IP, if configured.
- Security Groups.
- Storage.

This console-exploration lab does not require launching a billable instance.

---

🧪 10. Optional Lab — Connect to a Linux EC2 Instance

Only perform this lab if you have an AWS account and have reviewed the applicable pricing.

Step 1: Launch a Linux instance

Select an appropriate Linux AMI and instance type.

Step 2: Configure a key pair

Create or select an appropriate key pair.

Keep the private key secure.

Step 3: Configure networking

Select the VPC and subnet.

Verify that the instance has an appropriate reachable endpoint and route.

Step 4: Configure the Security Group

Allow SSH from a trusted source.

Avoid opening SSH to everyone without a justified requirement.

Step 5: Connect

For Ubuntu:

ssh -i my-key.pem ubuntu@EC2_PUBLIC_IP

Step 6: Verify the server

whoami
hostname
uname -a
ip addr
df -h

Step 7: Clean up

If you no longer need the instance, terminate it and review related EBS volumes, snapshots, public IPv4 addresses, and other resources for possible ongoing charges.

Do not delete resources containing important data.

---

🚨 11. Troubleshooting Scenario — SSH Timeout

Symptom

SSH does not connect and eventually times out.

Possible causes

- Instance is not running.
- Incorrect public IP or endpoint.
- Missing route.
- Missing Internet Gateway configuration.
- Security Group blocks TCP port 22.
- Network ACL blocks traffic.
- Operating-system firewall blocks traffic.
- Network path problem.

Troubleshooting flow

SSH Timeout
     |
     ▼
Check Instance State
     |
     ▼
Verify IP / Endpoint
     |
     ▼
Check Route Table
     |
     ▼
Check Internet Gateway
     |
     ▼
Check Security Group
     |
     ▼
Check Network ACL
     |
     ▼
Check SSH Service
     |
     ▼
Retest

Test TCP connectivity where possible:

nc -vz EC2_PUBLIC_IP 22

A timeout is a clue, not proof of a single root cause.

---

🚨 12. Troubleshooting Scenario — Permission Denied (Publickey)

Symptom

Permission denied (publickey).

Checks

1. Verify the private key.
2. Verify the AMI's default username.
3. Check private key permissions.
4. Confirm the key corresponds to the instance configuration.
5. Review verbose SSH output.
6. Investigate server-side SSH configuration if necessary.

Diagnostic command

ssh -vvv -i my-key.pem ubuntu@EC2_PUBLIC_IP

This commonly indicates an authentication problem rather than a network timeout.

---

🚨 13. Troubleshooting Scenario — Website Not Opening

Step 1: Check instance state

Confirm the instance is running.

Step 2: Check web server service

sudo systemctl status nginx

Step 3: Check listening ports

sudo ss -lntp

Step 4: Test locally

curl -I http://localhost

Step 5: Check network access

Review Security Groups, routing, Network ACLs, and the operating-system firewall.

Troubleshooting flow

Website Not Opening
        |
        ▼
Check Instance
        |
        ▼
Check Web Service
        |
        ▼
Check Listening Port
        |
        ▼
Test Local Application
        |
        ▼
Check Network Rules
        |
        ▼
Test External Access

---

💰 14. EC2 Cost Management

Before launching instances:

- Review current pricing.
- Choose an appropriate instance type.
- Review storage settings.
- Monitor resource usage.
- Stop or terminate unused instances.
- Check EBS volumes and snapshots.
- Review public IPv4 address charges.
- Check for other associated billable resources.

Stopping an instance does not necessarily eliminate all related charges.

---

🎯 15. Interview Questions

Q1. What is Amazon EC2?

EC2 is an AWS compute service that provides virtual servers called instances.

Q2. What is an AMI?

An AMI is an image used to launch EC2 instances.

Q3. What is an instance type?

An instance type determines the compute resources available to an instance.

Q4. What is a Security Group?

A Security Group is a stateful virtual firewall that controls inbound and outbound network traffic.

Q5. What is a key pair?

A key pair contains public and private keys and is commonly used for SSH authentication to Linux instances.

Q6. What is EBS?

EBS is a block storage service used with EC2 instances.

Q7. What is the difference between stopping and terminating an instance?

Stopping shuts down an instance that can generally be started again. Terminating permanently deletes the instance.

Q8. How do you troubleshoot an SSH timeout?

I would check the instance state, IP address, routing, Internet Gateway configuration, Security Group, Network ACL, SSH service, and network path.

Q9. Why does SSH return Permission denied (publickey)?

Possible causes include an incorrect private key, incorrect username, invalid key permissions, or a mismatch between the private key and the instance's configured public key.

Q10. How would you troubleshoot a website hosted on EC2?

I would check the instance state, web server service, listening port, local application response, Security Group rules, routing, Network ACLs, and operating-system firewall.

---

📸 16. Screenshots

Recommended screenshots:

screenshots/
├── 01-ec2-dashboard.png
├── 02-instance-configuration.png
├── 03-security-group-rules.png
├── 04-ec2-instance-details.png
└── 05-ssh-troubleshooting.png

Remove sensitive account information and never upload private keys or credentials.

---

💡 17. Key Takeaways

- EC2 provides virtual servers.
- AMIs are used to launch instances.
- Instance types determine compute characteristics.
- Key pairs support SSH authentication.
- Security Groups control network traffic.
- Public IPs do not guarantee connectivity.
- EBS provides block storage.
- Stopping and terminating have different consequences.
- SSH timeouts and public-key errors require different troubleshooting approaches.
- Cost management is an essential part of cloud administration.

---

🚀 Day 18 Completed

Today I strengthened my understanding of Amazon EC2, AMIs, instance types, Security Groups, key pairs, EBS, and troubleshooting.

Next: Day 19 — AWS VPC: CIDR Blocks, Subnets, Route Tables, Internet Gateways, and VPC Architecture.
