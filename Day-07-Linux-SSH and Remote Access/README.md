# Day 07 — Linux SSH & Remote Access

## 📌 Overview

Today I learned and practiced SSH (Secure Shell), one of the fundamental technologies used for remotely managing Linux servers.

I focused on SSH authentication, SSH keys, server configuration, permissions, troubleshooting, and secure remote access.

---

## 1. What is SSH?

SSH (Secure Shell) is a protocol used to securely access and manage remote systems over an encrypted connection.

The default SSH port is:

```text
TCP 22
```

Basic connection:

```bash
ssh username@server-ip
```

---

## 2. SSH Client and Server

### SSH Client

The system initiating the connection.

```bash
ssh user@server
```

### SSH Server

The remote system accepting the connection.

The SSH server process is commonly:

```text
sshd
```

---

## 3. SSH Service

Checked the SSH service using:

```bash
sudo systemctl status ssh
```

On systems using `sshd` as the service name:

```bash
sudo systemctl status sshd
```

Also practiced:

```bash
sudo systemctl start ssh
sudo systemctl enable ssh
```

---

## 4. Check SSH Port

Verified whether SSH is listening on TCP port 22:

```bash
sudo ss -lntp | grep ':22'
```

This helped connect service management with networking concepts learned earlier.

---

## 5. SSH Client

Checked the installed SSH client:

```bash
ssh -V
```

Basic connection syntax:

```bash
ssh username@server-ip
```

Custom port:

```bash
ssh -p 2222 username@server-ip
```

Identity file:

```bash
ssh -i private-key username@server-ip
```

---

## 6. SSH Key-Based Authentication

Learned about SSH public/private key authentication.

```text
Private Key
    ↓
Client

Public Key
    ↓
Server
```

Generated an Ed25519 key pair using:

```bash
ssh-keygen -t ed25519
```

Typical files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

### Important

The private key must remain private and should never be uploaded to GitHub or shared publicly.

---

## 7. Public Key

Displayed the public key using:

```bash
cat ~/.ssh/id_ed25519.pub
```

The public key can be placed on the remote server in:

```text
~/.ssh/authorized_keys
```

---

## 8. authorized_keys

Inspected:

```bash
cat ~/.ssh/authorized_keys
```

The file contains public keys that are allowed to authenticate for that user.

---

## 9. SSH Permissions

Practiced secure permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 600 ~/.ssh/authorized_keys
```

Restricted permissions are important for protecting SSH credentials.

---

## 10. SSH Configuration

Inspected the SSH server configuration:

```bash
sudo cat /etc/ssh/sshd_config
```

Also checked effective configuration:

```bash
sudo sshd -T
```

Important configuration concepts include:

* Port
* PermitRootLogin
* PasswordAuthentication
* PubkeyAuthentication
* AllowUsers

---

## 11. Validate SSH Configuration

Before restarting SSH after configuration changes:

```bash
sudo sshd -t
```

If the configuration is valid, the command normally produces no output.

This helps reduce the risk of breaking SSH access due to configuration syntax errors.

---

## 12. SSH Verbose Mode

Practiced SSH troubleshooting using:

```bash
ssh -v username@server
```

More detailed:

```bash
ssh -vv username@server
```

Maximum debugging:

```bash
ssh -vvv username@server
```

Verbose mode helps identify where a connection is failing, such as:

```text
Connection
    ↓
SSH handshake
    ↓
Host key
    ↓
Authentication
    ↓
Session
```

---

## 13. known_hosts

Learned about:

```text
~/.ssh/known_hosts
```

This file stores known SSH server host keys.

Checked entries using:

```bash
ssh-keygen -F hostname
```

Learned that unexpected host-key changes should be investigated rather than blindly ignored.

---

## 14. SSH Logs

Practiced checking SSH logs:

```bash
sudo journalctl -u ssh -n 50
```

On systems using `sshd`:

```bash
sudo journalctl -u sshd -n 50
```

These logs are useful when investigating authentication and service problems.

---

## 15. Testing Port 22

Used Netcat to test TCP connectivity:

```bash
nc -vz SERVER_IP 22
```

This helps determine whether TCP port 22 is reachable.

---

## 16. SSH and sudo

Learned that connecting through SSH does not automatically provide root access.

Checked the current user:

```bash
whoami
```

User information:

```bash
id
```

Sudo permissions:

```bash
sudo -l
```

Administrative commands can be executed through `sudo` when permitted.

---

## 17. SCP

Learned that SSH can also be used for secure file transfer.

Upload:

```bash
scp file.txt user@server:/tmp/
```

Download:

```bash
scp user@server:/tmp/file.txt .
```

Copy a directory:

```bash
scp -r folder user@server:/tmp/
```

---

## 18. SSH Troubleshooting

Practiced the following troubleshooting approach:

```text
SSH Connection Failure
        ↓
Check IP / DNS
        ↓
Check network connectivity
        ↓
Check TCP port 22
        ↓
Check Security Group / Firewall
        ↓
Check sshd service
        ↓
Check listening port
        ↓
Check username
        ↓
Check SSH key
        ↓
Check permissions
        ↓
Check authentication logs
        ↓
Use ssh -vvv
```

---

## 19. AWS EC2 SSH Troubleshooting

For an EC2 instance, important checks include:

* Correct instance state
* Correct public/private IP
* Security Group
* Network ACL
* Route table
* Port 22
* SSH service
* Correct username
* Correct private key
* Private-key permissions

Example:

```bash
ssh -i key.pem ubuntu@SERVER_IP
```

The default username depends on the AMI, so it should be verified rather than assumed.

---

# 🎤 Interview Questions Practiced

1. What is SSH?
2. Which port does SSH use?
3. What is sshd?
4. SSH client vs SSH server?
5. How do you check SSH service status?
6. How do you check whether port 22 is listening?
7. What is SSH key-based authentication?
8. Public key vs private key?
9. What is `authorized_keys`?
10. What is `known_hosts`?
11. What is `ssh-keygen`?
12. What is `ssh-copy-id`?
13. Why should SSH private-key permissions be restricted?
14. What does `ssh -vvv` do?
15. How do you troubleshoot SSH connection failure?
16. Connection refused vs connection timeout?
17. How do you validate `sshd_config`?
18. Why is direct root SSH access generally avoided?
19. How do you troubleshoot SSH on an AWS EC2 instance?
20. What is SCP?

---

# ⭐ Key Takeaway

SSH is more than just:

```bash
ssh user@server
```

A reliable SSH troubleshooting approach requires understanding:

```text
Network
   ↓
TCP 22
   ↓
sshd
   ↓
User
   ↓
SSH Key
   ↓
Permissions
   ↓
Authentication
   ↓
Remote Session
```

**Day 7 completed ✅**

#Linux #SSH #LinuxAdministration #CloudComputing #DevOps #AWS #CloudEngineer

