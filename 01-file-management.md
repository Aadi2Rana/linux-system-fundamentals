# Linux SysAdmin Labs – Lesson 01: File Management & Navigation

## Objective

This lab demonstrates essential Linux system administration skills involving:

* Checking available disk space
* Navigating the Linux filesystem
* Creating organized directory structures
* Managing files through the Command Line Interface (CLI)
* Verifying file creation and permissions

---

## Environment

| Item             | Value        |
| ---------------- | ------------ |
| Operating System | Ubuntu Linux |
| User             | otaku        |
| Shell            | Bash         |

---

## Step 1: Inspect System Storage

Before performing administrative tasks, a system administrator should verify available storage resources.

### Command

```bash
df -h
```

### Output

```bash
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           360M  2.8M  358M   1% /run
/dev/sda2       117G   13G   98G  12% /
tmpfs           1.8G   24M  1.8G   2% /dev/shm
tmpfs           5.0M  8.0K  5.0M   1% /run/lock
tmpfs           360M  160K  360M   1% /run/user/1000
```

### Explanation

The `df -h` command displays disk usage statistics in a human-readable format.

Key observations:

* Root filesystem (`/dev/sda2`) has 117 GB total storage.
* Approximately 98 GB is available.
* Current disk utilization is 12%.

---

## Step 2: Workspace Creation & Navigation

A structured directory layout helps maintain organized administrative projects.

### Verify Current Directory

```bash
pwd
```

Output:

```bash
/home/otaku
```

### Create Project Directory

```bash
mkdir -p ~/workspace/sysadmin_labs/01_navigation
```

### Navigate Into Directory

```bash
cd ~/workspace/sysadmin_labs/01_navigation
```

### Explanation

* `mkdir -p` creates parent directories if they do not already exist.
* `cd` changes the current working directory.
* `pwd` displays the absolute path of the current location.

---

## Step 3: File Creation & Verification

System administrators frequently work with log files, configuration files, and databases.

### Create Files

```bash
touch server_auth.log app_error.log system_config.txt database.db
```

### Verify File Creation

```bash
ls -lh
```

### Output

```bash
total 0
-rw-rw-r-- 1 otaku otaku 0 Jun 22 22:01 app_error.log
-rw-rw-r-- 1 otaku otaku 0 Jun 22 22:01 database.db
-rw-rw-r-- 1 otaku otaku 0 Jun 22 22:01 server_auth.log
-rw-rw-r-- 1 otaku otaku 0 Jun 22 22:01 system_config.txt
```

### Explanation

Files created:

| File              | Purpose                |
| ----------------- | ---------------------- |
| server_auth.log   | Authentication logs    |
| app_error.log     | Application error logs |
| system_config.txt | Configuration data     |
| database.db       | Database file          |

The `ls -lh` command provides:

* File permissions
* Ownership information
* File size
* Modification timestamp

---

## Skills Demonstrated

* Disk space monitoring
* Filesystem navigation
* Directory creation
* File creation
* File inspection
* Linux command-line proficiency

---

## Commands Used

```bash
df -h
pwd
mkdir -p ~/workspace/sysadmin_labs/01_navigation
cd ~/workspace/sysadmin_labs/01_navigation
touch server_auth.log app_error.log system_config.txt database.db
ls -lh
```

---

## Learning Outcome

After completing this lab, I can:

* Navigate the Linux filesystem efficiently
* Create structured project directories
* Create and manage files using the CLI
* Inspect storage utilization
* Verify file properties and permissions
* Apply foundational Linux administration practices

```
```
# Administrator's Notes

This section explains **why** Linux file management and navigation are fundamental to system administration, **how** the Linux filesystem is organized, why storage monitoring is important, and how these basic skills are applied in enterprise environments.

---

# Administrator's Explanation

## What Was Accomplished?

If someone asks:

> **"What did you accomplish in this lesson?"**

A professional answer would be:

> I practiced the core Linux administration skills required to navigate the filesystem, monitor available storage, create structured project directories, manage files from the command line, and inspect file metadata. These are foundational tasks that every Linux administrator performs before managing services, users, applications, or servers.

---

# Why Learn Linux File Management?

A senior administrator may ask:

> **"Why is file management one of the first things a Linux administrator should learn?"**

A strong answer would be:

> Everything in Linux is organized around the filesystem. Configuration files, application logs, user data, services, and devices are all accessed through files and directories. Before managing servers effectively, an administrator must understand how to navigate the filesystem, organize data, and inspect files safely.

---

# Why Check Disk Space First?

The following command was executed:

```bash
df -h
```

Before making changes to a system, administrators verify that sufficient storage is available.

The workflow looks like this:

```text
Administrator

↓

Check Disk Usage

↓

Verify Available Space

↓

Perform Administrative Tasks
```

Running out of disk space can prevent applications, databases, and even the operating system from functioning correctly.

Checking storage before beginning work is considered a best practice.

---

# Why Use `df -h`?

The `df` command reports filesystem usage.

The `-h` option displays values in a human-readable format.

Instead of showing:

```text
102945792
```

It displays:

```text
98G
```

This makes storage utilization much easier to interpret during routine administration.

---

# Understanding the Root Filesystem

One important observation was:

```text
/dev/sda2
```

mounted on:

```text
/
```

The root filesystem is the primary storage location for Linux.

It contains:

* Operating system files
* Installed applications
* User home directories
* System configuration
* Logs
* Libraries

Every directory ultimately exists beneath the root directory.

---

# Why Use `pwd`?

The following command was executed:

```bash
pwd
```

It displays the current working directory.

Example:

```text
/home/otaku
```

Knowing the current location prevents administrators from accidentally modifying files in the wrong directory.

---

# Why Create Organized Directories?

The following command was used:

```bash
mkdir -p ~/workspace/sysadmin_labs/01_navigation
```

A structured directory layout makes projects easier to manage.

The structure becomes:

```text
Home Directory

↓

workspace

↓

sysadmin_labs

↓

01_navigation
```

Organized directories improve maintainability and make projects easier to understand for both individuals and teams.

---

# Why Use the `-p` Option?

The `-p` option creates parent directories automatically.

Without it:

```text
workspace

↓

sysadmin_labs

↓

01_navigation
```

would need to be created one level at a time.

With `-p`, the entire directory structure is created in a single command.

---

# Why Change Directories?

The following command was executed:

```bash
cd ~/workspace/sysadmin_labs/01_navigation
```

Changing into the project directory ensures that all newly created files remain organized and stored in the intended location.

Administrators frequently move between directories while managing systems, configuration files, and logs.

---

# Why Create Empty Files with `touch`?

The following command was executed:

```bash
touch server_auth.log app_error.log system_config.txt database.db
```

The `touch` command creates empty files without adding content.

Administrators commonly use it to:

* Prepare log files
* Create configuration files
* Generate placeholder files
* Test permissions
* Verify application behavior

It is one of the most frequently used Linux commands.

---

# Why Use Meaningful File Names?

Each file was created with a descriptive purpose.

| File                  | Purpose                            |
| --------------------- | ---------------------------------- |
| **server_auth.log**   | Stores authentication-related logs |
| **app_error.log**     | Stores application error messages  |
| **system_config.txt** | Stores configuration information   |
| **database.db**       | Represents a database file         |

Using clear and descriptive names improves readability and simplifies troubleshooting.

---

# Why Verify Files with `ls -lh`?

The following command was executed:

```bash
ls -lh
```

This confirms:

* File creation
* File ownership
* Permissions
* File size
* Modification timestamps

Administrators routinely verify system changes after creating or modifying files.

---

# Understanding File Permissions

Example output:

```text
-rw-rw-r--
```

This information indicates:

* File type
* Owner permissions
* Group permissions
* Permissions for other users

Even before learning advanced permission management, administrators should understand how to recognize basic permission information.

---

# Why Learn the Command Line?

Linux administrators primarily manage servers through the command line.

Many production servers:

* Have no graphical interface.
* Are accessed remotely using SSH.
* Are managed through terminal sessions.

Command-line proficiency allows administrators to work efficiently regardless of the environment.

---

# Enterprise Usage

File management is performed every day in enterprise environments.

Administrators routinely:

* Create project directories
* Organize configuration files
* Monitor disk usage
* Manage application logs
* Inspect file permissions
* Prepare deployment directories
* Verify storage availability

These tasks form the foundation for nearly every other administrative responsibility.

---

# How Enterprise File Management Works

Small laboratory environment:

```text
Administrator

↓

Linux Filesystem

↓

Project Files
```

Enterprise environment:

```text
Administrator

↓

Linux Servers

↓

Application Directories

↓

Configuration Files

↓

Logs

↓

Databases

↓

Shared Storage
```

Although enterprise systems are much larger, they rely on the same filesystem concepts demonstrated in this lab.

---

# Advantages of This Setup

* Organized directory structure
* Simple project management
* Efficient command-line workflow
* Easy storage monitoring
* Standard Linux filesystem practices
* Foundation for future administration tasks
* Clear file organization

---

# Limitations

Every file management approach has trade-offs.

This laboratory environment:

* Uses a single local filesystem.
* Does not demonstrate multiple disks or partitions.
* Does not include network storage.
* Does not include filesystem permissions beyond basic inspection.
* Demonstrates one workstation.

Enterprise environments commonly include:

* Multiple storage volumes
* RAID arrays
* Network Attached Storage (NAS)
* Storage Area Networks (SAN)
* Distributed filesystems
* Automated storage monitoring

---

# Lab Environment vs Enterprise Environment

| Feature      | Lab Environment        | Enterprise Environment                              |
| ------------ | ---------------------- | --------------------------------------------------- |
| Storage      | Single local disk      | Multiple storage systems                            |
| Files        | Personal project files | Applications, databases, logs, and shared resources |
| Monitoring   | Manual `df` checks     | Centralized storage monitoring                      |
| Organization | Individual workspace   | Standardized enterprise directory structures        |
| Access       | Single user            | Multiple users and services                         |
| Scale        | One Linux workstation  | Hundreds or thousands of servers                    |

---

# Possible Interview Questions

## Why check disk space before starting work?

To verify that sufficient storage is available for applications, logs, updates, and administrative tasks while preventing failures caused by insufficient disk space.

---

## Why use `mkdir -p`?

The `-p` option creates parent directories automatically, allowing complex directory structures to be created in a single command.

---

## Why use `pwd`?

`pwd` displays the current working directory, helping administrators verify their location before creating, modifying, or deleting files.

---

## Why use `touch`?

`touch` creates empty files or updates file timestamps, making it useful for preparing configuration files, logs, and testing administrative workflows.

---

## Why verify files with `ls -lh`?

`ls -lh` confirms that files exist and displays important metadata such as permissions, ownership, file size, and modification times.

---

## Why is command-line proficiency important?

Most Linux servers are administered remotely without graphical interfaces, making command-line skills essential for efficient system management.

---

# Administrator's Summary

If a senior administrator asks:

> **"Why did you perform these tasks?"**

A professional answer would be:

> I practiced the fundamental Linux administration skills required to work with the filesystem efficiently. I verified storage availability before beginning work, created an organized directory structure, navigated the filesystem, created administrative files, and verified their properties using standard Linux commands. These foundational skills are essential because nearly every administrative task—from managing users and services to troubleshooting applications—depends on understanding how Linux organizes and manages files.
