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
