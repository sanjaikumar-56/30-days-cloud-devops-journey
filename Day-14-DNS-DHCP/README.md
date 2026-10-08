
# Day 14 — DNS & DHCP

## 📌 Overview

Today I learned two fundamental networking services:

- DNS — Domain Name System
- DHCP — Dynamic Host Configuration Protocol

DNS translates domain names into IP addresses, while DHCP automatically provides network configuration such as IP address, subnet mask, default gateway and DNS server information.

These services are important for:

- Linux Administration
- Network Engineering
- Cloud Computing
- AWS
- Server Troubleshooting
- DevOps

---

# 🌐 1. DNS — Domain Name System

DNS is responsible for resolving human-readable domain names into IP addresses.

Example:

```text
google.com
     ↓
DNS Resolution
     ↓
142.x.x.x
```

Without DNS, users would need to remember IP addresses instead of domain names.

---

# 🔄 2. DNS Resolution Process

A simplified DNS resolution flow:

```text
Client
  ↓
Local DNS Configuration
  ↓
Recursive DNS Resolver
  ↓
Root DNS Server
  ↓
TLD DNS Server
  ↓
Authoritative DNS Server
  ↓
IP Address
  ↓
Client
```

In practice, caching often means the resolver can answer without contacting every level.

---

# 🧩 3. DNS Components

## Recursive DNS Resolver

A recursive resolver performs DNS lookups on behalf of the client.

Examples:

- ISP DNS resolvers
- Google Public DNS
- Cloudflare DNS

Example:

```text
8.8.8.8
```

## Authoritative DNS Server

An authoritative DNS server contains the actual DNS records for a domain.

Example records may include:

```text
example.com → IP address
mail.example.com → mail server
```

---

# 🌳 4. DNS Hierarchy

DNS uses a hierarchical structure:

```text
.
│
├── .com
│   └── example.com
│
├── .org
│   └── example.org
│
└── .in
    └── example.in
```

The hierarchy contains:

- Root
- TLD
- Authoritative domain servers

---

# 📋 5. Important DNS Records

| Record | Purpose |
|---|---|
| A | Maps hostname to IPv4 address |
| AAAA | Maps hostname to IPv6 address |
| CNAME | Alias for another hostname |
| MX | Mail server information |
| NS | Authoritative name servers |
| TXT | Text-based information such as verification records |

Example:

```text
example.com      A       192.0.2.10
www.example.com  CNAME   example.com
```

---

# ⏱️ 6. TTL

TTL means Time To Live.

It determines how long a DNS record can be cached before it needs to be refreshed.

Example:

```text
DNS Record
TTL = 300 seconds
```

This means the record may be cached for approximately 5 minutes.

---

# 🐧 7. Linux DNS Configuration

## /etc/hosts

The `/etc/hosts` file provides local hostname-to-IP mappings.

Check it:

```bash
cat /etc/hosts
```

Example:

```text
127.0.0.1 localhost
192.168.1.10 server01
```

---

## /etc/resolv.conf

This file contains DNS resolver configuration on many Linux systems.

Check it:

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 8.8.8.8
```

Note:

On modern Linux distributions, `/etc/resolv.conf` may be managed automatically by NetworkManager, systemd-resolved or another network-management service.

---

# 🔎 8. DNS Troubleshooting Commands

Check IP configuration:

```bash
ip -4 addr
```

Check routing:

```bash
ip route
```

Check hostname resolution:

```bash
getent hosts google.com
```

Use nslookup:

```bash
nslookup google.com
```

Use dig:

```bash
dig google.com
```

Short DNS result:

```bash
dig +short google.com
```

Query an A record:

```bash
dig A google.com
```

Query MX records:

```bash
dig MX google.com
```

Query NS records:

```bash
dig NS google.com
```

Query a specific DNS server:

```bash
dig @8.8.8.8 google.com
```

---

# 🚨 9. DNS Troubleshooting Scenario

### Problem

The server can access the internet using an IP address, but domain names are not resolving.

Example:

```bash
ping -c 4 8.8.8.8
```

Works.

But:

```bash
ping -c 4 google.com
```

Fails.

### Troubleshooting

```text
Application
     ↓
Check IP connectivity
     ↓
Check DNS resolution
     ↓
Check /etc/hosts
     ↓
Check /etc/resolv.conf
     ↓
Use getent
     ↓
Use nslookup / dig
     ↓
Test specific DNS server
     ↓
Verify DNS connectivity
```

Commands:

```bash
cat /etc/hosts
cat /etc/resolv.conf
getent hosts google.com
dig google.com
dig @8.8.8.8 google.com
```

### Diagnosis

If:

```bash
ping 8.8.8.8
```

works but:

```bash
ping google.com
```

fails, basic IP connectivity is working and DNS resolution should be investigated.

---

# 📡 10. DHCP — Dynamic Host Configuration Protocol

DHCP automatically provides network configuration to clients.

A DHCP server can provide:

- IP address
- Subnet mask
- Default gateway
- DNS server
- Lease information

---

# 🔄 11. DHCP DORA Process

DHCP commonly uses the DORA process:

```text
Client
  ↓
DHCP Discover
  ↓
DHCP Offer
  ↓
DHCP Request
  ↓
DHCP Acknowledge
  ↓
Client receives configuration
```

### D — Discover

Client searches for a DHCP server.

### O — Offer

DHCP server offers an IP configuration.

### R — Request

Client requests the offered configuration.

### A — Acknowledge

DHCP server confirms the lease.

---

# 🔢 12. DHCP Ports

DHCP uses UDP:

```text
DHCP Server → UDP 67
DHCP Client → UDP 68
```

---

# ⏳ 13. DHCP Lease

A DHCP IP address is normally assigned for a specific lease period.

Example:

```text
Client
   ↓
IP: 192.168.1.25
Lease: Temporary
```

The client may renew the lease before it expires.

---

# 🐧 14. Linux DHCP / Network Troubleshooting

Check network interfaces:

```bash
ip link
```

Check IPv4 address:

```bash
ip -4 addr
```

Check routing:

```bash
ip route
```

Check NetworkManager devices:

```bash
nmcli device status
```

Check NetworkManager connections:

```bash
nmcli connection show
```

Check boot/network logs:

```bash
journalctl -b
```

---

# 🔍 15. DNS vs DHCP

| Feature | DNS | DHCP |
|---|---|---|
| Full Name | Domain Name System | Dynamic Host Configuration Protocol |
| Main Purpose | Name resolution | Network configuration |
| Converts | Name → IP | Assigns IP configuration |
| Common Protocol | DNS | DHCP |
| Transport | Usually UDP/TCP 53 | UDP |
| Common Ports | 53 | 67/68 |
| Example | google.com → IP | Client receives 192.168.1.20 |

---

# ☁️ 16. AWS Connection

DNS and DHCP concepts are important when working with AWS networking.

Relevant AWS concepts include:

- VPC
- Subnets
- EC2
- Route 53
- Private IP addresses
- DNS resolution
- DHCP option sets
- Route tables

Example:

```text
AWS VPC
   ↓
Subnet
   ↓
EC2 Instance
   ↓
Private IP
   ↓
DNS Resolution
   ↓
Application
```

Understanding DNS helps troubleshoot problems where an EC2 instance has network connectivity but cannot resolve a hostname.

---

# 🧪 17. Hands-on Lab

## Step 1 — Check IP address

```bash
ip -4 addr
```

## Step 2 — Check default route

```bash
ip route
```

## Step 3 — Check local hostname mappings

```bash
cat /etc/hosts
```

## Step 4 — Check DNS configuration

```bash
cat /etc/resolv.conf
```

## Step 5 — Test DNS resolution

```bash
getent hosts google.com
```

## Step 6 — Query DNS

```bash
nslookup google.com
```

## Step 7 — Use dig

```bash
dig google.com
```

## Step 8 — Check specific records

```bash
dig A google.com
dig MX google.com
dig NS google.com
```

## Step 9 — Test a specific DNS server

```bash
dig @8.8.8.8 google.com
```

## Step 10 — Test IP vs DNS connectivity

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

---

# 🛠️ 18. General Network Troubleshooting Flow

```text
Network Problem
      ↓
Check Interface
      ↓
Check IP Address
      ↓
Check Default Route
      ↓
Test Gateway
      ↓
Test IP Connectivity
      ↓
Check DNS Resolution
      ↓
Check DNS Server
      ↓
Check Port
      ↓
Check Application
```

The important point is to troubleshoot layer by layer instead of randomly changing configurations.

---

# 🎯 19. Interview Questions

### Q1. What is DNS?

DNS is a hierarchical naming system that translates domain names into IP addresses and can also provide other information through DNS records.

### Q2. What is DHCP?

DHCP automatically provides network configuration such as IP address, subnet mask, default gateway and DNS server to clients.

### Q3. What is the DHCP DORA process?

DORA stands for:

```text
Discover
Offer
Request
Acknowledge
```

### Q4. What are common DNS records?

Common records include:

```text
A
AAAA
CNAME
MX
NS
TXT
```

### Q5. What is the difference between recursive and authoritative DNS?

A recursive resolver performs DNS lookups on behalf of clients, while an authoritative DNS server provides the authoritative DNS records for a domain.

### Q6. What are the DHCP ports?

```text
UDP 67 — Server
UDP 68 — Client
```

### Q7. A server can ping 8.8.8.8 but cannot ping google.com. What would you check?

I would suspect DNS resolution. I would check `/etc/hosts`, `/etc/resolv.conf`, test using `getent`, `nslookup` and `dig`, and then test a known DNS resolver such as `8.8.8.8`.

### Q8. What is TTL in DNS?

TTL specifies how long a DNS record can be cached before it should be refreshed.

---

# 💡 Key Takeaways

- DNS translates names into IP addresses.
- DHCP automatically provides network configuration.
- DNS commonly uses port 53.
- DHCP uses UDP 67 and 68.
- `/etc/hosts` provides local hostname mappings.
- `/etc/resolv.conf` contains resolver configuration on many Linux systems.
- `dig` is an important DNS troubleshooting tool.
- DORA represents the DHCP allocation process.
- DNS and DHCP are essential for Linux and cloud networking.
- Systematic troubleshooting is more effective than guesswork.

---

# 📸 Screenshots

Recommended screenshots:

```text
screenshots/
├── 01-ipv4-address.png
├── 02-resolv-conf.png
├── 03-hosts-file.png
├── 04-dig-query.png
└── 05-dns-troubleshooting.png
```

---

# 🚀 Day 14 Completed

Today I strengthened my understanding of DNS resolution, DNS records, DHCP, Linux network configuration and network troubleshooting.

Next:

Day 15 — Routing, NAT & Troubleshooting
