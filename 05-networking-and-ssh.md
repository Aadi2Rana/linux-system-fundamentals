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
# Administrator's Notes

This section explains **why** networking fundamentals are important for Linux administration, **how** IP addressing and connectivity testing work, why SSH is used for remote management, and how these concepts are applied in enterprise environments.

---

# Administrator's Explanation

## What Was Accomplished?

If someone asks:

> **"What did you accomplish in this lesson?"**

A professional answer would be:

> I examined the network configuration of an Ubuntu system, verified Internet connectivity, observed how DHCP dynamically assigns IP addresses across different networks, and deployed the OpenSSH server to enable secure remote administration. These are fundamental networking and system administration skills required for managing Linux servers in both small and enterprise environments.

---

# Why Learn Linux Networking?

A senior administrator may ask:

> **"Why is networking one of the first things a Linux administrator should learn?"**

A strong answer would be:

> Almost every Linux server communicates with other systems over a network. Before deploying applications, managing servers, or troubleshooting connectivity issues, an administrator must understand how interfaces, IP addressing, routing, DNS, and network services work. Networking forms the foundation of remote administration and enterprise infrastructure.

---

# How a Linux System Connects to a Network

When a Linux computer joins a network, the communication process typically looks like this:

```text
Linux Computer
      │
      ▼
Network Interface
      │
      ▼
Router / Access Point
      │
      ▼
Internet
```

The network interface provides connectivity, while the router forwards traffic between the local network and external networks.

---

# Why Inspect Network Interfaces?

The following command was used:

```bash
ip -br a
```

This provides a simplified overview of the system's network interfaces.

Example output:

```text
lo

wlp3s0
```

Each interface has a specific purpose.

| Interface  | Purpose                                                                |
| ---------- | ---------------------------------------------------------------------- |
| **lo**     | Loopback interface used for internal communication within the computer |
| **wlp3s0** | Wireless network adapter used for external network connectivity        |

Understanding available interfaces helps administrators troubleshoot network problems and verify connectivity.

---

# What Is the Loopback Interface?

The loopback interface (`lo`) always exists, even when the computer is disconnected from every network.

```text
Computer

↓

127.0.0.1

↓

Computer
```

It allows applications running on the same machine to communicate with each other without leaving the system.

Many services use the loopback interface for local testing and internal communication.

---

# Why Did the IP Address Change?

One important observation was that the IP address changed depending on the connected network.

Example:

```text
Mobile Hotspot

↓

10.x.x.x
```

```text
Home Wi-Fi

↓

192.168.x.x
```

This occurs because different routers maintain different private IP address ranges.

Each network has its own DHCP server that automatically assigns available addresses to connected devices.

---

# What Is DHCP?

DHCP stands for **Dynamic Host Configuration Protocol**.

Instead of configuring network settings manually, the DHCP server automatically provides:

* IP address
* Subnet mask
* Default gateway
* DNS server information

The process looks like this:

```text
Linux Computer

↓

DHCP Request

↓

Router

↓

Assign IP Address

↓

Network Ready
```

This automation simplifies network management and reduces configuration errors.

---

# Why Test Connectivity with `ping`?

The following command was executed:

```bash
ping -c 3 8.8.8.8
```

The `ping` utility sends ICMP Echo Request packets to verify network connectivity.

Successful replies confirm:

* Network interface is functioning.
* The default gateway is reachable.
* Internet connectivity is available.
* Packet transmission is working correctly.

A successful result with **0% packet loss** indicates reliable communication.

---

# Why Use Google's DNS Server?

The address:

```text
8.8.8.8
```

belongs to Google's public DNS service.

It is commonly used for connectivity testing because:

* It is highly available.
* It is globally reachable.
* It responds reliably to ICMP requests.
* It provides a consistent target for troubleshooting.

Using a well-known public address helps isolate connectivity problems.

---

# Why Did Wi-Fi Have Lower Latency?

Latency measures the time required for data to travel to a destination and back.

Lower latency generally indicates:

* Better signal quality
* More efficient routing
* Lower network congestion
* Faster communication

Comparing hotspot and Wi-Fi connections demonstrates how different network paths affect performance.

---

# Why Install OpenSSH?

OpenSSH provides secure remote administration for Linux systems.

Instead of physically accessing the computer, administrators can securely connect over the network.

The communication path becomes:

```text
Administrator

↓

SSH Client

↓

Encrypted Connection

↓

Linux Server
```

SSH is one of the most important services used in Linux system administration.

---

# Why Was the SSH Service Missing?

Initially, the system displayed:

```text
Unit ssh.service could not be found
```

This simply indicated that the OpenSSH Server package was not installed.

Installing the package created:

* The SSH daemon
* The service definition
* The required configuration files

Once installed, the service became available for management through `systemctl`.

---

# Why Use `systemctl`?

The command:

```bash
sudo systemctl enable --now ssh
```

performs two tasks simultaneously.

```text
Enable Service

↓

Start Automatically During Boot
```

and

```text
Start Service Immediately
```

This ensures that SSH is available immediately and automatically after every future system restart.

---

# Why Verify the Service Status?

The following command was used:

```bash
sudo systemctl status ssh
```

This confirms:

* The service is installed.
* The service is running.
* The daemon started successfully.
* SSH is ready to accept remote connections.
* Recent log messages are available if troubleshooting is required.

Verifying service status is a standard administrative practice after installing or modifying services.

---

# Why Does SSH Listen on Port 22?

By default, SSH uses:

```text
TCP Port 22
```

This standardized port allows SSH clients to locate the remote service without additional configuration.

Many organizations retain the default port internally while restricting access through firewalls or VPNs.

---

# Enterprise Usage

Networking and SSH administration are essential in enterprise environments.

Administrators routinely perform tasks such as:

* Configuring network interfaces
* Troubleshooting connectivity
* Deploying remote management services
* Managing secure remote access
* Monitoring network performance
* Supporting servers across multiple locations

Nearly every Linux server in production relies on these networking concepts.

---

# How Enterprise Networking Works

Small laboratory environment:

```text
Administrator

↓

Wi-Fi Router

↓

Ubuntu Server
```

Enterprise environment:

```text
Administrator

↓

Corporate Network

↓

Firewall

↓

Core Switches

↓

Linux Servers

Database Servers

Application Servers

Virtual Machines
```

Although enterprise networks are much larger, they rely on the same networking principles learned in this lab.

---

# Advantages of This Setup

* Simple network configuration
* Automatic IP address assignment
* Easy connectivity verification
* Secure remote administration
* Standard Linux networking tools
* SSH encrypted communication
* Foundation for enterprise server management

---

# Limitations

Every networking configuration has trade-offs.

This laboratory environment:

* Relies on dynamic IP addresses.
* Uses a basic wireless network.
* Does not include static IP configuration.
* Does not demonstrate VLANs or routing.
* Does not include firewall configuration.
* Supports a single Linux workstation.

Enterprise environments commonly include:

* Static IP addressing
* VLAN segmentation
* Enterprise firewalls
* VPN connectivity
* Redundant network paths
* Centralized network monitoring

---

# Lab Environment vs Enterprise Environment

| Feature       | Lab Environment              | Enterprise Environment                         |
| ------------- | ---------------------------- | ---------------------------------------------- |
| Network       | Home Wi-Fi or Mobile Hotspot | Enterprise LAN and WAN                         |
| IP Assignment | DHCP                         | DHCP and Static IPs                            |
| SSH Access    | Single workstation           | Hundreds or thousands of servers               |
| Monitoring    | Manual testing               | Centralized monitoring platforms               |
| Security      | Basic network security       | Firewalls, VPNs, IDS/IPS, network segmentation |
| Scale         | One Linux computer           | Large enterprise infrastructure                |

---

# Possible Interview Questions

## Why inspect network interfaces?

To identify available network adapters, verify IP assignments, troubleshoot connectivity issues, and confirm interface status.

---

## Why did the IP address change?

Different networks use different DHCP servers, which automatically assign available IP addresses based on their configured address ranges.

---

## Why use `ping`?

`ping` verifies network connectivity by sending ICMP Echo Requests and measuring response time, packet loss, and reachability.

---

## Why use Google's DNS server for testing?

The address `8.8.8.8` is highly available, globally reachable, and provides a reliable target for verifying Internet connectivity.

---

## Why install OpenSSH?

OpenSSH provides encrypted remote administration, allowing administrators to securely manage Linux systems across a network.

---

## Why verify the SSH service after installation?

Verifying the service confirms that it installed successfully, started correctly, and is ready to accept secure remote connections.

---

# Administrator's Summary

If a senior administrator asks:

> **"Why did you perform these tasks?"**

A professional answer would be:

> I practiced the networking fundamentals required for Linux administration by examining network interfaces, understanding DHCP-based IP assignment, verifying Internet connectivity, and deploying the OpenSSH server for secure remote management. These tasks establish the foundation for administering Linux systems across networks. Understanding how interfaces, IP addressing, connectivity testing, and SSH work together is essential before progressing to more advanced topics such as VPNs, firewalls, enterprise networking, and large-scale infrastructure management.

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
