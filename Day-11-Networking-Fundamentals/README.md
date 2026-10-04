# 🌐 Day 11 — Networking Fundamentals

Part of my **30-Day Cloud & DevOps Learning Journey**.

Today I started the **Networking** phase of the journey after completing the Linux Administration fundamentals.

The goal was to understand how Linux systems communicate over networks and how to approach basic network troubleshooting systematically.

---

## 🎯 Objectives

- Understand the OSI model
- Understand the TCP/IP model
- Understand MAC and IP addresses
- Understand network interfaces
- Understand ports and sockets
- Understand client-server communication
- Understand basic routing
- Practice Linux networking commands
- Perform basic network troubleshooting

---

# 1. What is Networking?

Computer networking allows systems to communicate and exchange data.

A simplified communication flow is:

```text
Application
     ↓
Transport
     ↓
IP / Network
     ↓
Network Interface
     ↓
Physical Network
```

For example, when connecting to an SSH server:

```text
Client
  │
  │ TCP
  │ Destination Port 22
  ▼
Linux Server
  │
  └── SSH Service
```

---

# 2. OSI Model

The OSI model contains seven conceptual layers.

```text
Layer 7 → Application
Layer 6 → Presentation
Layer 5 → Session
Layer 4 → Transport
Layer 3 → Network
Layer 2 → Data Link
Layer 1 → Physical
```

## Layer 1 — Physical

Responsible for transmitting raw signals.

Examples:

- Ethernet cables
- Fiber
- Radio signals

## Layer 2 — Data Link

Responsible for communication on the local network.

Important concept:

**MAC Address**

## Layer 3 — Network

Responsible for logical addressing and routing.

Important concept:

**IP Address**

## Layer 4 — Transport

Responsible for host-to-host communication.

Main protocols:

- TCP
- UDP

## Layer 5 — Session

Responsible for managing communication sessions.

## Layer 6 — Presentation

Deals with data representation, encoding, encryption and compression.

## Layer 7 — Application

Provides network functionality to applications.

Examples:

- HTTP
- HTTPS
- DNS
- SSH
- FTP

---

# 3. TCP/IP Model

The TCP/IP model is commonly represented using four layers:

```text
Application
     ↓
Transport
     ↓
Internet
     ↓
Network Access
```

Simplified OSI mapping:

| OSI | TCP/IP |
|---|---|
| Application | Application |
| Presentation | Application |
| Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link | Network Access |
| Physical | Network Access |

---

# 4. MAC Address

A MAC address is associated with a network interface and operates primarily at Layer 2.

Example:

```text
00:1A:2B:3C:4D:5E
```

Linux command:

```bash
ip link
```

Look for:

```text
link/ether
```

Example:

```text
eth0
    link/ether 00:1A:2B:3C:4D:5E
```

---

# 5. IP Address

An IP address provides logical addressing for network communication.

Example:

```text
192.168.1.10
```

Linux commands:

```bash
ip addr
```

or:

```bash
ip -4 addr
```

Example:

```text
inet 192.168.1.10/24
```

---

# 6. MAC Address vs IP Address

| MAC Address | IP Address |
|---|---|
| Layer 2 | Layer 3 |
| Associated with network interface | Logical network address |
| Used primarily within local network communication | Used for routing between networks |
| Hexadecimal format | IPv4 uses dotted-decimal notation |
| Example: `00:1A:2B:3C:4D:5E` | Example: `192.168.1.10` |

A simple way to remember:

> MAC identifies the network interface, while IP provides logical addressing for network communication.

---

# 7. Network Interface

A network interface allows a system to communicate over a network.

Check interfaces:

```bash
ip link
```

Typical interfaces may include:

```text
lo
eth0
ens5
enp0s3
```

### Loopback

The loopback interface is normally:

```text
lo
```

with:

```text
127.0.0.1
```

It allows the system to communicate with itself.

---

# 8. Ports

An IP address identifies a host, while a port identifies a network service or endpoint on that host.

Example:

```text
192.168.1.10:22
```

Here:

```text
IP   → 192.168.1.10
Port → 22
```

Common ports:

| Service | Port | Protocol |
|---|---:|---|
| SSH | 22 | TCP |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| DNS | 53 | UDP/TCP |
| FTP | 21 | TCP |
| MySQL | 3306 | TCP |
| PostgreSQL | 5432 | TCP |
| RDP | 3389 | TCP |

---

# 9. Socket

A socket represents an endpoint of network communication.

A practical representation is:

```text
IP Address + Port + Protocol
```

Example:

```text
192.168.1.10:22/TCP
```

A TCP connection can be understood using:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

Example:

```text
Client
192.168.1.20:51432
        │
        │ TCP
        ▼
Server
192.168.1.10:22
```

---

# 10. Important Linux Networking Commands

## View network interfaces

```bash
ip link
```

## View IP addresses

```bash
ip addr
```

IPv4 only:

```bash
ip -4 addr
```

## View routing table

```bash
ip route
```

## View neighbor information

```bash
ip neigh
```

## Test connectivity

```bash
ping -c 4 8.8.8.8
```

## Test loopback

```bash
ping -c 4 127.0.0.1
```

## Check listening TCP ports

```bash
ss -lnt
```

## Check listening TCP/UDP ports with processes

```bash
sudo ss -lntup
```

## Check DNS resolution

```bash
getent hosts google.com
```

or:

```bash
dig google.com
```

## Test an HTTP/HTTPS endpoint

```bash
curl -I https://google.com
```

---

# 11. Understanding `ss`

Command:

```bash
sudo ss -lntup
```

Options:

```text
-l → Listening sockets
-n → Numeric addresses/ports
-t → TCP
-u → UDP
-p → Process information
```

This is especially useful when troubleshooting services.

For example:

```bash
sudo ss -lntp | grep ':80'
```

can help identify whether something is listening on TCP port 80.

---

# 12. Hands-On Lab

## Step 1 — Check hostname

```bash
hostname
```

## Step 2 — Check interfaces

```bash
ip link
```

## Step 3 — Check IPv4 address

```bash
ip -4 addr
```

## Step 4 — Check MAC address

```bash
ip link
```

Look for:

```text
link/ether
```

## Step 5 — Check routing table

```bash
ip route
```

Look for:

```text
default via <gateway>
```

## Step 6 — Test loopback

```bash
ping -c 4 127.0.0.1
```

## Step 7 — Test external IP

```bash
ping -c 4 8.8.8.8
```

## Step 8 — Test DNS

```bash
getent hosts google.com
```

## Step 9 — Check listening ports

```bash
sudo ss -lntup
```

## Step 10 — Test application connectivity

```bash
curl -I https://google.com
```

---

# 13. Network Troubleshooting Method

A common mistake is to immediately start changing configurations.

Instead, troubleshoot layer by layer.

```text
Network Interface
        ↓
IP Address
        ↓
Routing Table
        ↓
Default Gateway
        ↓
IP Connectivity
        ↓
DNS
        ↓
Port
        ↓
Application
```

---

# 14. Scenario — Server Cannot Access Internet

### Symptom

The Linux server has an IP address but cannot access the internet.

### Step 1 — Check interface

```bash
ip link
```

Confirm the interface is UP.

### Step 2 — Check IP

```bash
ip addr
```

Confirm the interface has an appropriate IP address.

### Step 3 — Check route

```bash
ip route
```

Look for a default route.

### Step 4 — Test gateway

```bash
ping -c 4 <gateway-ip>
```

### Step 5 — Test external IP

```bash
ping -c 4 8.8.8.8
```

### Step 6 — Test DNS

```bash
getent hosts google.com
```

or:

```bash
dig google.com
```

### Step 7 — Test application

```bash
curl -I https://google.com
```

---

# 15. Important Troubleshooting Logic

### Case 1

```text
Gateway ❌
```

Investigate:

- Interface
- IP configuration
- Route
- Local network

### Case 2

```text
Gateway ✅
8.8.8.8 ❌
```

Investigate:

- Routing
- Firewall
- NAT
- Upstream network
- Cloud networking configuration

### Case 3

```text
8.8.8.8 ✅
google.com ❌
```

Likely investigate:

- DNS configuration
- `/etc/resolv.conf`
- DNS server reachability
- DNS records

### Case 4

```text
DNS ✅
curl ❌
```

Investigate:

- Destination port
- Firewall
- Proxy
- TLS
- Application/server

This gives a much more reliable troubleshooting approach than simply running `ping`.

---

# 16. Interview Questions

### Q1. What is the OSI model?

The OSI model is a seven-layer conceptual model used to understand network communication:

```text
Physical
Data Link
Network
Transport
Session
Presentation
Application
```

### Q2. What is the difference between MAC and IP?

MAC is a Layer 2 address associated with a network interface, while IP is a Layer 3 logical address used for network communication and routing.

### Q3. What is a port?

A port is a logical endpoint used to identify a network service or application on a host.

### Q4. What is a socket?

A socket is an endpoint of network communication, commonly represented using an IP address, port and transport protocol.

### Q5. How do you check the IP address in Linux?

```bash
ip addr
```

or:

```bash
ip -4 addr
```

### Q6. How do you check the routing table?

```bash
ip route
```

### Q7. How do you check listening ports?

```bash
sudo ss -lntup
```

### Q8. How do you find which process is using port 80?

```bash
sudo ss -lntp | grep ':80'
```

or:

```bash
sudo lsof -i :80
```

### Q9. How would you troubleshoot a server that cannot access the internet?

I would check the interface, IP address, routing table, default gateway, external IP connectivity, DNS resolution and finally application-level connectivity.

---

# 17. Key Commands — Day 11 Cheat Sheet

```bash
ip link
ip addr
ip -4 addr
ip route
ip neigh
ping -c 4 <IP>
ss -lnt
sudo ss -lntup
getent hosts <domain>
dig <domain>
nslookup <domain>
curl -I <URL>
```

---

# 🎯 Day 11 Takeaway

The most important concept from today is the troubleshooting flow:

```text
Interface
   ↓
IP
   ↓
Route
   ↓
Gateway
   ↓
Connectivity
   ↓
DNS
   ↓
Port
   ↓
Application
```

This foundation will be used heavily in:

- AWS VPC
- EC2
- Security Groups
- NACLs
- Load Balancers
- Docker networking
- Kubernetes networking
- SSH troubleshooting
- Production server troubleshooting

---

## 📸 Screenshots

Recommended screenshots for this day's GitHub folder:

```text
screenshots/
├── 01-network-interface.png
├── 02-ip-address.png
├── 03-routing-table.png
├── 04-listening-ports.png
├── 05-dns-test.png
└── 06-connectivity-test.png
```

Avoid exposing passwords, private keys, tokens, or sensitive infrastructure information in screenshots.
