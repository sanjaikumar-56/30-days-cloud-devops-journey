# 🌐 Day 13 — TCP, UDP & Ports

Part of my 30-Day Cloud & DevOps Learning Journey.

Today I moved deeper into Layer 4 of networking and focused on how applications communicate using TCP, UDP and port numbers.

---

## 🎯 Objectives

- Understand the Transport Layer
- Understand TCP
- Understand UDP
- Understand port numbers
- Understand source and destination ports
- Understand ephemeral ports
- Understand TCP three-way handshake
- Understand TCP connection states
- Learn Linux socket inspection
- Practice TCP connectivity testing
- Understand port troubleshooting

---

# 1. Transport Layer

The Transport Layer is Layer 4 of the OSI model.

```text
Layer 7 → Application
Layer 6 → Presentation
Layer 5 → Session
Layer 4 → Transport       ← Day 13
Layer 3 → Network
Layer 2 → Data Link
Layer 1 → Physical
```

The Transport Layer provides communication between applications running on different hosts.

The two major transport protocols are:

```text
TCP
UDP
```

---

# 2. What is a Port?

An IP address identifies a host.

A port identifies a service or application endpoint on that host.

Example:

```text
192.168.1.10:22
```

Here:

```text
IP Address → 192.168.1.10
Port       → 22
```

Port 22 is normally used by SSH.

Another example:

```text
192.168.1.10:443
```

Port 443 is normally used for HTTPS.

---

# 3. Why Are Ports Required?

A server can run multiple network services at the same time.

```text
                Linux Server
              192.168.1.10
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      SSH :22    HTTP :80   HTTPS :443
```

The IP address gets traffic to the correct host.

The destination port helps the operating system deliver that traffic to the correct service.

---

# 4. TCP

TCP stands for Transmission Control Protocol.

TCP is:

- Connection-oriented
- Reliable
- Ordered
- Error-aware
- Uses acknowledgements
- Supports retransmission
- Provides flow control
- Provides congestion control

TCP is commonly used for:

```text
SSH
HTTP
HTTPS
FTP
SMTP
MySQL
PostgreSQL
```

The main goal of TCP is reliable communication.

---

# 5. UDP

UDP stands for User Datagram Protocol.

UDP is:

- Connectionless
- Lightweight
- Lower overhead
- Does not provide TCP-style delivery guarantees
- Does not provide TCP-style ordering
- Does not perform a TCP three-way handshake

UDP is commonly used for:

```text
DNS
DHCP
VoIP
Streaming
Real-time applications
Online gaming
```

The main advantage is simplicity and low protocol overhead.

---

# 6. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable delivery | No TCP-style delivery guarantee |
| Ordered data | No TCP-style ordering |
| Retransmission | No TCP-style retransmission |
| Higher overhead | Lower overhead |
| Connection state | No TCP-style connection establishment |
| SSH, HTTP, HTTPS | DNS, DHCP, real-time traffic |

Simple way to remember:

```text
TCP → Reliability
UDP → Simplicity / Low overhead
```

---

# 7. TCP Three-Way Handshake

TCP uses a three-way handshake to establish a connection.

```text
Client                         Server
  │                              │
  │ -------- SYN --------------> │
  │                              │
  │ <------ SYN + ACK ----------- │
  │                              │
  │ -------- ACK --------------> │
  │                              │
  │       Connection Ready       │
```

### Step 1 — SYN

The client requests a TCP connection.

```text
Client → Server
SYN
```

### Step 2 — SYN-ACK

The server acknowledges the request and sends its own synchronization information.

```text
Server → Client
SYN + ACK
```

### Step 3 — ACK

The client acknowledges the server.

```text
Client → Server
ACK
```

The TCP connection can now carry application data.

---

# 8. TCP Connection Example

Suppose a client connects to an SSH server:

```text
Client:
192.168.1.20:50000

Server:
192.168.1.10:22
```

The communication can be represented as:

```text
Client
192.168.1.20:50000
       │
       │ TCP
       ▼
Server
192.168.1.10:22
```

The server listens on port 22.

The client normally uses a temporary ephemeral source port.

---

# 9. Source and Destination Ports

Example:

```text
Client:
192.168.1.20:52341

Server:
142.250.x.x:443
```

Therefore:

```text
Source IP        → 192.168.1.20
Source Port      → 52341

Destination IP   → 142.250.x.x
Destination Port → 443
```

The server listens on its application port.

The client normally receives an ephemeral source port.

---

# 10. Ephemeral Ports

An ephemeral port is a temporary client-side port assigned by the operating system for a network connection.

Example:

```text
192.168.1.20:52341
        │
        │ TCP
        ▼
142.250.x.x:443
```

Here:

```text
52341 → Ephemeral client port
443   → Server application port
```

The exact ephemeral port range depends on the operating system and configuration.

---

# 11. TCP Connection States

Common TCP states include:

```text
LISTEN
SYN-SENT
SYN-RECEIVED
ESTABLISHED
FIN-WAIT
CLOSE-WAIT
TIME-WAIT
```

Linux command:

```bash
ss -ant
```

---

# 12. LISTEN

Example:

```text
LISTEN 0 128 0.0.0.0:22
```

This means a service is waiting for incoming TCP connections on port 22.

For SSH:

```text
sshd
  ↓
TCP 22
  ↓
LISTEN
```

---

# 13. ESTABLISHED

Example:

```text
ESTAB
```

means the TCP connection has been established.

```text
Client ←──── TCP ────→ Server
             │
          ESTABLISHED
```

---

# 14. TIME-WAIT

After a TCP connection closes, one endpoint may enter:

```text
TIME-WAIT
```

This helps TCP handle delayed packets and connection termination safely.

TIME-WAIT by itself does not mean the server is broken.

---

# 15. CLOSE-WAIT

`CLOSE-WAIT` indicates that the remote endpoint has closed its side of the connection, while the local application has not yet completely closed its side.

A persistent or unusually large number of CLOSE-WAIT connections can indicate an application resource-handling problem.

---

# 16. Linux `ss` Command

Show TCP sockets:

```bash
ss -t
```

Show UDP sockets:

```bash
ss -u
```

Show listening sockets:

```bash
ss -l
```

Show numeric addresses:

```bash
ss -n
```

Show processes:

```bash
sudo ss -p
```

Combined:

```bash
sudo ss -lntup
```

Options:

```text
-l → Listening
-n → Numeric
-t → TCP
-u → UDP
-p → Process
```

---

# 17. Find a Specific Port

Find SSH:

```bash
sudo ss -lntp | grep ':22'
```

Find HTTP:

```bash
sudo ss -lntp | grep ':80'
```

Find HTTPS:

```bash
sudo ss -lntp | grep ':443'
```

Find MySQL:

```bash
sudo ss -lntp | grep ':3306'
```

---

# 18. Netcat

Netcat (`nc`) can be used to test network connectivity.

Syntax:

```bash
nc -vz <SERVER_IP> <PORT>
```

Example:

```bash
nc -vz 192.168.1.10 22
```

If successful, you may see:

```text
Connection to 192.168.1.10 22 port [tcp/ssh] succeeded!
```

This confirms that a TCP connection to the specified port could be established.

---

# 19. Testing a Web Port

```bash
nc -vz SERVER_IP 80
```

For HTTPS:

```bash
nc -vz SERVER_IP 443
```

If the port is reachable, the TCP connection should succeed.

If it fails, investigate the network path, firewall rules, service state and listening socket.

---

# 20. Connection Refused vs Timeout

These are useful troubleshooting clues.

## Connection Refused

Usually means the host was reachable but the connection was actively rejected.

Possible causes:

- Service is not running
- Nothing is listening on that port
- Host firewall is rejecting the connection

## Connection Timeout

Usually means no response was received within the expected time.

Possible causes:

- Firewall
- AWS Security Group
- NACL
- Routing issue
- Network path issue
- Filtering

These are clues rather than absolute proof; the exact behavior depends on the network and firewall configuration.

---

# 21. Network Troubleshooting Flow

Scenario:

```text
Application is not working
```

Use:

```text
IP Connectivity
      ↓
TCP/UDP Connectivity
      ↓
Port
      ↓
Listening Socket
      ↓
Process
      ↓
Service
      ↓
Logs
      ↓
Application
```

For example:

```bash
ping -c 4 SERVER_IP

nc -vz SERVER_IP 443

sudo ss -lntp | grep ':443'

systemctl status nginx

journalctl -u nginx -n 50
```

---

# 22. SSH Troubleshooting Example

Problem:

```text
SSH connection is not working.
```

### Client side

Check route:

```bash
ip route
```

Test connectivity:

```bash
ping -c 4 SERVER_IP
```

Test TCP 22:

```bash
nc -vz SERVER_IP 22
```

### Server side

Check listening port:

```bash
sudo ss -lntp | grep ':22'
```

Check SSH service:

```bash
systemctl status ssh
```

Check logs:

```bash
journalctl -u ssh -n 50
```

---

# 23. Useful Ports

| Port | Service | Transport |
|---:|---|---|
| 20/21 | FTP | TCP |
| 22 | SSH | TCP |
| 23 | Telnet | TCP |
| 25 | SMTP | TCP |
| 53 | DNS | UDP/TCP |
| 67/68 | DHCP | UDP |
| 80 | HTTP | TCP |
| 110 | POP3 | TCP |
| 143 | IMAP | TCP |
| 443 | HTTPS | TCP |
| 3306 | MySQL | TCP |
| 5432 | PostgreSQL | TCP |
| 3389 | RDP | TCP |

---

# 24. Hands-On Lab

Run:

```bash
sudo ss -lntup
```

Then:

```bash
ss -ant
```

Then:

```bash
ss -lunp
```

Find SSH:

```bash
sudo ss -lntp | grep ':22'
```

Test the SSH port:

```bash
nc -vz localhost 22
```

If a web server is installed:

```bash
nc -vz localhost 80
```

Then:

```bash
curl -I http://localhost
```

Check routing:

```bash
ip route get 8.8.8.8
```

---

# 25. Interview Questions

### What is TCP?

TCP is a connection-oriented transport protocol that provides reliable and ordered delivery of data.

### What is UDP?

UDP is a connectionless transport protocol with low overhead that does not provide TCP-style guarantees for delivery or ordering.

### Explain the TCP three-way handshake.

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
```

The client sends SYN, the server responds with SYN-ACK, and the client sends ACK to establish the connection.

### How do you check listening ports?

```bash
sudo ss -lntup
```

### How do you find which process is using port 80?

```bash
sudo ss -lntp | grep ':80'
```

### How do you test whether TCP port 443 is reachable?

```bash
nc -vz SERVER_IP 443
```

### What is an ephemeral port?

A temporary client-side port assigned by the operating system for a network connection.

### What does LISTEN mean?

A service is waiting for incoming TCP connections.

### What does ESTABLISHED mean?

A TCP connection has been successfully established between two endpoints.

---

# 🎯 Key Takeaway

The most important concept from Day 13:

```text
IP Address
     ↓
Port
     ↓
TCP / UDP
     ↓
Socket
     ↓
Process
     ↓
Service
     ↓
Application
```

And the troubleshooting approach:

```text
IP connectivity
      ↓
Port connectivity
      ↓
Listening socket
      ↓
Process
      ↓
Service
      ↓
Logs
      ↓
Application
```

This knowledge will directly help when troubleshooting:

- SSH
- Nginx/Apache
- AWS EC2
- Security Groups
- NACLs
- Load Balancers
- Docker networking

---

## 📸 Screenshots

Recommended:

```text
screenshots/
├── 01-listening-ports.png
├── 02-tcp-connection-states.png
├── 03-port-22.png
├── 04-nc-port-test.png
└── 05-network-troubleshooting.png
```

Avoid exposing passwords, private keys, access tokens or sensitive infrastructure details.
