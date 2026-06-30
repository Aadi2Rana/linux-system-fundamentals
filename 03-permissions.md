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
# Administrator's Notes

This section explains **why** Linux file permissions and ownership are fundamental to system security, **how** Linux controls access to files, why SSH authentication is important, and how these concepts are applied in enterprise Linux environments.

---

# Administrator's Explanation

## What Was Accomplished?

If someone asks:

> **"What did you accomplish in this lesson?"**

A professional answer would be:

> I practiced Linux file ownership and permission management by assigning ownership with `chown`, attempting to modify permissions with `chmod`, investigating permission-related errors, configuring OpenSSH for secure remote access, and using Git with SSH authentication. These tasks demonstrate core Linux security concepts and administrative workflows used to manage files and systems securely.

---

# Why Learn File Permissions?

A senior administrator may ask:

> **"Why are Linux permissions so important?"**

A strong answer would be:

> Linux is a multi-user operating system where multiple users and services share the same system. File permissions protect data by ensuring that only authorized users and processes can read, modify, or execute files. Proper permission management is one of the most important responsibilities of a Linux administrator because it directly affects system security and stability.

---

# How Linux File Ownership Works

Every file in Linux has two ownership attributes:

* Owner
* Group

The ownership model looks like this:

```text id="d1t2k7"
File
 │
 ├── Owner
 │
 └── Group
```

The owner controls the file, while the group allows multiple users with similar responsibilities to share access.

---

# Why Use `chown`?

The following command was executed:

```bash id="p6u9mz"
sudo chown engineer1:sysops server_auth.log
```

This changes:

* File owner → `engineer1`
* File group → `sysops`

The workflow is:

```text id="a4j7cn"
Administrator

↓

chown

↓

Owner Updated

↓

Group Updated
```

Changing ownership is commonly used when transferring responsibility for files between users or services.

---

# Why Assign Files to Groups?

Groups simplify permission management.

Instead of granting permissions individually to every user, administrators assign users to groups.

Example:

```text id="q8w4la"
sysops Group

↓

engineer1

administrator

backup-service
```

Any file owned by the **sysops** group can be shared according to its group permissions.

This makes administration significantly easier in enterprise environments.

---

# Understanding Linux Permissions

Linux permissions are divided into three categories.

```text id="r3h5bt"
Owner

Group

Others
```

Each category may receive:

```text id="n6m1ys"
Read (r)

Write (w)

Execute (x)
```

Example:

```text id="u9e4pk"
-rw-r--r--
```

Meaning:

| Category | Permission   |
| -------- | ------------ |
| Owner    | Read + Write |
| Group    | Read         |
| Others   | Read         |

This permission model provides flexible access control while maintaining security.

---

# Why Use `chmod`?

The command:

```bash id="f7v3rd"
chmod 640 server_auth.log
```

uses numeric permission notation.

The value:

```text id="b2x9cf"
640
```

means:

| Number | Permission   |
| ------ | ------------ |
| 6      | Read + Write |
| 4      | Read Only    |
| 0      | No Access    |

Resulting permissions:

```text id="v5z1jw"
Owner → rw-

Group → r--

Others → ---
```

Numeric notation provides administrators with a fast and consistent method of managing permissions.

---

# Why Did "Operation Not Permitted" Occur?

The following error appeared:

```text id="k8f6xa"
Operation not permitted
```

Although ownership appeared correct, Linux denied the permission change.

Possible causes include:

* The command was executed by a user without sufficient privileges.
* The file resided on a filesystem with restricted permission handling.
* Immutable file attributes or mount options prevented modification.
* The environment imposed additional security restrictions.

A Linux administrator should investigate rather than assume the cause.

Useful troubleshooting commands include:

```bash
ls -l server_auth.log
id
mount
lsattr server_auth.log
```

Troubleshooting permission issues is a routine administrative task.

---

# Why Verify File Metadata?

After the failed operation, the file was examined again.

```bash id="c7q8fw"
ls -l server_auth.log
```

This verifies:

* Current owner
* Current group
* Existing permissions
* File size
* Timestamp

Administrators verify system state before making additional changes.

---

# Why Install OpenSSH?

The following command returned:

```text id="w4n7hp"
Unit ssh.service could not be found.
```

This indicated that the OpenSSH Server package was not installed.

Installing OpenSSH provides:

* Secure remote login
* Secure command execution
* File transfer
* Remote administration

SSH is the standard protocol used for Linux server administration.

---

# Why Use Git with SSH?

Instead of using HTTPS authentication, Git was configured to communicate through SSH.

The communication path becomes:

```text id="x2r6lm"
Git

↓

SSH Authentication

↓

GitHub

↓

Repository
```

SSH authentication offers several advantages:

* Strong cryptographic authentication
* No repeated password prompts
* Secure communication
* Better automation support

Most Linux administrators prefer SSH for managing Git repositories.

---

# Why Did Host Key Verification Fail?

During the first SSH connection to GitHub, the following error occurred:

```text id="m5t9ed"
Host key verification failed
```

This happens because SSH had never communicated with GitHub before.

The first connection follows this process:

```text id="p4c2kh"
Connect to GitHub

↓

Receive Host Key

↓

Administrator Verifies Identity

↓

Store Key in known_hosts

↓

Future Connections Trusted
```

Accepting the verified host key protects against man-in-the-middle attacks by ensuring future connections communicate with the correct server.

---

# Enterprise Usage

Permission management is one of the most important responsibilities of Linux administrators.

Common enterprise tasks include:

* Managing shared directories
* Assigning application ownership
* Protecting sensitive files
* Controlling service accounts
* Securing configuration files
* Managing SSH authentication
* Maintaining Git repositories

Every production Linux server relies on these security principles.

---

# How Enterprise Permission Management Works

Small laboratory environment:

```text id="z1u7ne"
Administrator

↓

chown

↓

chmod

↓

Linux File
```

Enterprise environment:

```text id="g6y2mp"
Administrator

↓

Identity Management

↓

User Groups

↓

Application Accounts

↓

Shared Storage

↓

Linux Servers
```

Enterprise environments often integrate Linux permissions with centralized identity services while still relying on the same ownership and permission model.

---

# Advantages of This Setup

* Strong file security
* Flexible access control
* Multi-user support
* Group-based administration
* Secure remote authentication
* Standard Linux security model
* Secure Git authentication
* Enterprise-compatible permission management

---

# Limitations

Every access control system has trade-offs.

This laboratory environment:

* Uses local user accounts.
* Manages permissions manually.
* Does not include Access Control Lists (ACLs).
* Does not integrate with centralized authentication.
* Demonstrates a single workstation.

Enterprise environments commonly include:

* LDAP or Active Directory integration
* Access Control Lists (ACLs)
* SELinux or AppArmor
* Centralized identity management
* Enterprise SSH key management
* Automated permission auditing

---

# Lab Environment vs Enterprise Environment

| Feature           | Lab Environment            | Enterprise Environment                      |
| ----------------- | -------------------------- | ------------------------------------------- |
| Users             | Local users                | Centralized identity management             |
| Permissions       | Standard Linux permissions | Permissions plus ACLs and security policies |
| Authentication    | Local SSH keys             | Enterprise key management                   |
| Repository Access | Individual Git repository  | Organization-managed repositories           |
| Security          | Manual administration      | Automated auditing and compliance           |
| Scale             | Single workstation         | Hundreds or thousands of servers            |

---

# Possible Interview Questions

## Why use `chown`?

`chown` changes file ownership, allowing administrators to assign responsibility for files to the appropriate users or service accounts.

---

## Why use groups?

Groups simplify permission management by allowing multiple users to share access without assigning permissions individually.

---

## What does permission `640` mean?

`640` grants the owner read and write permissions, the group read-only permission, and no permissions to others.

---

## Why did `chmod` return "Operation not permitted"?

Possible causes include insufficient privileges, filesystem restrictions, immutable file attributes, or security mechanisms preventing permission changes.

---

## Why use SSH instead of HTTPS with Git?

SSH provides secure key-based authentication, simplifies repeated access, supports automation, and avoids entering passwords for every operation.

---

## Why does SSH verify host keys?

Host key verification confirms the identity of the remote server and protects against man-in-the-middle attacks.

---

# Administrator's Summary

If a senior administrator asks:

> **"Why did you perform these tasks?"**

A professional answer would be:

> I practiced Linux ownership and permission management because protecting files and controlling user access are fundamental responsibilities of a Linux administrator. I learned how ownership and permissions work together to secure resources, investigated permission-related errors through troubleshooting, configured OpenSSH for secure remote administration, and used Git with SSH authentication for secure repository management. These concepts form the security foundation of Linux systems and are used extensively across enterprise environments to protect applications, services, and sensitive data.
