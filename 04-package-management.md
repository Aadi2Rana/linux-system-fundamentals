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
