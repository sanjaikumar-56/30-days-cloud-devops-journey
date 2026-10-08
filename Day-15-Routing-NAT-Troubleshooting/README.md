# Day 15 — Routing, NAT & Network Troubleshooting

## 📌 Overview

Today I learned how packets are routed between networks, how NAT translates IP addresses, and how to systematically troubleshoot network connectivity problems.

These concepts are important for:

- Linux Administration
- Network Engineering
- Cloud Engineering
- AWS VPC
- DevOps
- Server Troubleshooting

---

# 🛣️ 1. What is Routing?

Routing is the process of determining where a network packet should be forwarded based on its destination IP address.

A simplified flow:

```text
Source
  ↓
Routing Table
  ↓
Next Hop / Gateway
  ↓
Network Interface
  ↓
Destination
```

Linux uses a routing table to determine the best route for a packet.

---

# 📋 2. Linux Routing Table

View the routing table:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.20
```

### Default route

```text
default via 192.168.1.1 dev eth0
```

This means traffic for destinations without a more specific route is sent through:

```text
Gateway: 192.168.1.1
Interface: eth0
```

### Connected route

```text
192.168.1.0/24 dev eth0
```

This indicates that the network is directly connected through `eth0`.

---

# 🎯 3. Default Gateway

A default gateway is the router used when the destination does not match a more specific route.

Example:

```text
Linux Server
192.168.1.20
      |
      ↓
Gateway
192.168.1.1
      |
      ↓
Internet
```

Check the default gateway:

```bash
ip route
```

or:

```bash
ip route | grep default
```

---

# 🔎 4. Route Selection

Linux can have multiple routes that match a destination.

For example:

```text
10.0.0.0/8
10.10.0.0/16
10.10.20.0/24
default
```

For destination:

```text
10.10.20.50
```

The most specific matching route is:

```text
10.10.20.0/24
```

This is called:

## Longest Prefix Match

In general:

```text
More specific route
        ↓
Less specific route
        ↓
Default route
```

---

# 🧰 5. Important Routing Commands

### Display routes

```bash
ip route
```

### Display IPv4 routes

```bash
ip -4 route
```

### Check the route to a destination

```bash
ip route get 8.8.8.8
```

This can show:

- Destination
- Gateway
- Network interface
- Source IP

### Check default route

```bash
ip route | grep default
```

---

# 🔄 6. What is NAT?

NAT stands for:

Network Address Translation

NAT modifies IP addressing information as packets pass through a router, firewall or NAT device.

One common use is allowing private IP addresses to access external networks using a public IP address.

Example:

```text
Private Network

192.168.1.20
192.168.1.21
192.168.1.22
       |
       ↓
      NAT
       |
       ↓
Public IP
       |
       ↓
Internet
```

---

# 🔐 7. Private IP Address Ranges

The commonly used private IPv4 ranges are:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

Private addresses are intended for internal networks and are not directly routable across the public internet.

NAT is commonly used to provide outbound connectivity from private networks.

---

# 🔄 8. SNAT

SNAT means:

Source Network Address Translation

The source IP address is translated.

Example:

```text
Before NAT:

Source: 192.168.1.20
Destination: 8.8.8.8

        ↓

After NAT:

Source: Public IP
Destination: 8.8.8.8
```

The external destination sees the NAT device's public address.

---

# 🔄 9. DNAT

DNAT means:

Destination Network Address Translation

The destination IP address is translated.

Example:

```text
Internet Client
      |
      ↓
Public IP:80
      |
      ↓
DNAT
      |
      ↓
Private Server:80
```

DNAT can be used to forward traffic from a public address or port to an internal server.

---

# ⚖️ 10. Routing vs NAT

| Routing | NAT |
|---|---|
| Determines where a packet should go | Translates IP addresses |
| Uses routing information | Modifies source/destination addressing |
| Selects next hop/interface | Performs address translation |
| Does not inherently require address translation | Often implemented on routers/firewalls |

Important:

Routing and NAT are related, but they are not the same thing.

---

# ☁️ 11. NAT in AWS

A common AWS architecture is:

```text
                  Internet
                     |
                     ↓
              Internet Gateway
                     |
                Public Subnet
                     |
                NAT Gateway
                     |
                Private Subnet
                     |
                    EC2
```

A private EC2 instance can use a NAT Gateway for outbound internet connectivity without requiring a public IPv4 address.

### Important AWS components

- VPC
- Public subnet
- Private subnet
- Route table
- NAT Gateway
- Internet Gateway
- EC2

---

# 🌐 12. Internet Gateway vs NAT Gateway

### Internet Gateway

Provides connectivity between a VPC and the internet for resources whose networking configuration permits that connectivity.

### NAT Gateway

Allows resources in a private subnet to initiate outbound connections to external networks.

A NAT Gateway does not make private instances directly reachable from the internet.

---

# 🧪 13. Hands-on Lab

## Step 1 — Check interfaces

```bash
ip link
```

## Step 2 — Check IPv4 addresses

```bash
ip -4 addr
```

## Step 3 — Check routing table

```bash
ip route
```

## Step 4 — Check a specific route

```bash
ip route get 8.8.8.8
```

## Step 5 — Check default gateway

```bash
ip route | grep default
```

## Step 6 — Check neighbor information

```bash
ip neigh
```

## Step 7 — Test localhost

```bash
ping -c 4 127.0.0.1
```

## Step 8 — Test the gateway

Replace the address with your actual gateway:

```bash
ping -c 4 <gateway-ip>
```

## Step 9 — Test external IP connectivity

```bash
ping -c 4 8.8.8.8
```

## Step 10 — Test DNS

```bash
ping -c 4 google.com
```

## Step 11 — Test HTTPS

```bash
curl -I https://google.com
```

## Step 12 — Check listening ports

```bash
ss -lntup
```

## Step 13 — Test a TCP port

```bash
nc -vz <server-ip> 80
```

---

# 🛠️ 14. Network Troubleshooting Method

When a server has a connectivity problem, troubleshoot from the lower layers toward the application.

```text
Network Problem
      ↓
Check Interface
      ↓
Check IP Address
      ↓
Check Routing Table
      ↓
Check Default Gateway
      ↓
Test Gateway
      ↓
Test IP Connectivity
      ↓
Check DNS
      ↓
Check Port
      ↓
Check Firewall
      ↓
Check Service
      ↓
Verify Application
```

---

# 🚨 15. Scenario — No Internet Access

### Symptom

The server cannot access the internet.

### Step 1 — Interface

```bash
ip link
```

Check whether the interface is UP.

### Step 2 — IP address

```bash
ip -4 addr
```

Check whether the interface has an IP address.

### Step 3 — Routing

```bash
ip route
```

Look for:

```text
default via ...
```

### Step 4 — Gateway

```bash
ping -c 4 <gateway-ip>
```

### Step 5 — External IP

```bash
ping -c 4 8.8.8.8
```

### Step 6 — DNS

```bash
ping -c 4 google.com
```

This allows us to identify whether the problem is related to:

- Interface
- IP configuration
- Routing
- Gateway
- Internet connectivity
- DNS

---

# 🚨 16. Scenario — Gateway Works but Internet Does Not

Suppose:

```text
Gateway       ✓
8.8.8.8       ✗
```

Possible areas to investigate:

- Routing
- Firewall
- NAT
- Network ACL
- Upstream router
- Internet connectivity

The important point is that the local network is reachable, but connectivity beyond the gateway is failing.

---

# 🚨 17. Scenario — IP Works but DNS Fails

Suppose:

```bash
ping -c 4 8.8.8.8
```

works.

But:

```bash
ping -c 4 google.com
```

fails.

This strongly suggests investigating DNS.

Commands:

```bash
cat /etc/resolv.conf
getent hosts google.com
dig google.com
```

This connects directly with Day 14.

---

# 🚨 18. Scenario — DNS Works but Application Fails

Suppose hostname resolution works:

```bash
getent hosts example.com
```

But the application cannot connect.

Next check:

```bash
ss -lntp
```

Then:

```bash
nc -vz <server-ip> <port>
```

And:

```bash
curl -v http://example.com
```

Troubleshooting flow:

```text
DNS
 ↓
Port
 ↓
Firewall
 ↓
Service
 ↓
Application
```

---

# 🔍 19. Useful Troubleshooting Commands

### Network interface

```bash
ip link
```

### IP address

```bash
ip addr
```

### Routing

```bash
ip route
```

### Specific route

```bash
ip route get 8.8.8.8
```

### Neighbor table

```bash
ip neigh
```

### Connectivity

```bash
ping -c 4 8.8.8.8
```

### DNS

```bash
dig google.com
```

### Listening sockets

```bash
ss -lntup
```

### Port testing

```bash
nc -vz <server-ip> <port>
```

### HTTP debugging

```bash
curl -v https://google.com
```

### Interface statistics

```bash
ip -s link
```

---

# 🎯 20. Interview Questions

## Q1. What is routing?

Routing is the process of determining the path or next hop for forwarding packets toward a destination network.

## Q2. What is a routing table?

A routing table contains destination networks and the information required to determine where packets should be forwarded.

## Q3. What is a default gateway?

A default gateway is the next-hop router used when there is no more specific route for a destination.

## Q4. What is NAT?

NAT translates IP addresses as traffic passes through a NAT device.

## Q5. What is SNAT?

SNAT changes the source IP address of a packet.

## Q6. What is DNAT?

DNAT changes the destination IP address of a packet.

## Q7. What is the difference between routing and NAT?

Routing determines where a packet should be forwarded, while NAT translates source or destination addressing.

## Q8. How do you troubleshoot a server with no internet?

I would first check the interface and IP address using `ip link` and `ip addr`. Then I would check the routing table and default gateway using `ip route`. Next, I would test the gateway and an external IP such as 8.8.8.8. If IP connectivity works but hostname resolution fails, I would troubleshoot DNS. Finally, I would check ports, firewalls, services and application logs.

## Q9. What is an AWS NAT Gateway?

An AWS NAT Gateway provides outbound connectivity from resources in private subnets to external networks without requiring those private resources to have public IPv4 addresses.

---

# 📸 21. Screenshots

Recommended screenshots:

```text
screenshots/
├── 01-network-interfaces.png
├── 02-routing-table.png
├── 03-route-get.png
├── 04-gateway-connectivity.png
└── 05-network-troubleshooting.png
```

For LinkedIn, focus on screenshots that demonstrate actual hands-on work rather than only theory.

---

# 🧠 22. Key Takeaways

- Routing determines where packets should go.
- Linux stores routing information in a routing table.
- `ip route` is the primary command for inspecting routes.
- `ip route get` helps determine the route to a specific destination.
- A default gateway handles destinations without a more specific route.
- Longest Prefix Match determines the most specific matching route.
- NAT translates IP addresses.
- SNAT translates source addresses.
- DNAT translates destination addresses.
- AWS NAT Gateway provides outbound connectivity for private subnets.
- Network troubleshooting should be systematic.
- Always identify the failing layer before changing configurations.

---

# 🚀 Day 15 Completed

Today I strengthened my understanding of routing, NAT, AWS NAT Gateway and systematic network troubleshooting.

These concepts provide an important foundation for the upcoming AWS networking topics in this 30-day Cloud & DevOps journey.
