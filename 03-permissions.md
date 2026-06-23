# Lesson 3: Linux File Permissions & Ownership

## Objective

To understand Linux file ownership, permission control using `chmod`, and how system-level restrictions behave under different user contexts. This lab also includes troubleshooting permission and SSH-related issues.

---

## Environment

| Item     | Value        |
| -------- | ------------ |
| OS       | Ubuntu Linux |
| User     | otaku        |
| Shell    | Bash         |
| Lab User | engineer1    |

---

## Step 1: File Ownership Transfer

After creating log files in a workspace directory, ownership was transferred to a system user.

### Command Executed

```bash id="p1q9aa"
sudo chown engineer1:sysops server_auth.log
ls -l server_auth.log
```

### Output

```text id="p1q9ab"
-rw-rw-r-- 1 engineer1 sysops 0 Jun 22 22:27 server_auth.log
```

### Explanation

The file ownership was successfully changed:

* Owner → `engineer1`
* Group → `sysops`

This simulates real-world log ownership delegation in enterprise systems.

---

## Step 2: Permission Modification Attempt

An attempt was made to restrict file permissions using numeric mode.

### Command Executed

```bash id="p1q9ac"
chmod 640 server_auth.log
```

### Output

```text id="p1q9ad"
chmod: changing permissions of 'server_auth.log': Operation not permitted
```

### Observation

Although the file ownership was correctly assigned, permission modification failed unexpectedly.

---

## Step 3: System Behavior & Troubleshooting Insight

The error `Operation not permitted` indicates that:

* The file system or environment may be enforcing restrictions
* The command context may not have sufficient privilege over the file at runtime
* The file may be under a controlled or mounted environment with limited write access rules

Despite the error, file metadata still showed:

```text id="p1q9ae"
-rw-rw-r-- 1 engineer1 sysops 0 Jun 22 22:27 server_auth.log
```

---

## Step 4: SSH Service Investigation

During setup, SSH service status was checked.

### Command Executed

```bash id="p1q9af"
sudo systemctl status ssh
```

### Output

```text id="p1q9ag"
Unit ssh.service could not be found.
```

### Resolution

OpenSSH server was installed and enabled:

```bash id="p1q9ah"
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
```

---

## Step 5: Git & GitHub Workflow Setup

The repository was cloned and updated using SSH-based Git authentication.

### Commands Executed

```bash id="p1q9ai"
git clone git@github.com:Aadi2Rana/linux-system-fundamentals.git
cd linux-system-fundamentals
git add .
git commit -m "Added Lesson 3 Permissions"
git push
```

---

## Step 6: Git SSH Connection Issue (Resolved)

An initial SSH verification failure occurred due to first-time host key verification.

### Error

```text id="p1q9aj"
Host key verification failed.
```

### Resolution

GitHub host was manually verified and added to known hosts:

```text id="p1q9ak"
Warning: Permanently added 'github.com' to known hosts.
```

---

## Skills Demonstrated

* File ownership management (`chown`)
* Permission control (`chmod`)
* Linux troubleshooting
* SSH setup and debugging
* Git SSH authentication workflow
* Repository synchronization

---

## Commands Used

```bash id="p1q9al"
sudo chown engineer1:sysops server_auth.log
chmod 640 server_auth.log
sudo systemctl status ssh
sudo apt install openssh-server -y
git clone git@github.com:Aadi2Rana/linux-system-fundamentals.git
git add .
git commit -m "Added Lesson 3 Permissions"
git push
```

---

## Learning Outcome

After completing this lab, I can:

* Assign file ownership in Linux
* Understand permission bit manipulation
* Identify and troubleshoot permission errors
* Install and manage SSH services
* Use Git with SSH authentication
* Maintain a structured Linux lab workflow

```
```
