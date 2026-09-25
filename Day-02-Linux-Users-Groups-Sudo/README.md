
# Day 02 — Linux Users, Groups & sudo 🐧

## 📌 Overview

Day 2 of my 30-day Cloud & DevOps learning journey focused on **Linux user management, groups, authentication, authorization, and sudo**.

These concepts are essential for Linux administration, cloud servers, access control, and DevOps environments.

Alongside Linux, I also started practicing the **Git workflow** that I will use throughout this 30-day journey.

---

# 🐧 Linux

## 📚 Topics Covered

* Linux users
* UID and GID
* Root user
* Primary and supplementary groups
* `/etc/passwd`
* `/etc/shadow`
* `/etc/group`
* `/etc/gshadow`
* User management
* Group management
* `sudo`
* `/etc/sudoers`
* `/etc/sudoers.d/`
* `visudo`
* Authentication vs Authorization
* Principle of Least Privilege

---

## 1. Linux Users

A Linux user represents an identity on the system.

Every user has attributes such as:

* Username
* UID
* Primary GID
* Supplementary groups
* Home directory
* Login shell

Check the current user:

```bash
whoami
```

Display detailed identity information:

```bash
id
```

For a specific user:

```bash
id clouduser
```

---

## 2. UID — User ID

UID stands for **User ID**.

Linux internally identifies users using numeric UIDs.

The root user has:

```text
UID = 0
```

Check a user's UID:

```bash
id clouduser
```

---

## 3. GID — Group ID

GID stands for **Group ID**.

It identifies a Linux group numerically.

Example:

```text
uid=1001(clouduser)
gid=1001(clouduser)
```

---

## 4. Root User

The root user is the Linux superuser.

```text
Username: root
UID: 0
```

Root has extensive administrative privileges.

Examples:

* Creating and deleting users
* Modifying system configuration
* Installing software
* Managing services
* Changing ownership and permissions

Because unrestricted root access is risky, administrative tasks are commonly performed using `sudo`.

---

# 📁 Important Linux Account Files

| File              | Purpose                                        |
| ----------------- | ---------------------------------------------- |
| `/etc/passwd`     | Basic user account information                 |
| `/etc/shadow`     | Password hashes and password-aging information |
| `/etc/group`      | Group information                              |
| `/etc/gshadow`    | Secure group account information               |
| `/etc/sudoers`    | sudo authorization policy                      |
| `/etc/sudoers.d/` | Additional sudo policy files                   |

---

## 5. `/etc/passwd`

View user account information:

```bash
cat /etc/passwd
```

A typical entry contains seven fields:

```text
username:password-placeholder:UID:GID:GECOS:home:shell
```

Example:

```text
clouduser:x:1001:1001:Cloud User:/home/clouduser:/bin/bash
```

---

## 6. `/etc/shadow`

The `/etc/shadow` file contains password hashes and password-aging information.

It is protected because it contains sensitive authentication data.

View it with appropriate privileges:

```bash
sudo cat /etc/shadow
```

---

## 7. Groups

Groups allow administrators to manage permissions for multiple users collectively.

View groups:

```bash
groups
```

View a specific user's groups:

```bash
groups clouduser
```

More detailed information:

```bash
id clouduser
```

---

# 👤 User Management

## Create a user

```bash
sudo useradd -m -s /bin/bash clouduser
```

Set a password:

```bash
sudo passwd clouduser
```

Verify:

```bash
id clouduser
```

---

## Delete a user

```bash
sudo userdel clouduser
```

To remove the user's home directory as part of deletion:

```bash
sudo userdel -r clouduser
```

---

## Modify a user

Change the login shell:

```bash
sudo usermod -s /bin/bash clouduser
```

Add a supplementary group:

```bash
sudo usermod -aG developers clouduser
```

### Important: `-aG`

`-G` specifies supplementary groups.

`-a` means append.

Therefore:

```bash
sudo usermod -aG developers clouduser
```

adds the user to `developers` without replacing existing supplementary group memberships.

---

# 👥 Group Management

## Create a group

```bash
sudo groupadd developers
```

Create another group:

```bash
sudo groupadd cloudadmins
```

Check a group:

```bash
getent group developers
```

---

## Add user to a group

```bash
sudo usermod -aG developers clouduser
```

Verify:

```bash
id clouduser
```

---

## Remove user from a group

```bash
sudo gpasswd -d clouduser developers
```

Verify:

```bash
groups clouduser
```

---

# 🔐 sudo

`sudo` provides controlled privilege elevation according to the configured sudo policy.

Example:

```bash
sudo systemctl restart nginx
```

Instead of operating the entire session as root, a user can use `sudo` for authorized administrative commands.

---

## Why use sudo instead of root?

Using `sudo` supports the **Principle of Least Privilege**.

Instead of:

```text
Login as root
      ↓
Everything has maximum privilege
```

we can use:

```text
Normal user
      ↓
sudo specific command
      ↓
Elevated privilege
```

This improves control and accountability.

---

# ⚙️ sudo Configuration

Main configuration:

```text
/etc/sudoers
```

Edit or validate sudo configuration using:

```bash
sudo visudo
```

Additional policy files can be stored under:

```text
/etc/sudoers.d/
```

Avoid casually editing `/etc/sudoers` with a normal editor because a syntax error can cause administrative problems.

---

# 🔑 Authentication vs Authorization

### Authentication

Answers:

> **Who are you?**

Examples:

* Password
* SSH key

### Authorization

Answers:

> **What are you allowed to do?**

Examples:

* File permissions
* Group membership
* sudo policy

```text
Authentication
      ↓
Who are you?
      ↓
Authorization
      ↓
What can you access?
```

---

# 🧪 Hands-on Lab

## Step 1 — Create a user

```bash
sudo useradd -m -s /bin/bash clouduser
```

Set the password:

```bash
sudo passwd clouduser
```

Verify:

```bash
id clouduser
```

---

## Step 2 — Create groups

```bash
sudo groupadd developers
```

```bash
sudo groupadd cloudadmins
```

---

## Step 3 — Add the user to groups

```bash
sudo usermod -aG developers clouduser
```

```bash
sudo usermod -aG cloudadmins clouduser
```

Verify:

```bash
id clouduser
```

---

## Step 4 — Grant administrative access

On Ubuntu/Debian systems:

```bash
sudo usermod -aG sudo clouduser
```

Verify:

```bash
id clouduser
```

Switch to the user:

```bash
su - clouduser
```

Test sudo:

```bash
sudo whoami
```

Expected output:

```text
root
```

Exit:

```bash
exit
```

---

# 🔎 Troubleshooting Scenario

### Problem

A user was added to a group:

```bash
sudo usermod -aG developers clouduser
```

But the user does not see the new group when running:

```bash
groups
```

### Reason

The existing login session may not have refreshed its supplementary group membership.

### Solution

Log out and start a new login session.

Then verify:

```bash
id clouduser
```

---

# 🔧 Commands Practiced

```bash
whoami
id
groups
who
w
getent passwd
getent group
cat /etc/passwd
sudo cat /etc/shadow
cat /etc/group
useradd
passwd
usermod
userdel
groupadd
gpasswd
sudo
visudo
```

---

# 🔧 Git Practice

From Day 2 onward, Git is part of my daily learning workflow.

## Git workflow

```text
Learn
  ↓
Practice
  ↓
Document
  ↓
git status
  ↓
git diff
  ↓
git add
  ↓
git commit
  ↓
git push
  ↓
GitHub
```

### Commands practiced

Check repository status:

```bash
git status
```

View changes:

```bash
git diff
```

Stage the Day 2 README:

```bash
git add Day-02-Linux-Users-Groups-Sudo/README.md
```

Commit:

```bash
git commit -m "Day 02: Linux users groups and sudo"
```

Push to GitHub:

```bash
git push origin main
```

---

# 🎤 Interview Questions

### 1. What is UID?

A UID is the numeric identifier used by Linux to identify a user.

### 2. What is GID?

A GID is the numeric identifier assigned to a group.

### 3. What is the UID of root?

`0`.

### 4. What is `/etc/passwd`?

It contains basic Linux user account information such as username, UID, GID, home directory, and login shell.

### 5. What is `/etc/shadow`?

It stores password hashes and password-aging information and is normally protected from regular users.

### 6. What is the difference between a primary and supplementary group?

The primary group is the user's default group identity, while supplementary groups provide additional group memberships and associated access.

### 7. What is sudo?

`sudo` provides controlled privilege elevation according to configured authorization policies.

### 8. Why use sudo instead of root?

It allows administrators to grant controlled administrative privileges while following the principle of least privilege.

### 9. What does `usermod -aG` do?

It appends a user to one or more supplementary groups without removing their existing supplementary group memberships.

### 10. Where is sudo configured?

Primarily in `/etc/sudoers`, with additional policy files commonly stored in `/etc/sudoers.d/`.

### 11. What is the difference between authentication and authorization?

Authentication verifies identity; authorization determines what that identity is allowed to access or perform.

---

# 🧠 Key Takeaways

* Linux identifies users using UIDs.
* Groups simplify access management.
* Root has UID 0 and extensive administrative privileges.
* `/etc/passwd` contains basic user account information.
* `/etc/shadow` contains sensitive password-related information.
* `sudo` provides controlled privilege elevation.
* `usermod -aG` is commonly used to add supplementary group membership.
* Least privilege is an important security principle.
* Git is being used to document and version-control this learning journey.

---
---

## 📅 Progress

**Day 02 / 30 ✅**

### Next

**Day 03 — Linux File Permissions, ACL, SUID, SGID & Sticky Bit**
