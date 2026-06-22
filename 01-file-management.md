# Lesson 1: Linux File System Navigation & Management

## Objective
To demonstrate core competency in navigating the Linux directory structure, creating organized workspaces, managing files via the Command Line Interface (CLI), and inspecting system storage.

---

## Step 1: Inspecting System Storage
Before performing administrative tasks, a SysAdmin must verify available disk space. 

**Command executed:**
```bash
df -h
df -h: Shows total, used, and available disk space in human-readable formats (G for Gigabytes).
# Verify current home location
pwd

# Create a structured directory path
mkdir -p ~/workspace/sysadmin_labs/01_navigation

# Navigate into the project directory
cd ~/workspace/sysadmin_labs/01_navigation
# Create multiple administrative target files simultaneously
touch server_auth.log app_error.log system_config.txt database.db

# View detailed properties of the directory contents
ls -lh
