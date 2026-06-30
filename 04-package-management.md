# Lesson 4: Package Management & System Maintenance (Linux SysAdmin Lab)

## Objective

To practice Linux package management using `apt`, system cleanup using `autoremove`, and to document real-world troubleshooting scenarios involving Git, file permissions, and SSH configuration.

---

## Environment

| Item            | Value        |
| --------------- | ------------ |
| OS              | Ubuntu Linux |
| User            | otaku        |
| Shell           | Bash         |
| Package Manager | APT          |

---

## Step 1: Installing and Searching Packages

The `apt` tool was used to search and install system utilities.

### Command Executed

```bash id="pkg1"
apt search htop
```

### Result

The package `htop` was found in the official Ubuntu repositories.

### Package Details

```text id="pkg2"
htop - interactive processes viewer
```

---

### Installation

```bash id="pkg3"
sudo apt install htop -y
```

### Outcome

* `htop` installed successfully
* Dependencies automatically resolved by APT

---

## Step 2: System Cleanup Using Autoremove

Unused packages were removed to free system space.

### Command Executed

```bash id="pkg4"
sudo apt autoremove -y
```

### Result

* Approximately **140 MB** of disk space was freed
* Multiple unused libraries were safely removed

### Explanation

`autoremove` removes packages that were installed as dependencies but are no longer required by the system.

---

## Step 3: Git Error – Permission Issue

During repository update, Git encountered a file permission conflict.

### Error

```text id="git1"
error: open("server_auth.log"): Permission denied
fatal: unable to index file
```

### Cause

The file `server_auth.log` had restricted permissions (`640`), which prevented Git from reading it under the current user context.

---

## Step 4: Git Command Mistake

A syntax error occurred due to an incorrect command:

```text id="git2"
git: 'push.' is not a git command
```

### Cause

A trailing dot (`.`) was mistakenly added to the `git push` command.

---

## Step 5: SSH Verification Issue

SSH service was checked for remote access setup.

### Command Executed

```bash id="ssh1"
sudo systemctl status ssh
```

### Result

```text id="ssh2"
Unit ssh.service could not be found.
```

### Explanation

The OpenSSH server was not installed by default and needs manual installation using:

```bash
sudo apt install openssh-server -y
```

---

## Step 6: Networking Verification

Network connectivity was tested using ICMP packets.

### Command Executed

```bash id="net1"
ping -c 3 8.8.8.8
```

### Result

* 0% packet loss
* Stable internet connectivity confirmed

---

## Step 7: Git Workflow Fix & Recovery

After resolving permission and syntax issues, Git operations were successfully executed.

### Commands Used

```bash id="git3"
git add .
git commit -m "Added Lesson 4 Package Management"
git push
```

### Outcome

* Changes successfully committed
* Repository updated on GitHub

---

## Key Learning Points

* Searching and installing packages using `apt`
* System cleanup with `autoremove`
* Understanding Linux file permission impact on Git
* Fixing Git command syntax errors
* Diagnosing missing SSH server issues
* Validating internet connectivity using `ping`

---

## Skills Demonstrated

* Linux package management
* System maintenance and cleanup
* Git troubleshooting
* File permission awareness
* Network diagnostics
* SSH service understanding

---

## Commands Used

```bash id="cmd1"
apt search htop
sudo apt install htop -y
sudo apt autoremove -y
ping -c 3 8.8.8.8
sudo systemctl status ssh
git add .
git commit -m "Added Lesson 4 Package Management"
git push
```
# Administrator's Notes

This section explains **why** package management is essential in Linux administration, **how** APT manages software and dependencies, why system maintenance is important, and how Git, file permissions, networking, and SSH troubleshooting fit into everyday system administration.

---

# Administrator's Explanation

## What Was Accomplished?

If someone asks:

> **"What did you accomplish in this lesson?"**

A professional answer would be:

> I practiced Linux package management using the APT package manager by searching for, installing, and maintaining software packages. I also performed system cleanup using `autoremove`, diagnosed Git permission and syntax issues, verified network connectivity, and investigated SSH service availability. These tasks represent common maintenance and troubleshooting responsibilities performed by Linux system administrators.

---

# Why Learn Package Management?

A senior administrator may ask:

> **"Why is package management important?"**

A strong answer would be:

> Linux systems rely on package managers to install, update, verify, and remove software in a secure and organized manner. Rather than downloading applications manually, package managers retrieve software from trusted repositories, automatically resolve dependencies, and simplify long-term system maintenance.

---

# What Is APT?

APT (Advanced Package Tool) is Ubuntu's package management system.

The installation process works like this:

```text
Administrator
      │
      ▼
APT
      │
      ▼
Ubuntu Repository
      │
      ▼
Download Package
      │
      ▼
Install Dependencies
      │
      ▼
Install Application
```

APT automatically downloads required software, verifies package information, installs dependencies, and configures applications for use.

---

# Why Search Before Installing?

The following command was used:

```bash
apt search htop
```

Searching allows administrators to:

* Verify that the package exists.
* View available software.
* Confirm the correct package name.
* Avoid installing the wrong application.

Searching repositories before installation is considered good administrative practice.

---

# Why Install `htop`?

`htop` is an interactive process monitoring tool used by Linux administrators.

Compared to the traditional `top` command, `htop` provides:

* Interactive process management
* Colorized output
* CPU utilization monitoring
* Memory usage monitoring
* Process searching
* Easy process termination

It is commonly installed on Linux servers for real-time performance monitoring.

---

# How Dependency Management Works

When executing:

```bash
sudo apt install htop -y
```

APT performs much more than simply installing one package.

The workflow looks like this:

```text
Install Request

↓

Locate Package

↓

Check Dependencies

↓

Download Required Packages

↓

Install Everything

↓

Configure Software
```

Dependency resolution is one of APT's most valuable features, ensuring that applications have everything they need to function correctly.

---

# Why Use `autoremove`?

The following command was executed:

```bash
sudo apt autoremove -y
```

During software installation, additional dependency packages are often installed automatically.

Later, when the original software is removed, many of those dependencies are no longer required.

`autoremove` safely removes these unused packages.

Example:

```text
Application

↓

Dependency A

Dependency B

Dependency C

↓

Application Removed

↓

Dependencies No Longer Needed

↓

autoremove Removes Them
```

This helps maintain a clean and efficient operating system.

---

# Why Perform System Maintenance?

Regular maintenance provides several benefits:

* Frees disk space
* Removes obsolete packages
* Reduces software clutter
* Improves maintainability
* Lowers the attack surface by removing unused software

Routine maintenance is a standard responsibility for Linux administrators.

---

# Why Did Git Show a Permission Error?

Git displayed:

```text
error: open("server_auth.log"): Permission denied
```

This occurred because Linux file permissions prevented Git from reading the file.

Linux controls access through ownership and permission bits.

The process looks like this:

```text
Git

↓

Open File

↓

Linux Permission Check

↓

Access Denied
```

If the current user lacks sufficient permissions, Git cannot include the file in the repository.

---

# Why Are File Permissions Important?

Linux protects files by controlling who can:

* Read
* Write
* Execute

Proper permissions help:

* Prevent unauthorized access
* Protect sensitive files
* Maintain system security
* Reduce accidental modifications

Understanding permissions is essential for every Linux administrator.

---

# Why Did the Git Command Fail?

The following error occurred:

```text
git: 'push.' is not a git command
```

This was caused by a simple syntax mistake.

The incorrect command:

```text
git push.
```

was interpreted differently because of the extra period.

Correct command:

```bash
git push
```

Even small typing mistakes can prevent terminal commands from executing correctly.

---

# Why Verify the SSH Service?

The command:

```bash
sudo systemctl status ssh
```

returned:

```text
Unit ssh.service could not be found
```

This indicated that the OpenSSH Server package had not yet been installed.

Checking service status before troubleshooting prevents unnecessary configuration changes and helps identify missing software.

---

# Why Test Network Connectivity?

The following command was used:

```bash
ping -c 3 8.8.8.8
```

`ping` verifies:

* Internet connectivity
* Network interface functionality
* Packet transmission
* Basic routing

Receiving replies with **0% packet loss** confirmed that the system was successfully connected to the Internet.

---

# Why Use Git in System Administration?

Git is widely used by system administrators for:

* Documentation
* Configuration files
* Bash scripts
* Automation
* Infrastructure-as-Code
* Backup of configuration changes

Version control provides:

* Change history
* Rollback capability
* Collaboration
* Documentation tracking
* Safe experimentation

---

# Enterprise Usage

Package management is performed daily across enterprise Linux environments.

Administrators regularly:

* Install software
* Apply security updates
* Remove unused packages
* Upgrade applications
* Resolve dependency issues
* Maintain production servers
* Troubleshoot package failures

APT forms the foundation of software lifecycle management on Ubuntu systems.

---

# How Enterprise Package Management Works

Small laboratory environment:

```text
Administrator

↓

APT

↓

Ubuntu Repository

↓

Linux Server
```

Enterprise environment:

```text
Administrator

↓

Internal Package Repository

↓

APT

↓

Hundreds of Linux Servers

↓

Automated Deployment
```

Large organizations often maintain internal package repositories to ensure software consistency, improve security, and control software versions across all systems.

---

# Advantages of This Setup

* Trusted software repositories
* Automatic dependency resolution
* Easy software installation
* Simplified updates
* Efficient system cleanup
* Strong integration with Ubuntu
* Reliable package verification
* Standard enterprise package management workflow

---

# Limitations

Every package management solution has trade-offs.

This laboratory setup:

* Uses public Ubuntu repositories.
* Performs manual package management.
* Requires Internet connectivity.
* Installs software on a single machine.

Enterprise environments commonly include:

* Internal package mirrors
* Offline repositories
* Automated software deployment
* Centralized configuration management
* Software approval processes
* Enterprise patch management

---

# Lab Environment vs Enterprise Environment

| Feature      | Lab Environment            | Enterprise Environment                |
| ------------ | -------------------------- | ------------------------------------- |
| Repository   | Public Ubuntu repositories | Internal enterprise repositories      |
| Installation | Manual                     | Automated deployment systems          |
| Maintenance  | Individual administrator   | Centralized IT teams                  |
| Updates      | Manual APT commands        | Scheduled enterprise patch management |
| Monitoring   | Manual verification        | Centralized monitoring platforms      |
| Scale        | Single Linux workstation   | Hundreds or thousands of servers      |

---

# Possible Interview Questions

## Why use APT instead of downloading software manually?

APT installs software from trusted repositories, automatically resolves dependencies, simplifies updates, and improves software security and reliability.

---

## Why search before installing a package?

Searching verifies that the package exists, confirms the correct package name, and helps administrators choose the appropriate software.

---

## Why use `autoremove`?

`autoremove` removes dependency packages that are no longer required, helping keep the operating system clean and reducing unnecessary disk usage.

---

## Why did Git report a permission error?

Git was unable to read the file because Linux file permissions restricted access to the current user.

---

## Why verify SSH before configuring it?

Checking the service status confirms whether OpenSSH is installed and prevents unnecessary troubleshooting if the service does not yet exist.

---

## Why test Internet connectivity with `ping`?

`ping` confirms that the system can communicate over the network by measuring packet delivery and response time.

---

# Administrator's Summary

If a senior administrator asks:

> **"Why did you perform these tasks?"**

A professional answer would be:

> I practiced Linux package management using APT because software installation, maintenance, and updates are core responsibilities of a Linux administrator. I learned how APT resolves dependencies, how `autoremove` keeps systems clean, and how Linux permissions affect applications such as Git. I also practiced troubleshooting common administrative issues involving Git, networking, and SSH services. Together, these skills form the foundation for maintaining secure, stable, and well-managed Linux systems in both small and enterprise environments.

---

## Learning Outcome

After completing this lab, I can:

* Manage Linux software packages using APT
* Install and remove system utilities safely
* Identify and resolve Git permission issues
* Correct command syntax errors in terminal workflows
* Test network connectivity
* Understand SSH service setup requirements

```
```
