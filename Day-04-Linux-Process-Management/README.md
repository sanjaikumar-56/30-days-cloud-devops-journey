# Day 04 — Linux Process Management

## 📌 Topics Covered

Today I learned and practiced Linux process management and troubleshooting.

### 1. Process Fundamentals

* What is a process?
* Program vs Process
* PID — Process ID
* PPID — Parent Process ID
* PID 1
* Parent-child process relationships
* Process tree

### 2. Process Monitoring

Practiced:

```bash
ps
ps -f
ps aux
ps -ef
pstree -p
top
htop
```

Used these commands to inspect running processes, CPU usage, memory usage, process IDs, parent processes, and process states.

### 3. Finding Processes

Practiced:

```bash
pgrep -a process_name
pidof process_name
ps -p PID -f
```

Example:

```bash
pgrep -a python
```

### 4. CPU and Memory Troubleshooting

To identify high CPU-consuming processes:

```bash
ps aux --sort=-%cpu | head -10
```

To identify high memory-consuming processes:

```bash
ps aux --sort=-%mem | head -10
```

Also practiced real-time monitoring using:

```bash
top
htop
```

### 5. Process States

Learned common process states:

| State | Meaning               |
| ----- | --------------------- |
| R     | Running/Runnable      |
| S     | Sleeping              |
| D     | Uninterruptible sleep |
| T     | Stopped               |
| Z     | Zombie                |
| I     | Idle kernel thread    |

### 6. Signals and Process Termination

Learned:

```bash
kill PID
kill -15 PID
kill -9 PID
```

#### SIGTERM — 15

Requests graceful termination and allows the application to perform cleanup.

#### SIGKILL — 9

Forces termination and cannot be caught or handled by the target process.

Preferred approach:

```text
SIGTERM
   ↓
Check process
   ↓
If still running and justified
   ↓
SIGKILL
```

### 7. Zombie and Orphan Processes

#### Zombie

A process that has terminated but whose parent has not yet collected its exit status.

Check:

```bash
ps -eo pid,ppid,stat,cmd | grep ' Z'
```

#### Orphan

A running child process whose original parent has terminated and which is subsequently re-parented.

### 8. Foreground and Background Processes

Practiced:

```bash
command &
jobs
bg
fg
```

Also practiced:

```bash
nohup command &
```

### 9. Process Priority

Learned:

```bash
nice
renice
```

Example:

```bash
nice -n 10 sleep 300 &
```

### 10. `/proc` Filesystem

Explored process information through:

```bash
/proc/PID/
```

Practiced:

```bash
cat /proc/PID/status
readlink -f /proc/PID/exe
readlink -f /proc/PID/cwd
ls -l /proc/PID/fd
```

### 11. Finding Processes Using Ports

Practiced:

```bash
sudo ss -lntp
```

For a specific port:

```bash
sudo ss -lntp | grep ':80'
```

Also:

```bash
sudo lsof -i :80
```

### 12. Practical Troubleshooting Flow

For a server experiencing high CPU:

```text
Check system
    ↓
top / htop
    ↓
Identify process
    ↓
Find PID
    ↓
Inspect process
    ↓
Check parent process
    ↓
Investigate logs/application
    ↓
Take appropriate action
```

## 🧪 Hands-on Practice

I practiced:

* Listing running processes
* Finding processes by name
* Monitoring CPU and memory
* Creating background processes
* Moving processes between foreground/background
* Sending signals
* Understanding process states
* Investigating `/proc`
* Finding processes listening on ports
* Testing `nice` and `renice`
* Using `nohup`

## 🎯 Interview Questions Practiced

1. What is a process?
2. What is PID?
3. What is PPID?
4. What is PID 1?
5. `ps aux` vs `ps -ef`
6. How do you find a process by name?
7. How do you find the process consuming high CPU?
8. How do you find the process consuming high memory?
9. What is a zombie process?
10. What is an orphan process?
11. SIGTERM vs SIGKILL
12. How do you find which process is using port 80?
13. What is `/proc`?
14. What are `nice` and `renice`?
15. What is the difference between foreground and background processes?

## 📸 Practical Evidence

Screenshots from the hands-on lab are available in the `screenshots/` directory.

## 📚 Key Takeaway

Linux process management is essential for system administration, cloud operations, and DevOps.

The main goal is not only to find a process, but to understand:

```text
What is running?
        ↓
Who started it?
        ↓
What resources is it using?
        ↓
What state is it in?
        ↓
Which port/resource is it using?
        ↓
What is the correct troubleshooting action?
```

---

**Day 4 completed ✅**

#Linux #LinuxAdministration #CloudComputing #DevOps #AWS #CloudEngineer #LinuxCommands #SystemAdministration #100DaysOfDevOps

