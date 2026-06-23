# Lesson 5: Networking, Connectivity Testing & SSH Setup

## Objective

To understand Linux networking fundamentals, analyze IP assignment changes, test internet connectivity, and install/configure the OpenSSH server for remote system administration.

---

## Environment

| Item    | Value                     |
| ------- | ------------------------- |
| OS      | Ubuntu Linux              |
| User    | otaku                     |
| Network | Dynamic (Hotspot / Wi-Fi) |
| Tooling | ip, ping, apt, systemctl  |

---

## Step 1: Network Interface Inspection

### Command

```bash
ip -br a
```

### Observations

* `lo` → Loopback interface (127.0.0.1)
* `wlp3s0` → Wireless network adapter

### Key Learning

The system dynamically assigns IP addresses depending on the connected network.

Example transitions:

* Mobile hotspot → `10.x.x.x`
* Home Wi-Fi → `192.168.x.x`

This confirms DHCP-based IP allocation.

---

## Step 2: Connectivity Testing

### Command

```bash
ping -c 3 8.8.8.8
```

### Result

* 0% packet loss
* Stable internet connection confirmed

### Key Insight

Lower latency on Wi-Fi compared to mobile hotspot indicates improved routing efficiency and signal stability.

---

## Step 3: SSH Server Installation & Verification

### Problem Identified

SSH service was not available:

```text
Unit ssh.service could not be found
```

---

### Solution

### Commands Executed

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

---

### Expected Output

* Service status: `active (running)`
* SSH daemon listening on port 22
* System ready for remote administration

---

## Key Skills Learned

* Network interface inspection
* IP address tracking via DHCP
* Connectivity testing using ICMP
* SSH server installation
* Linux service management using systemd

---

## Commands Used

```bash
ip -br a
ping -c 3 8.8.8.8
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

---

## Learning Outcome

After completing this lab, I can:

* Identify and interpret network interfaces
* Understand dynamic IP assignment
* Test network connectivity and latency
* Install and manage SSH services
* Use systemd to control Linux services

```
```
