# Lesson 7: Automation via Bash Scripting

## Objective

To automate routine system administration tasks by creating a Bash script that collects disk and memory usage information and writes the results to a centralized log file.

---

## Environment

| Item             | Value             |
| ---------------- | ----------------- |
| Operating System | Ubuntu Linux      |
| Shell            | Bash              |
| Script File      | health_check.sh   |
| Output Log       | server_health.log |

---

## Step 1: Creating the Health Monitoring Script

A Bash script was created to automate collection of system health information.

### Command Executed

```bash
nano health_check.sh
```

### Script Source Code

```bash
#!/bin/bash

OUTPUT_LOG="server_health.log"

echo "==========================================" >> $OUTPUT_LOG
echo "SCAN TIME: $(date)" >> $OUTPUT_LOG
echo "==========================================" >> $OUTPUT_LOG

echo "--- DISK SPACE ALERT STATUS ---" >> $OUTPUT_LOG
df -h / | awk 'NR==2 {print "Available Space: " $4 " | Use Percentage: " $5}' >> $OUTPUT_LOG

echo "--- MEMORY CONSUMPTION STATUS ---" >> $OUTPUT_LOG
free -h | awk 'NR==2 {print "Total RAM: " $2 " | Used RAM: " $3 " | Free RAM: " $4}' >> $OUTPUT_LOG

echo "Scan Complete. Data written to $OUTPUT_LOG"
```

---

## Step 2: Making the Script Executable

New script files do not have execute permissions by default.

### Command Executed

```bash
chmod +x health_check.sh
```

### Verification

The script was successfully granted execution permissions and became runnable as a program.

---

## Step 3: Executing the Script

### Command Executed

```bash
./health_check.sh
```

### Output

```text
Scan Complete. Data written to server_health.log
```

---

## Step 4: Reviewing the Generated Report

### Command Executed

```bash
cat server_health.log
```

### Output Captured

```text
==========================================
SCAN TIME: Thu Jun 25 11:51:56 AM PKT 2026
==========================================
--- DISK SPACE ALERT STATUS ---
Available Space: 98G | Use Percentage: 12%
--- MEMORY CONSUMPTION STATUS ---
Total RAM: 3.5Gi | Used RAM: 2.7Gi | Free RAM: 283Mi
```

---

## Output Analysis

### Storage Status

* Root filesystem available space: **98 GB**
* Current utilization: **12%**
* No storage pressure detected.

### Memory Status

* Total RAM: **3.5 GiB**
* Used RAM: **2.7 GiB**
* Free RAM: **283 MiB**

The script successfully collected and formatted system resource information into a reusable log file.

---

## Skills Demonstrated

* Bash scripting fundamentals
* File output redirection (`>>`)
* Variable creation and usage
* Command substitution (`$(date)`)
* Text processing with `awk`
* Permission management using `chmod`
* Basic system monitoring automation

---

## Repository Completion Summary

This repository documents practical Linux administration tasks including:

1. File system navigation and management
2. User and group administration
3. File permissions and ownership
4. Package management with APT
5. Networking and SSH deployment
6. Service management using systemd
7. Bash scripting and task automation

---
