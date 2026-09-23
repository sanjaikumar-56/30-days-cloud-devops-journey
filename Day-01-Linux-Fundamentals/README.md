# Day 01 — Linux Fundamentals 🐧

## 📌 Overview

Today I started my 30-day Cloud & DevOps learning journey by building a strong foundation in Linux.

Linux is an important technology for Cloud Engineering, System Administration, Networking, and DevOps.

---

## 📚 Topics Covered

### 1. Operating System

An operating system manages computer hardware and provides services required by applications.

Examples:

* Windows
* Linux
* macOS

### 2. Linux

Linux is an open-source, Unix-like operating system kernel. In common usage, Linux also refers to complete distributions built around the Linux kernel.

### 3. Linux Kernel

The kernel is the core component responsible for managing:

* CPU
* Memory
* Processes
* Storage
* Devices
* Networking

### 4. Linux Distribution

A Linux distribution combines the Linux kernel with system utilities, libraries, package management tools, and other software.

Examples:

* Ubuntu
* Debian
* RHEL
* Rocky Linux
* Amazon Linux

### 5. Shell

A shell is a command interpreter that allows users to interact with the operating system through commands.

Example:

```bash
bash
```

### 6. Bash

Bash stands for **Bourne Again Shell** and is one of the most commonly used shells in Linux.

---

## 🏗️ Linux Architecture

```text
Applications
     ↓
Shell
     ↓
System Libraries / System Calls
     ↓
Linux Kernel
     ↓
Hardware
```

---

## 📁 Linux Filesystem

Important directories:

| Directory | Purpose                           |
| --------- | --------------------------------- |
| `/`       | Root of the filesystem            |
| `/home`   | Normal users' home directories    |
| `/root`   | Root user's home directory        |
| `/etc`    | System configuration files        |
| `/var`    | Variable data and logs            |
| `/tmp`    | Temporary files                   |
| `/usr`    | User-space programs and libraries |
| `/dev`    | Device files                      |
| `/proc`   | Process and kernel information    |
| `/boot`   | Boot-related files                |

---

## 💻 Commands Practiced

### Check current directory

```bash
pwd
```

### List files

```bash
ls
```

### List hidden files

```bash
ls -la
```

### Change directory

```bash
cd /etc
```

### Go to home directory

```bash
cd ~
```

### Go to parent directory

```bash
cd ..
```

### Create directory

```bash
mkdir cloud
```

### Create nested directories

```bash
mkdir -p cloud/aws/ec2
```

### Create an empty file

```bash
touch notes.txt
```

### Copy a file

```bash
cp notes.txt backup.txt
```

### Move / rename a file

```bash
mv backup.txt aws-notes.txt
```

### Remove a file

```bash
rm aws-notes.txt
```

---

## 🧪 Hands-on Practice

Created a practice directory and worked with files and directories.

```text
day1/
├── cloud/
│   └── aws/
│       └── ec2/
│           └── notes.txt
└── devops/
```

Practiced:

* Creating directories
* Creating files
* Navigating directories
* Copying files
* Renaming files
* Removing files
* Using absolute and relative paths

---

## 🎤 Interview Questions

### What is Linux?

Linux is an open-source, Unix-like kernel. The term Linux is also commonly used for complete operating systems built around the Linux kernel.

### What is a kernel?

The kernel is the core component of an operating system that manages system resources and provides an interface between applications and hardware.

### What is a Linux distribution?

A Linux distribution is a complete operating system built around the Linux kernel with utilities, libraries, package management tools, and applications.

### What is a shell?

A shell is a command interpreter that provides an interface for users to interact with the operating system.

### What is the difference between `/` and `/root`?

`/` is the root of the entire Linux filesystem, while `/root` is the home directory of the root user.

### What is the difference between an absolute and relative path?

An absolute path starts from the root directory, while a relative path is interpreted from the current working directory.

---

## 💡 Key Takeaways

1. The Linux kernel manages system resources and hardware interaction.
2. A Linux distribution provides a complete usable operating system around the kernel.
3. The shell provides a command-line interface to interact with the system.
4. Linux uses a hierarchical filesystem starting from `/`.
5. Understanding basic Linux commands is essential for Cloud and DevOps roles.

---

## 🔗 Related

**LinkedIn:** [Add Day 01 LinkedIn post after publishing]

---

## 📅 Progress

**Day 01 / 30 ✅**

Next: **Linux File Management**

