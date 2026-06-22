nano 03-permissions.md
# Lesson 3: Linux File Permissions & Ownership

## Objective
To master the Linux discretionary access control system by modifying file ownership parameters and configuring absolute numeric permissions to enforce strict data isolation.

---

## Step 1: Modifying Administrative Ownership
Securing files requires mapping them to the proper service accounts or personnel teams rather than default creator properties.

**Command executed:**
```bash
sudo chown engineer1:sysops server_auth.log
Step 2: Enforcing Read/Write Isolation
Configuring explicit bitmasks to secure system log infrastructure data.

Command executed:

Bash
chmod 640 server_auth.log
* **To Save and Exit Nano:** Press `Ctrl + O`, hit **Enter** to confirm the filename, then press `Ctrl + X` to exit.

---
