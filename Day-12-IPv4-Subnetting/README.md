# 🌐 Day 12 — IPv4 & Subnetting

Part of my 30-Day Cloud & DevOps Learning Journey.

Today I focused on IPv4 addressing and subnetting, with an emphasis on understanding how IP networks are divided and how Linux identifies network configuration and routes.

## 🎯 Objectives

- Understand IPv4 addressing
- Understand 32-bit IPv4 structure
- Understand binary representation
- Understand network and host portions
- Understand subnet masks
- Understand CIDR notation
- Calculate network and broadcast addresses
- Calculate usable host ranges
- Understand private IPv4 ranges
- Practice subnetting
- Inspect Linux IP and routing configuration

---

# 1. What is IPv4?

IPv4 is a Layer 3 addressing protocol that uses 32-bit addresses.

Example:

```text
192.168.1.10
```

An IPv4 address consists of four 8-bit octets.

```text
192 . 168 . 1 . 10
 │     │     │    │
 8     8     8    8 bits

Total = 32 bits
```

Each octet can have a value from:

```text
0 - 255
```

because:

```text
2^8 = 256
```

---

# 2. IPv4 Binary Representation

Example:

```text
192.168.1.10
```

Binary:

```text
11000000.10101000.00000001.00001010
```

Each octet contains 8 bits:

```text
128 64 32 16 8 4 2 1
```

---

# 3. Network Portion and Host Portion

Consider:

```text
192.168.1.10/24
```

The `/24` means that the first 24 bits represent the network portion.

```text
Network                     Host
<------------------------>  <------>
192.168.1                   .10

24 bits                       8 bits
```

Therefore:

```text
Network:
192.168.1.0

Host:
10
```

---

# 4. CIDR

CIDR stands for Classless Inter-Domain Routing.

Example:

```text
192.168.1.10/24
```

The `/24` represents the number of network prefix bits.

Common CIDR values:

```text
/16
/17
/18
/19
/20
/21
/22
/23
/24
/25
/26
/27
/28
/29
/30
```

A larger CIDR prefix means fewer host addresses in the subnet.

---

# 5. Subnet Masks

For:

```text
192.168.1.10/24
```

the subnet mask is:

```text
255.255.255.0
```

Binary:

```text
11111111.11111111.11111111.00000000
```

Therefore:

```text
24 network bits
8 host bits
```

---

# 6. CIDR and Address Count

The total number of IPv4 addresses in a subnet is:

```text
2^(32 - prefix)
```

Examples:

```text
/24

2^(32-24)
= 2^8
= 256 addresses
```

```text
/26

2^(32-26)
= 2^6
= 64 addresses
```

```text
/28

2^(32-28)
= 2^4
= 16 addresses
```

For traditional IPv4 host subnets, the usable host count is:

```text
2^(host bits) - 2
```

because the network and broadcast addresses are not assigned to ordinary hosts.

---

# 7. Common CIDR Cheat Sheet

| CIDR | Subnet Mask | Total Addresses | Traditional Usable Hosts |
|---|---|---:|---:|
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /20 | 255.255.240.0 | 4,096 | 4,094 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

Note: Cloud providers can reserve addresses inside subnets, so cloud-specific usable-address counts may differ from the traditional formula.

---

# 8. Network Address

The network address identifies the subnet itself.

Example:

```text
192.168.1.0/24
```

Network address:

```text
192.168.1.0
```

It represents the entire subnet rather than an individual host.

---

# 9. Broadcast Address

For a traditional IPv4 subnet, the broadcast address is the final address in the subnet.

Example:

```text
192.168.1.0/24
```

Broadcast:

```text
192.168.1.255
```

Traditional usable host range:

```text
192.168.1.1
-
192.168.1.254
```

---

# 10. Subnetting with Block Size

A useful method for subnetting is the block-size method.

Formula:

```text
Block Size = 256 - subnet mask value
```

For `/26`:

```text
Subnet Mask:
255.255.255.192

Block Size:
256 - 192

= 64
```

Therefore subnet boundaries are:

```text
0
64
128
192
```

---

# 11. Subnetting Example

Find the network and broadcast addresses for:

```text
192.168.10.75/26
```

### Step 1 — Find subnet mask

```text
/26
=
255.255.255.192
```

### Step 2 — Find block size

```text
256 - 192
=
64
```

### Step 3 — Find subnet boundaries

```text
0
64
128
192
```

### Step 4 — Locate 75

```text
64 ≤ 75 < 128
```

Therefore:

```text
Network:
192.168.10.64
```

Broadcast:

```text
192.168.10.127
```

Traditional usable host range:

```text
192.168.10.65
-
192.168.10.126
```

---

# 12. /24 Divided into /26 Subnets

Starting network:

```text
192.168.1.0/24
```

Dividing it into `/26` gives four subnets:

```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

Conceptually:

```text
192.168.1.0/24
        │
        ├── 192.168.1.0/26
        │
        ├── 192.168.1.64/26
        │
        ├── 192.168.1.128/26
        │
        └── 192.168.1.192/26
```

Each subnet contains:

```text
64 total addresses
62 traditional usable hosts
```

---

# 13. Private IPv4 Address Ranges

Private IPv4 ranges defined by RFC 1918 are:

### 10.0.0.0/8

```text
10.0.0.0
-
10.255.255.255
```

### 172.16.0.0/12

```text
172.16.0.0
-
172.31.255.255
```

### 192.168.0.0/16

```text
192.168.0.0
-
192.168.255.255
```

These addresses are used for private network communication and are not globally routable on the public Internet.

---

# 14. Special IPv4 Addresses

## Loopback

```text
127.0.0.0/8
```

Most commonly:

```text
127.0.0.1
```

Used by a system to communicate with itself.

## Link-local

```text
169.254.0.0/16
```

Used for link-local addressing in certain situations.

## Unspecified address

```text
0.0.0.0
```

For example:

```text
0.0.0.0:80
```

can mean a service is listening on port 80 on all IPv4 interfaces.

---

# 15. Public vs Private IP

Private IP:

```text
10.0.1.10
```

Used inside private networks.

Public IP:

```text
203.x.x.x
```

A publicly routable IPv4 address.

Conceptually:

```text
Private Network
      │
      │
      ▼
    NAT
      │
      ▼
  Internet
```

This concept becomes important when working with AWS VPCs and NAT Gateway.

---

# 16. Linux Networking Commands

## Check IP addresses

```bash
ip addr
```

## IPv4 addresses only

```bash
ip -4 addr
```

## Check network interfaces

```bash
ip link
```

## Check routing table

```bash
ip route
```

## Check neighbor table

```bash
ip neigh
```

## Find the route Linux will use

```bash
ip route get 8.8.8.8
```

Example:

```text
8.8.8.8 via 192.168.1.1 dev eth0
```

---

# 17. Hands-On Lab

Run the following commands:

```bash
hostname

ip link

ip -4 addr

ip route

ip neigh

ip route get 8.8.8.8
```

Record:

- Active interface
- IPv4 address
- CIDR prefix
- MAC address
- Default gateway
- Routing information

---

# 18. Subnetting Practice

### Problem 1

```text
192.168.10.50/24
```

Find:

- Network
- Broadcast
- First host
- Last host
- Traditional usable host count

### Problem 2

```text
10.0.5.70/26
```

Find:

- Network
- Broadcast
- First host
- Last host

### Problem 3

```text
172.16.20.200/27
```

Find:

- Network
- Broadcast
- First host
- Last host

### Problem 4

Calculate traditional usable hosts for:

```text
/25
/26
/27
/28
```

---

# 19. Interview Questions

### Q1. What is IPv4?

IPv4 is a Layer 3 addressing protocol that uses 32-bit addresses to identify interfaces/hosts on an IP network.

### Q2. What is CIDR?

CIDR is a classless method of representing IP networks using a prefix length such as `/24` or `/26`.

### Q3. What does /24 mean?

It means that the first 24 bits are the network prefix and the remaining 8 bits are available for hosts.

### Q4. What is the subnet mask for /26?

```text
255.255.255.192
```

### Q5. How many total addresses are in /26?

```text
2^(32-26)
= 64
```

### Q6. How many traditional usable hosts are in /26?

```text
64 - 2
= 62
```

### Q7. What are the private IPv4 ranges?

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

### Q8. Why do we subnet networks?

Subnetting allows us to divide a larger network into smaller logical networks, improving address utilization, organization, isolation and routing design.

---

# 🎯 Key Takeaway

The most important subnetting workflow I practiced today is:

```text
CIDR
  ↓
Subnet Mask
  ↓
Block Size
  ↓
Subnet Boundaries
  ↓
Network Address
  ↓
Broadcast Address
  ↓
Usable Host Range
```

This foundation will be directly useful when I start working with:

- AWS VPC
- Public and Private Subnets
- Route Tables
- NAT Gateway
- Security Groups
- NACLs
- EC2 Networking
- Cloud Network Troubleshooting

---

## 📸 Screenshots

Recommended screenshots:

```text
screenshots/
├── 01-ipv4-address.png
├── 02-routing-table.png
├── 03-subnet-calculation.png
├── 04-subnetting-practice.png
└── 05-route-get.png
```

Avoid exposing passwords, private keys, access tokens, or other sensitive infrastructure information.
