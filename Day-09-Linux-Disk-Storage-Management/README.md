# Day 09 — Linux Disk & Storage Management

## 📌 Overview

Today I learned how Linux manages disks, partitions, filesystems, mount points, and storage usage.

I also practiced troubleshooting disk-space issues and learned how Linux storage concepts connect with AWS EBS volumes.

---

## 🎯 Topics Covered

- Linux disks and block devices
- Partitions
- Filesystems
- Mount points
- `lsblk`
- `df`
- `du`
- Inodes
- `findmnt`
- `mount` and `umount`
- `blkid`
- UUID
- `/etc/fstab`
- `fdisk`
- Large-file troubleshooting
- Deleted files still consuming disk space
- Basic LVM concepts
- AWS EBS storage concepts

---

## 1. Linux Storage Architecture

A basic Linux storage structure is:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount Point
 ↓
Directory
 ↓
Application
```

Example:

```text
/dev/sdb
   ↓
/dev/sdb1
   ↓
ext4
   ↓
/data
```

---

## 2. Block Devices

Linux represents storage devices as block devices.

Examples:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
```

They are generally available under:

```bash
/dev/
```

---

## 3. Check Disks Using lsblk

```bash
lsblk
```

For filesystem information:

```bash
lsblk -f
```

`lsblk` displays block devices and their relationships in a tree structure.

The `-f` option also displays filesystem information.

---

## 4. Check Disk Usage with df

```bash
df -h
```

The `-h` option displays values in human-readable units.

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
```

Useful information includes:

- Total size
- Used space
- Available space
- Usage percentage
- Mount point

Check a specific filesystem:

```bash
df -h /
```

---

## 5. Filesystem Type

```bash
df -Th
```

This displays filesystem type along with disk usage.

Common Linux filesystems include:

```text
ext4
xfs
btrfs
```

---

## 6. df vs du

### df

```bash
df -h
```

Shows filesystem-level disk usage.

### du

```bash
du -sh /var
```

Shows disk usage associated with files and directories.

### Key Difference

```text
df → Filesystem usage
du → File/directory usage
```

---

## 7. Find Large Directories

To inspect root-level directories:

```bash
sudo du -xhd1 / | sort -h
```

For `/var`:

```bash
sudo du -sh /var/*
```

This helps identify which directories are consuming significant disk space.

---

## 8. Find Large Files

Example:

```bash
sudo find /var -type f -size +100M -exec ls -lh {} \;
```

This searches `/var` for files larger than 100 MB.

---

## 9. Inodes

Linux filesystems use inodes to store metadata about files and directories.

An inode contains information such as:

- Ownership
- Permissions
- File type
- Timestamps
- File size
- References to file data

Check inode usage:

```bash
df -ih
```

---

## 10. Disk Space vs Inode Exhaustion

A filesystem can have available disk space but still be unable to create new files if its inodes are exhausted.

Check disk space:

```bash
df -h
```

Check inode usage:

```bash
df -ih
```

Important distinction:

```text
df -h  → Disk capacity
df -i  → Inode capacity
```

---

## 11. Check Mounted Filesystems

```bash
findmnt
```

Check the root filesystem:

```bash
findmnt /
```

The `mount` command can also display mounted filesystems:

```bash
mount
```

---

## 12. Mounting a Filesystem

A filesystem can be attached to a directory called a mount point.

Conceptually:

```text
/dev/sdb1
    ↓
ext4 filesystem
    ↓
/data
```

Create a mount point:

```bash
sudo mkdir /data
```

Mount a filesystem:

```bash
sudo mount /dev/sdb1 /data
```

Verify:

```bash
df -h
```

or:

```bash
findmnt /data
```

> Mounting actual block devices should only be performed after confirming the device and filesystem. Incorrect commands can cause data loss.

---

## 13. Unmounting

Unmount:

```bash
sudo umount /data
```

If the filesystem is busy, identify processes using it:

```bash
sudo lsof +D /data
```

or:

```bash
sudo fuser -vm /data
```

Also make sure the current shell is not inside the mount point.

---

## 14. Filesystem UUID

Check filesystem UUIDs:

```bash
sudo blkid
```

Example:

```text
/dev/sdb1: UUID="xxxx-xxxx" TYPE="ext4"
```

UUIDs provide a persistent identifier for filesystems.

---

## 15. /etc/fstab

Linux stores persistent filesystem mount configuration in:

```text
/etc/fstab
```

View it:

```bash
cat /etc/fstab
```

A conceptual entry looks like:

```text
UUID=xxxx-xxxx   /data   ext4   defaults   0   2
```

This allows Linux to know which filesystem should be mounted at which mount point.

---

## 16. Validating fstab

After carefully modifying `/etc/fstab`, it is important to validate the configuration.

```bash
sudo mount -a
```

This attempts to mount filesystems configured in `fstab` that are not currently mounted.

Incorrect `fstab` entries can cause boot problems, so configuration changes should be made carefully.

---

## 17. Partition Information

Inspect disk and partition information:

```bash
sudo fdisk -l
```

For everyday inspection, `lsblk` is often a safer first command.

I used `fdisk` only for inspection and did not modify partitions.

---

## 18. Deleted Files Still Consuming Space

Sometimes `df -h` shows significant disk usage while `du` doesn't appear to account for it.

One possible reason is an open file that has already been deleted.

Check:

```bash
sudo lsof +L1
```

A process can continue holding the file open until it closes the file descriptor.

---

## 19. LVM Basics

LVM stands for:

**Logical Volume Manager**

Basic architecture:

```text
Physical Disk
      ↓
Physical Volume (PV)
      ↓
Volume Group (VG)
      ↓
Logical Volume (LV)
      ↓
Filesystem
      ↓
Mount Point
```

Useful commands:

```bash
sudo pvs
sudo vgs
sudo lvs
```

If the system does not use LVM, these commands may return no configured volumes.

---

## 20. AWS EBS Connection

Linux storage concepts are directly relevant to AWS EC2.

A simplified architecture is:

```text
AWS EC2
   ↓
EBS Volume
   ↓
Linux Block Device
   ↓
Partition
   ↓
Filesystem
   ↓
Mount Point
```

For example:

```text
EBS Volume
    ↓
/dev/nvme1n1
    ↓
Partition
    ↓
ext4 / XFS
    ↓
/data
```

Exact device naming depends on the EC2 instance and operating system.

---

## 21. Increasing EC2 Storage

A high-level process for increasing an EBS-backed filesystem is:

```text
Increase EBS volume size
        ↓
Verify Linux detects new capacity
        ↓
Expand partition if required
        ↓
Expand filesystem
        ↓
Verify with df -h
```

Useful commands/tools can include:

```bash
lsblk
```

For ext4:

```bash
resize2fs
```

For XFS:

```bash
xfs_growfs
```

The exact commands depend on the disk layout and filesystem.

---

# 🧪 Hands-on Commands Practiced

### Block devices

```bash
lsblk
lsblk -f
```

### Disk usage

```bash
df -h
df -Th
df -ih
```

### Directory usage

```bash
sudo du -xhd1 / | sort -h
sudo du -sh /var/*
```

### Large files

```bash
sudo find /var -type f -size +100M -exec ls -lh {} \;
```

### Mount information

```bash
findmnt
findmnt /
```

### UUID

```bash
sudo blkid
```

### Filesystem configuration

```bash
cat /etc/fstab
```

### Partition inspection

```bash
sudo fdisk -l
```

### Deleted open files

```bash
sudo lsof +L1
```

### LVM

```bash
sudo pvs
sudo vgs
sudo lvs
```

---

# 🎤 Interview Questions

## 1. What is the difference between df and du?

`df` reports filesystem-level disk usage, while `du` reports disk usage associated with files and directories.

## 2. How do you check disk usage?

```bash
df -h
```

## 3. How do you find which directory is consuming the most space?

```bash
sudo du -xhd1 / | sort -h
```

Then investigate the largest directory.

## 4. How do you check inode usage?

```bash
df -ih
```

## 5. What is a mount point?

A mount point is a directory through which a filesystem becomes accessible in the Linux directory tree.

## 6. What is /etc/fstab?

`/etc/fstab` contains persistent filesystem mount configuration.

## 7. What does blkid do?

`blkid` displays block-device attributes such as filesystem type and UUID.

## 8. Why can df show high usage while du doesn't?

One possible reason is an open deleted file. A process may still hold the file descriptor, keeping its disk blocks allocated.

Check:

```bash
sudo lsof +L1
```

## 9. What is LVM?

LVM is a storage management system that provides an abstraction layer between physical storage and logical volumes.

## 10. How would you increase an EC2 disk?

Increase the EBS volume size, verify the new size in Linux, expand the partition if necessary, expand the filesystem using the appropriate tool, and verify with `df -h`.

---

# ⭐ Key Takeaways

```text
lsblk  → Disks & partitions
df     → Filesystem usage
du     → Directory/file usage
df -i  → Inode usage
blkid  → UUID & filesystem information
findmnt → Mounted filesystems
mount  → Mount filesystem
umount → Unmount filesystem
fstab  → Persistent mount configuration
pvs    → Physical volumes
vgs    → Volume groups
lvs    → Logical volumes
```

## 🔗 Day 9 Learning Chain

```text
Day 5 → Services & systemd
Day 6 → Networking
Day 7 → SSH
Day 8 → Package Management
Day 9 → Disk & Storage Management
```

**Day 9 completed ✅**
