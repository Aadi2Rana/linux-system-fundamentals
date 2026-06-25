# Lesson 6: Service Management and Log Auditing

## Objective

To demonstrate Linux service administration using `systemctl`, troubleshoot Git synchronization issues, and inspect system services through the `systemd` framework.

---

## Environment

| Component        | Value          |
| ---------------- | -------------- |
| Operating System | Ubuntu Linux   |
| Architecture     | x86_64         |
| Service Manager  | systemd        |
| Target Service   | ssh.service    |
| Tools Used       | systemctl, git |

---

## Step 1: Repository Synchronization Troubleshooting

While synchronizing the local repository with the remote GitHub repository, Git detected a conflict caused by an untracked file.

### Command Executed

```bash
git pull origin main
```

### Error Encountered

```text
error: The following untracked working tree files would be overwritten by merge:
05-networking-and-ssh.md
```

### Resolution

The conflicting local file was removed before performing the pull operation again.

```bash
rm 05-networking-and-ssh.md
git pull origin main
```

### Result

```text
Updating 21aacf7..9e75965
Fast-forward
```

The local repository was successfully synchronized with GitHub.

---

## Step 2: Verifying System Architecture

### Command Executed

```bash
uname -m
```

### Output

```text
x86_64
```

### Interpretation

The system is running on a 64-bit Intel/AMD architecture.

---

## Step 3: Service Lifecycle Management

Administrators frequently start, stop, restart, and verify services without rebooting production servers.

### Stop SSH Service

```bash
sudo systemctl stop ssh
```

### Output

```text
Stopping 'ssh.service', but its triggering units are still active:
ssh.socket
```

### Explanation

The SSH service is configured with socket activation. The `ssh.socket` unit remains active and can automatically start the SSH service when required.

---

### Start SSH Service

```bash
sudo systemctl start ssh
```

### Verify Status

```bash
sudo systemctl status ssh
```

### Output Summary

```text
Loaded: loaded
Active: active (running)
Main PID: 74992 (sshd)
```

### Interpretation

* SSH service is installed correctly.
* Service is enabled during boot.
* SSH daemon is currently running.
* Remote administrative access is available.

---

## Concepts Learned

### systemctl

Used to manage system services:

```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl status ssh
```

### Service States

| State            | Meaning                       |
| ---------------- | ----------------------------- |
| active (running) | Service is operating normally |
| inactive         | Service is stopped            |
| failed           | Service encountered an error  |

### Socket Activation

Some services use socket activation where a companion `.socket` unit starts the service only when a connection is received.

Example:

```text
ssh.socket
```

This reduces resource consumption and improves efficiency.

---

## Skills Demonstrated

* Git repository synchronization
* Resolving Git pull conflicts
* Service lifecycle management
* SSH administration
* System architecture auditing
* Understanding socket-activated services
