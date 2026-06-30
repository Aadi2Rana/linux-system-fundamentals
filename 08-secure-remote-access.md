---

# Lesson 8: Secure Remote Administration Using Tailscale and SSH

## Objective

To configure secure remote administration for an Ubuntu workstation without exposing SSH directly to the internet by creating a private encrypted network using Tailscale and remotely accessing the machine from an Android device using an SSH client.

---

## Environment

| Component        | Value            |
| ---------------- | ---------------- |
| Operating System | Ubuntu 24.04 LTS |
| SSH Server       | OpenSSH          |
| VPN              | Tailscale        |
| Mobile OS        | Android          |
| SSH Client       | Termius          |

---

# Why This Setup?

Normally SSH can only be used inside the local network.

Accessing a computer from anywhere on the Internet usually requires:

* Public IP
* Port Forwarding
* Firewall configuration
* Dynamic DNS

Opening SSH directly to the Internet increases the attack surface.

Instead, Tailscale creates a private encrypted network where only authenticated devices can communicate.

This allows remote administration securely without exposing port 22 to the public Internet.

---

# Step 1 - Verify SSH Server

Before enabling remote access, verify that the OpenSSH server is installed and running.

### Commands Executed

```bash
sudo apt install openssh-server -y

sudo systemctl enable --now ssh

sudo systemctl status ssh
```

### Result

The SSH daemon was successfully enabled.

Output confirmed:

```
Active: active (running)
```

The server is now listening on:

```
0.0.0.0:22
```

and

```
:::22
```

meaning both IPv4 and IPv6 connections are accepted.

---

# Step 2 - Install Tailscale

Installed Tailscale using the official installation script.

Command executed:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

After installation:

```bash
sudo tailscale up
```

Authentication was completed through the browser using the same account that would later be used on the mobile device.

---

# Step 3 - Obtain the Tailscale IP Address

After successful authentication:

```bash
tailscale ip -4
```

Output:

```
100.x.x.x
```

This private address becomes the permanent endpoint used for SSH communication between trusted devices.

---

# Step 4 - Configure the Mobile Device

Installed:

* Tailscale
* Termius

Logged into the same Tailscale account.

Initially the connection would not establish.

After investigation the issue was traced to Android Private DNS blocking the VPN connection.

Solution:

Disabled Private DNS temporarily.

After disabling it:

* Tailscale connected successfully.
* The device joined the private network.

---

# Step 5 - Configure SSH Connection

Inside Termius a new host profile was created.

Configuration:

```
Host:
100.x.x.x

Port:
22

Username:
otaku

Authentication:
Ubuntu account password
```

Upon first connection the SSH host key was accepted.

The connection succeeded immediately.

The Ubuntu shell appeared directly on the Android phone.

---

# Verification

Successfully executed remote commands from Android while connected through Tailscale.

This confirmed:

* SSH server functioning correctly
* Tailscale private network operational
* Secure remote administration working over mobile data

---

# Lessons Learned

During deployment several practical issues were encountered.

### Incorrect command

Typed:

```bash
sudo tailscale upsudo tailscale up
```

Result:

```
tailscale: unknown subcommand
```

Correct command:

```bash
sudo tailscale up
```

---

### Android connectivity issue

Problem:

Tailscale connected but SSH would not work.

Cause:

Android Private DNS interfered with the VPN connection.

Resolution:

Disabled Private DNS.

The VPN connected successfully.

---

# Skills Demonstrated

* Installing OpenSSH Server
* Managing Linux services using systemctl
* Installing third-party software repositories
* Configuring secure remote administration
* Understanding VPN overlay networking
* Mobile SSH administration
* Basic troubleshooting
* Secure authentication using encrypted tunnels

---

# Commands Used

```bash
sudo apt install openssh-server -y

sudo systemctl enable --now ssh

sudo systemctl status ssh

curl -fsSL https://tailscale.com/install.sh | sh

sudo tailscale up

tailscale ip -4
```

---

# Outcome

A Lenovo ThinkPad T430 running Ubuntu was successfully transformed into a remotely manageable Linux workstation.

Using Tailscale and OpenSSH, secure administration can now be performed from an Android device running Termius over any internet connection without exposing SSH directly to the public internet.

---
# Administrator's Notes

This section explains **why** this remote administration solution was implemented, **how** it works internally, its advantages and limitations, and how it compares with enterprise remote access solutions. It is intended to help reinforce both practical knowledge and the underlying networking concepts.

---

# Administrator's Explanation

## What Was Accomplished?

If someone asks:

> **"What did you accomplish in this lesson?"**

A professional answer would be:

> I configured a secure remote administration solution for an Ubuntu workstation by combining OpenSSH with Tailscale. Instead of exposing SSH directly to the Internet through router configuration and port forwarding, I created a private encrypted overlay network that allows authenticated devices to communicate securely from anywhere. This enables remote Linux administration without exposing SSH services to the public Internet.

---

# Why Use Tailscale?

A senior administrator may ask:

> **"Why did you choose Tailscale instead of exposing SSH through port forwarding?"**

A strong answer would be:

> I wanted a secure remote administration solution without modifying network infrastructure or exposing SSH directly to the Internet. Tailscale creates a private encrypted network between authenticated devices, eliminating the need for public IP addresses, router configuration, Dynamic DNS, and inbound firewall rules. This significantly reduces the attack surface while maintaining secure remote access from any location.

---

# How Traditional Remote SSH Works

Without a VPN, remote SSH typically follows this path:

```text
Remote Device
      │
      ▼
Public Internet
      │
      ▼
Router
      │
      ▼
Port Forwarding
      │
      ▼
Ubuntu SSH Server
```

For this configuration to work, administrators usually require:

* A public IP address
* Router port forwarding
* Firewall configuration
* Dynamic DNS (if the public IP changes)

The SSH service becomes reachable from anywhere on the Internet, increasing the system's exposure to automated scans and attack attempts.

---

# How Tailscale Works

With Tailscale, the architecture changes completely.

```text
Android Phone
        │
        │
Encrypted Tailscale Network
        │
        ▼
Ubuntu Workstation
```

Instead of accepting unsolicited inbound connections, both devices establish secure outbound encrypted connections to the Tailscale network.

Once authenticated, they communicate over a private overlay network using encrypted tunnels.

No router configuration is required because the connection is initiated from inside each network rather than from the public Internet.

---

# Why No Router Configuration Was Needed

A common question is:

> **"Why didn't you configure the router?"**

Because SSH was never intended to be publicly accessible.

Traditional remote administration usually requires:

* Port forwarding
* Public IP addressing
* Firewall rule modifications
* Dynamic DNS

Tailscale removes these requirements by creating a trusted private network between authenticated devices.

As a result, the router does not need to expose any internal services to the Internet.

---

# Why Is Port 22 Still Used?

SSH continues to use its default communication port:

```text
TCP Port 22
```

However, there is an important distinction.

Without Tailscale:

```text
Internet
      │
      ▼
Port 22 Exposed Publicly
```

With Tailscale:

```text
Internet
      │
(No Direct Access)
      │
      ▼
Authenticated Tailscale Devices
      │
      ▼
Port 22
```

Port 22 remains active, but only devices that belong to the private Tailscale network can access it.

---

# Could Another SSH Port Be Used?

Yes.

Many organizations configure SSH to listen on alternative ports such as:

```text
2222
22022
10022
```

Changing the port reduces automated scanning and unnecessary log entries.

However, changing the port **does not provide real security**.

Effective SSH security comes from:

* Strong authentication
* SSH key-based login
* Multi-factor authentication
* VPN access
* Firewall policies
* Access control
* Network segmentation

Changing the port should only be considered an additional administrative measure rather than a primary security control.

---

# Why Install OpenSSH?

OpenSSH provides the secure communication service that allows administrators to remotely access Linux systems.

It supports:

* Remote shell access
* Secure file transfer
* Secure command execution
* Port forwarding
* Key-based authentication
* Administrative automation

Without the SSH service running, remote administration would not be possible.

---

# Why Use `systemctl`?

The following command was executed:

```bash
sudo systemctl enable --now ssh
```

This performs two operations simultaneously.

```text
Enable Service
      │
      ▼
Start Automatically at Boot
```

and

```text
Start Service Immediately
```

Using a single command ensures the SSH service is available immediately and remains available after future system reboots.

---

# Why Obtain the Tailscale IP Address?

After joining the Tailscale network, each device receives a unique private IP address.

Example:

```text
100.x.x.x
```

Unlike public IP addresses, this address exists only within the encrypted Tailscale network.

Administrators use this address to securely access remote systems without exposing them to the public Internet.

---

# Why Use Termius?

Termius provides a secure SSH client for Android devices.

Using Termius allows administrators to:

* Execute Linux commands remotely
* Manage services
* Monitor system health
* Perform troubleshooting
* Maintain servers while away from the office

The mobile device effectively becomes a portable Linux administration terminal.

---

# Why Did Android Private DNS Cause Problems?

During deployment, Tailscale connected but remote SSH communication initially failed.

The issue was traced to Android's Private DNS configuration.

Private DNS altered the device's DNS behavior, interfering with Tailscale's networking functionality.

Temporarily disabling Private DNS restored normal VPN communication and allowed encrypted connectivity to function correctly.

This demonstrates the importance of systematic troubleshooting when diagnosing networking issues.

---

# Verification

Running remote commands successfully confirmed that:

* OpenSSH was functioning correctly.
* Tailscale established a secure encrypted tunnel.
* Remote authentication succeeded.
* Secure shell access worked over the private VPN.
* The Ubuntu workstation could be administered from outside the local network without exposing SSH publicly.

---

# Enterprise Usage

Enterprise organizations frequently require administrators to securely manage servers located across multiple offices, factories, branch locations, or cloud environments.

Secure remote administration is commonly used for:

* Server maintenance
* Emergency troubleshooting
* Infrastructure monitoring
* Software deployment
* Security updates
* Configuration management
* Remote technical support

Modern organizations often implement VPN technologies before permitting administrative access to internal systems.

---

# Enterprise Remote Access Architecture

Small laboratory deployment:

```text
Administrator

↓

Tailscale

↓

Ubuntu Server
```

Enterprise deployment:

```text
Administrator

↓

VPN Gateway

↓

Firewall

↓

Internal Network

↓

Linux Servers

Windows Servers

Database Servers

Application Servers
```

Large organizations usually provide secure remote access through enterprise VPN solutions rather than exposing internal services directly to the Internet.

---

# Advantages of This Setup

* No router configuration required
* No port forwarding required
* No public IP address required
* End-to-end encrypted communication
* SSH is not exposed to the Internet
* Works behind NAT
* Supports remote administration from virtually anywhere
* Easy to deploy
* Excellent solution for home laboratories, development systems, and small organizations

---

# Limitations

Every solution has trade-offs.

This deployment requires:

* Internet connectivity
* A Tailscale account
* Tailscale installed on every participating device
* Trust in the Tailscale coordination infrastructure

For large enterprise environments, organizations often prefer centrally managed remote access solutions integrated with existing security policies and identity management systems.

---

# Lab Environment vs Enterprise Environment

| Feature              | Lab Environment                          | Enterprise Environment                           |
| -------------------- | ---------------------------------------- | ------------------------------------------------ |
| Purpose              | Remote administration of one workstation | Secure access to entire corporate infrastructure |
| Users                | Individual administrator                 | Multiple administrators and IT teams             |
| Router Configuration | Not required                             | Usually integrated into enterprise networking    |
| Public IP            | Not required                             | Managed by VPN gateways or network providers     |
| Encryption           | End-to-end encrypted                     | VPN and enterprise security policies             |
| Cost                 | Low                                      | Higher infrastructure investment                 |
| Scalability          | Small deployments                        | Large enterprise environments                    |

---

# Possible Interview Questions

## Why use Tailscale instead of exposing SSH directly?

Because Tailscale provides encrypted private networking without requiring router configuration, port forwarding, or exposing SSH services to the public Internet, significantly reducing the attack surface.

---

## Why is SSH still using port 22?

SSH continues to communicate over TCP port 22, but access is restricted to authenticated devices within the private Tailscale network rather than being publicly reachable.

---

## Why wasn't router configuration necessary?

Both devices establish outbound encrypted connections to the Tailscale network, eliminating the need for inbound port forwarding or firewall modifications.

---

## Why use a VPN for remote administration?

VPNs provide encrypted communication, authenticated access, and secure connectivity over untrusted public networks while protecting internal systems from direct Internet exposure.

---

## Why install OpenSSH?

OpenSSH provides the secure remote management service that allows administrators to remotely execute commands, transfer files, and administer Linux systems.

---

## Why was Android Private DNS causing issues?

Private DNS interfered with VPN networking, preventing proper communication between authenticated devices. Disabling it restored normal encrypted connectivity.

---

# Administrator's Summary

If a senior administrator asks:

> **"Why did you build it this way?"**

A professional answer would be:

> I chose Tailscale together with OpenSSH because it provides a secure and simple method of remote administration without exposing SSH directly to the public Internet. Rather than relying on router port forwarding, public IP addresses, or Dynamic DNS, Tailscale establishes an authenticated encrypted overlay network between trusted devices. SSH continues to provide secure remote shell access while remaining inaccessible to unauthorized Internet users. This approach is well suited for home laboratories, development environments, and small-scale remote administration, while larger enterprises typically implement centralized VPN solutions, site-to-site VPNs, SD-WAN, or MPLS to securely connect entire networks and administrative workstations.
