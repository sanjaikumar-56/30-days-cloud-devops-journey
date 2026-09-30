# Day 08 — Linux Package Management

## 📌 Overview

Today I learned how Linux manages software packages using package managers.

I mainly practiced Ubuntu/Debian package management using APT and DPKG, along with basic awareness of DNF/YUM used in RHEL-based systems.

---

## 🎯 Topics Covered

* Linux packages
* Package managers
* APT
* DPKG
* Repositories
* Package dependencies
* Installing packages
* Updating package information
* Upgrading packages
* Removing packages
* Purging packages
* Package search and information
* Package verification
* Package troubleshooting
* Nginx installation and verification
* Basic DNF/YUM commands

---

## 1. What is a Package?

A Linux package is a collection of files required to install and manage software.

Common package formats:

```text
Debian/Ubuntu → .deb
RHEL-based    → .rpm
```

---

## 2. Package Managers

Ubuntu/Debian commonly uses:

```text
APT + DPKG
```

RHEL-based distributions commonly use:

```text
DNF + RPM
```

APT is a higher-level package management tool, while DPKG is the lower-level Debian package management system.

---

## 3. Update Package Information

```bash
sudo apt update
```

This refreshes the local package index from configured repositories.

It does not itself upgrade all installed packages.

---

## 4. Upgrade Packages

```bash
sudo apt upgrade
```

This installs available newer versions of installed packages where the upgrade can be performed.

Typical workflow:

```bash
sudo apt update
sudo apt upgrade
```

---

## 5. Search for Packages

```bash
apt search nginx
```

Get package information:

```bash
apt show nginx
```

Check available and installed versions:

```bash
apt policy nginx
```

---

## 6. Install a Package

Example:

```bash
sudo apt install nginx
```

Multiple packages can also be installed:

```bash
sudo apt install curl wget git
```

---

## 7. Verify Package Installation

```bash
nginx -v
```

```bash
dpkg -s nginx
```

List installed packages:

```bash
dpkg -l | grep nginx
```

---

## 8. Remove vs Purge

Remove the package:

```bash
sudo apt remove nginx
```

Purge the package and its associated configuration files:

```bash
sudo apt purge nginx
```

Remove unused dependencies:

```bash
sudo apt autoremove
```

---

## 9. Package Files

List files installed by a package:

```bash
dpkg -L nginx
```

Find which package owns a file:

```bash
dpkg -S /usr/bin/curl
```

---

## 10. Package Dependencies

View dependencies:

```bash
apt depends nginx
```

View reverse dependencies:

```bash
apt rdepends nginx
```

Package managers use dependency information to install software and its required supporting packages.

---

## 11. Nginx Hands-on

I installed/verified Nginx using:

```bash
sudo apt install nginx
```

Checked the service:

```bash
sudo systemctl status nginx
```

Checked port 80:

```bash
sudo ss -lntp | grep ':80'
```

Tested the local web server:

```bash
curl http://localhost
```

Tested Nginx configuration:

```bash
sudo nginx -t
```

Checked logs:

```bash
sudo journalctl -u nginx -n 30
```

---

## 12. Package Troubleshooting

Useful commands:

```bash
dpkg --audit
```

```bash
sudo apt --fix-broken install
```

Check disk space:

```bash
df -h
```

Check repository configuration:

```bash
cat /etc/apt/sources.list
```

and:

```bash
ls -la /etc/apt/sources.list.d/
```

---

## 13. RHEL-Based Package Management

For RHEL-based systems:

```bash
sudo dnf install nginx
```

Search:

```bash
dnf search nginx
```

Information:

```bash
dnf info nginx
```

Upgrade:

```bash
sudo dnf upgrade
```

Remove:

```bash
sudo dnf remove nginx
```

Older systems may use:

```bash
yum
```

---

## 🎤 Interview Questions

### 1. What is a package manager?

A package manager is a tool used to install, update, remove, and manage software packages and their dependencies.

### 2. What is the difference between APT and DPKG?

APT is a higher-level package management tool that handles repositories and dependencies, while DPKG is the lower-level Debian package management system.

### 3. What is the difference between apt update and apt upgrade?

`apt update` refreshes package information from configured repositories, while `apt upgrade` installs available newer versions of installed packages.

### 4. What is the difference between apt remove and apt purge?

`apt remove` removes the package while configuration files may remain. `apt purge` removes the package and its associated configuration files.

### 5. What is a package repository?

A package repository is a location containing software packages and metadata that a package manager can access.

### 6. What is a package dependency?

A dependency is another package or component required by software for it to function correctly.

### 7. What would you check if package installation fails?

I would check network connectivity, DNS, repository configuration, package metadata, disk space, dependency errors, and the package manager output/logs.

### 8. Nginx is installed but the website isn't working. What would you check?

I would check the Nginx service, listening ports, configuration syntax, local HTTP response, logs, and then firewall or cloud security rules if applicable.

---

## ⭐ Key Takeaways

```text
APT
 ↓
Repository
 ↓
Package
 ↓
Dependencies
 ↓
Installation
 ↓
Service
 ↓
Port
 ↓
Application
```

Today I connected package management with concepts I learned on previous days:

```text
Day 5 → systemd
Day 6 → Networking
Day 7 → SSH
Day 8 → Package Management
```

**Day 8 completed ✅**

