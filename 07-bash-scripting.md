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
# Administrator's Notes

This section explains **why** Bash scripting is an essential Linux administration skill, **how** the script works internally, where similar automation is used in enterprise environments, and the advantages and limitations of this approach. It is intended to reinforce both scripting fundamentals and real-world system administration concepts.

---

# Administrator's Explanation

## What Was Accomplished?

If someone asks:

> **"What did you accomplish in this lesson?"**

A professional answer would be:

> I automated routine Linux system administration tasks by creating a Bash script that collects disk usage and memory utilization information and stores the results in a centralized log file. Instead of manually checking system resources each time, the script performs these tasks automatically, providing consistent and repeatable monitoring that can later be scheduled or integrated into larger monitoring systems.

---

# Why Use Bash Scripting?

A senior administrator may ask:

> **"Why write a Bash script instead of typing the commands manually?"**

A strong answer would be:

> Bash scripting allows repetitive administrative tasks to be automated, reducing human error and improving efficiency. Instead of manually executing several commands every time system information is needed, the script performs them in a consistent sequence and produces standardized output. Automation also makes it possible to schedule these tasks using cron or systemd timers.

---

# How Bash Automation Works

Without automation:

```text
Administrator

↓

Run df

↓

Run free

↓

Run date

↓

Format Output

↓

Save Results
```

Every execution requires manual effort.

With Bash scripting:

```text
Administrator

↓

Run Script

↓

Script Executes Commands

↓

Collects Results

↓

Creates Log File
```

One command performs the entire process automatically.

---

# Why Use a Bash Script?

Bash is the default command-line shell on most Linux distributions and provides direct access to system utilities.

A Bash script allows administrators to:

* Automate repetitive tasks
* Combine multiple commands into one workflow
* Reduce manual mistakes
* Standardize administrative procedures
* Schedule recurring jobs
* Improve operational efficiency

Because Bash is included with Linux by default, scripts are portable across most Linux systems without requiring additional software.

---

# Understanding the Script Structure

Every Bash script begins with:

```bash
#!/bin/bash
```

This line is called the **shebang**.

It tells Linux which interpreter should execute the script.

The overall execution flow looks like this:

```text
Script Starts

↓

Create Log File Reference

↓

Write Date and Time

↓

Collect Disk Information

↓

Collect Memory Information

↓

Append Results

↓

Display Completion Message
```

Each command performs a specific task while the script coordinates the entire workflow.

---

# Why Use Variables?

The script defines:

```bash
OUTPUT_LOG="server_health.log"
```

Instead of writing the filename repeatedly throughout the script.

Benefits include:

* Easier maintenance
* Improved readability
* Reduced typing errors
* Faster modification if the output filename changes

Changing one variable automatically updates every reference within the script.

---

# Why Use Output Redirection (`>>`)?

The script uses:

```bash
>>
```

instead of:

```bash
>
```

Difference:

```text
>

Creates a new file or overwrites existing contents.
```

```text
>>

Appends new information to the end of the existing file.
```

Appending allows administrators to maintain a historical record of multiple health checks instead of replacing previous results.

---

# Why Use Command Substitution?

The following syntax was used:

```bash
$(date)
```

Command substitution executes a command and inserts its output into another command.

Example:

```text
SCAN TIME:
Thu Jun 25 11:51:56 AM PKT 2026
```

Without command substitution, the administrator would need to manually enter timestamps.

---

# Why Use `df`?

The command:

```bash
df -h /
```

reports disk usage information for the root filesystem.

It provides:

* Total storage capacity
* Used storage
* Available storage
* Percentage utilization

Administrators monitor this information to prevent storage exhaustion that could interrupt services.

---

# Why Use `free`?

The command:

```bash
free -h
```

reports memory usage.

It provides:

* Total RAM
* Used RAM
* Free RAM
* Cached memory
* Available memory

Monitoring memory utilization helps administrators detect resource shortages and potential performance problems.

---

# Why Use `awk`?

Commands such as `df` and `free` generate large amounts of information.

The script uses `awk` to extract only the relevant values.

Without `awk`:

```text
Complete command output
```

With `awk`:

```text
Available Space: 98G

Use Percentage: 12%

Total RAM: 3.5Gi

Used RAM: 2.7Gi
```

Filtering unnecessary information produces cleaner reports that are easier to read.

---

# Why Make the Script Executable?

Linux treats scripts as ordinary text files until execute permission is granted.

The command:

```bash
chmod +x health_check.sh
```

adds execute permission.

Before:

```text
Text File
```

After:

```text
Executable Program
```

The operating system can now execute the script directly.

---

# Why Save Results to a Log File?

Instead of displaying information only on the terminal, the script stores results inside:

```text
server_health.log
```

Benefits include:

* Historical record keeping
* Easier troubleshooting
* System auditing
* Performance comparisons
* Centralized monitoring data

Persistent logs are significantly more useful than temporary terminal output.

---

# Enterprise Usage

Automation is one of the core responsibilities of Linux system administrators.

Organizations automate tasks such as:

* System health monitoring
* Backup operations
* Security checks
* User account management
* Log rotation
* Software updates
* Service monitoring
* Resource reporting
* Scheduled maintenance

Rather than manually performing hundreds of repetitive tasks, administrators rely on scripts to execute them consistently.

---

# How Enterprise Automation Works

Small lab automation:

```text
Administrator

↓

Run Script

↓

Generate Report
```

Enterprise automation:

```text
Cron

↓

Monitoring Script

↓

Collect System Information

↓

Store Logs

↓

Monitoring Platform

↓

Administrator Dashboard
```

Many organizations schedule scripts to run automatically every few minutes or hours using cron jobs or systemd timers.

---

# Advantages of This Setup

* Eliminates repetitive manual work
* Produces consistent output
* Reduces human error
* Easy to modify and extend
* Lightweight and fast
* Built entirely with standard Linux tools
* Can be scheduled for automatic execution
* Creates historical system health logs

---

# Limitations

Every automation solution has trade-offs.

This script:

* Collects only basic system metrics
* Does not send alerts
* Does not monitor CPU utilization
* Does not monitor running services
* Does not detect hardware failures
* Requires manual execution unless scheduled

Enterprise monitoring platforms typically include alerting, dashboards, centralized log collection, performance analytics, and automatic notifications.

---

# Lab Environment vs Enterprise Environment

| Feature       | Lab Environment    | Enterprise Environment                       |
| ------------- | ------------------ | -------------------------------------------- |
| Monitoring    | Manual Bash script | Automated monitoring platforms               |
| Execution     | Manual             | Scheduled automatically                      |
| Metrics       | Disk and memory    | CPU, RAM, disks, network, services, hardware |
| Storage       | Local log file     | Centralized log servers                      |
| Notifications | None               | Email, SMS, dashboards, incident systems     |
| Scale         | Single machine     | Hundreds or thousands of servers             |

---

# Possible Interview Questions

## Why automate system administration tasks?

Automation reduces repetitive work, improves consistency, minimizes human error, and allows administrators to manage many systems efficiently.

---

## Why use Bash instead of another programming language?

Bash is included by default on Linux systems, integrates directly with Linux commands, requires no additional software, and is ideal for system administration tasks.

---

## Why use `>>` instead of `>`?

The `>>` operator appends information to an existing log file instead of overwriting previous data, allowing historical records to be maintained.

---

## Why use `awk`?

`awk` extracts and formats only the required information from command output, making reports cleaner and easier to understand.

---

## Why make the script executable?

Linux requires execute permission before a script can be run directly as a program.

---

## Why log the results instead of displaying them?

Log files provide permanent records that support troubleshooting, auditing, historical analysis, and automated monitoring.

---

# Administrator's Summary

If a senior administrator asks:

> **"Why did you build it this way?"**

A professional answer would be:

> I created a Bash script to automate routine system health checks because automation is a fundamental responsibility of Linux system administration. The script combines several standard Linux utilities to collect disk and memory information, formats the results into a readable report, and stores them in a centralized log file. Using variables, command substitution, output redirection, and text processing makes the script reusable, maintainable, and easy to extend. Although this example performs basic monitoring, the same scripting principles are widely used in enterprise environments to automate backups, system monitoring, software deployment, scheduled maintenance, and infrastructure management.

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
