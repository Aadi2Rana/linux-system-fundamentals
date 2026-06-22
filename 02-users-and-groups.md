# Lesson 2: User and Group Administration

## Objective
To demonstrate enterprise-level identity management by creating specialized administrative groups, provisioning new system user accounts with customized environments, and auditing account configuration records.

---

## Step 1: Provisioning an Enterprise Group
In corporate environments, access control is managed via groups rather than individual users to scale securely.

**Command executed:**
sudo groupadd sysops
Step 2: Creating and Customizing User Accounts
Provisioning a new system user with an explicitly defined primary group and interactive shell environment.

Commands executed:

# Create user with home directory, primary group, and Bash shell
sudo useradd -m -g sysops -s /bin/bash engineer1

# Establish a secure account password
sudo passwd engineer1
Flags Explained:

-m: Generates the user's home space at /home/user_name.

-g: Restricts or assigns the user's explicit primary group affiliation.

-s: Assigns the system shell binary executable path for the user environment.

Step 3: Auditing Identity Management Accounts
Verifying account generation properties inside the central system authentication files.

Commands executed:

Bash
# Query the system identity database for the new account
getent passwd engineer1

# Audit group structures mapped to the user account
groups engineer1
Key Configuration File Learned:

/etc/passwd: The text file mapping user accounts to UIDs, GIDs, home paths, and startup shells.
