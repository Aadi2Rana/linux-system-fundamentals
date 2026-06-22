# Lesson 4: Package Management and Software Lifecycle via APT

## Objective
To demonstrate secure package management proficiencies by querying remote repository indexes, analyzing software metadata fields, executing managed installations, and maintaining lean operating system storage by clearing unreferenced dependencies.

---

## Step 1: Querying Package Repositories and Metadata
Before introducing new software binaries to an environment, a SysAdmin must audit available versions and system footprint implications.

**Commands executed:**
```bash
# Query the local cache database index for target keywords
apt search htop

# Analyze explicit package metadata records and structural dependencies
apt show htop
Step 2: Managed Package Deployment & Storage Optimization
Installing system binaries cleanly and purging obsolete orphan packages to prevent configuration drift.

Commands executed:

Bash
# Execute software installation using automated non-interactive confirmations
sudo apt install htop -y

# Detect and purge unreferenced dependencies and orphan packages
sudo apt autoremove -y
