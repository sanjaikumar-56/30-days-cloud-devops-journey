# Day 06 — Linux Networking Fundamentals

## 📌 Overview

Today I focused on Linux networking fundamentals and practiced commands used to inspect network interfaces, IP addresses, routes, DNS, ports, and connectivity.

The main objective was to understand how to troubleshoot a Linux server when network connectivity is not working.

---

## 1. Network Interfaces

Learned how Linux represents network connections through network interfaces.

Common interfaces include:

* `eth0`
* `ens33`
* `enp0s3`
* `lo`

Checked interfaces using:

```bash
ip link
```

```bash
ip addr
```

IPv4 addresses:

```bash
ip -4 addr
```

---

## 2. Loopback Interface

Learned about the loopback interface:

```text
lo
```

The common IPv4 loopback address is:

```text
127.0.0.1
```

Tested using:

```bash
ping -c 4 127.0.0.1
```

and:

```bash
ping -c 4 localhost
```

---

## 3. IP Address

Practiced identifying:

* IPv4 address
* CIDR prefix
* Network interface
* Interface state
* MAC address

Commands:

```bash
ip addr
```

```bash
ip link
```

---

## 4. MAC Address

Used:

```bash
ip link
```

to identify the MAC address associated with a network interface.

A MAC address operates at the data-link layer and is used for communication within the local network segment.

---

## 5. Routing Table

Checked the Linux routing table:

```bash
ip route
```

Learned how the routing table determines where packets should be sent.

Example:

```text
default via 192.168.1.1 dev eth0
```

Here:

```text
default → default route
192.168.1.1 → gateway
eth0 → network interface
```

---

## 6. Default Gateway

Identified the default gateway using:

```bash
ip route
```

The default gateway is normally used when there is no more specific route for the destination.

---

## 7. Route to a Specific Destination

Practiced:

```bash
ip route get 8.8.8.8
```

This helps identify:

* Destination
* Gateway
* Interface
* Source IP

---

## 8. Ping

Used `ping` to test IP-level connectivity.

```bash
ping -c 4 8.8.8.8
```

Also tested hostname connectivity:

```bash
ping -c 4 google.com
```

Learned that a failed ping does not always prove that a server is unreachable because ICMP may be blocked by firewalls or network security controls.

---

## 9. DNS

Learned that DNS translates hostnames into IP addresses.

Practiced:

```bash
getent hosts google.com
```

```bash
nslookup google.com
```

and:

```bash
dig +short google.com
```

---

## 10. DNS Configuration

Inspected:

```bash
cat /etc/resolv.conf
```

Also checked local hostname mappings:

```bash
cat /etc/hosts
```

Learned that `/etc/resolv.conf` may be generated or managed by another network-management component on modern Linux systems.

---

## 11. Hostname

Checked the system hostname:

```bash
hostname
```

Detailed information:

```bash
hostnamectl
```

---

## 12. Listening Ports

Used `ss` to inspect listening sockets:

```bash
sudo ss -lntp
```

Checked specific ports:

```bash
sudo ss -lntp | grep ':22'
```

```bash
sudo ss -lntp | grep ':80'
```

Learned how to identify which processes are listening on TCP ports.

---

## 13. Understanding `ss -lntp`

```text
-l → listening
-n → numeric addresses and ports
-t → TCP
-p → process information
```

Therefore:

```bash
sudo ss -lntp
```

shows listening TCP sockets and their associated processes.

---

## 14. Testing Port Connectivity

Practiced Netcat:

```bash
nc -vz <server-ip> 22
```

```bash
nc -vz <server-ip> 80
```

```bash
nc -vz <server-ip> 443
```

This helps test whether a TCP port is reachable.

---

## 15. HTTP Connectivity

Used:

```bash
curl -I https://example.com
```

and:

```bash
curl -v https://example.com
```

Learned how `curl -v` can provide useful information about DNS resolution, TCP connection, TLS, HTTP requests, and responses.

---

## 16. Network Statistics

Checked interface statistics:

```bash
ip -s link
```

Inspected:

* RX packets
* TX packets
* Errors
* Dropped packets

---

## 17. Neighbor Table

Checked the Linux neighbor table:

```bash
ip neigh
```

Learned how IPv4 communication on a local Ethernet network uses address resolution between IP addresses and MAC addresses.

---

## 18. Traceroute

Learned how traceroute can be used to inspect the network path toward a destination.

```bash
traceroute google.com
```

Alternative:

```bash
tracepath google.com
```

A missing response from a hop does not necessarily mean traffic is broken because intermediate routers may not respond to traceroute probes.

---

## 19. NetworkManager

Checked NetworkManager when available:

```bash
systemctl status NetworkManager
```

Practiced:

```bash
nmcli device status
```

and:

```bash
nmcli connection show
```

Learned that `nmcli` is the command-line interface for NetworkManager.

---

# 🧪 Network Troubleshooting Flow

Practiced a structured troubleshooting approach:

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
Test IP Connectivity
      ↓
Check DNS
      ↓
Check Port Connectivity
      ↓
Check Application
      ↓
Identify Root Cause
```

Example commands:

```bash
ip link
ip addr
ip route
ping -c 4 <gateway>
ping -c 4 8.8.8.8
getent hosts google.com
nc -vz <server-ip> <port>
curl -v <URL>
```

---

# 🎤 Interview Questions Practiced

1. What is a network interface?
2. What is the loopback interface?
3. What is `127.0.0.1`?
4. How do you check an IP address in Linux?
5. How do you check a MAC address?
6. How do you check the routing table?
7. What is a default gateway?
8. What is DNS?
9. What is `/etc/hosts`?
10. What is `/etc/resolv.conf`?
11. What is `ss`?
12. How do you check listening ports?
13. What is the difference between ping and `nc`?
14. What is `ip route get`?
15. What is traceroute?
16. What is `ip neigh`?
17. What is NetworkManager?
18. What is `nmcli`?
19. How do you troubleshoot a server with no internet?
20. What would you check if an IP works but a hostname does not?

---

# ⭐ Important Troubleshooting Scenario

### Problem

A server can reach:

```text
8.8.8.8
```

but cannot resolve:

```text
google.com
```

### Approach

```text
IP connectivity works
        ↓
Investigate DNS
        ↓
getent hosts google.com
        ↓
nslookup / dig
        ↓
Check resolver configuration
        ↓
Identify DNS issue
```

This helped me understand the difference between **network connectivity** and **DNS resolution**.

---

# 📸 Practical Evidence

Screenshots from the hands-on exercises are stored in the `screenshots/` directory.

---

## 🎯 Key Takeaway

Today I learned that Linux network troubleshooting should be systematic rather than based on guesswork.

Instead of immediately restarting networking services, I should identify where the failure occurs:

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

**Day 6 completed ✅**

#Linux #Networking #LinuxAdministration #CloudComputing #DevOps #AWS #CloudEngineer

