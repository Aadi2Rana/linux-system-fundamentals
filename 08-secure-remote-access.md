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
