# Lesson 9: Enterprise Virtualization with KVM and Libvirt

## Objective

To deploy and configure the native Linux Kernel-based Virtual Machine (KVM) hypervisor together with the Libvirt virtualization management framework, allowing Ubuntu to host and manage isolated virtual machines for testing, development, and enterprise workloads.

---

# Environment

| Item | Value |
| :--- | :--- |
| **Operating System** | Ubuntu 24.04.4 LTS |
| **Kernel** | Linux 6.17.0-35-generic |
| **Hypervisor** | KVM (Kernel-based Virtual Machine) |
| **Virtualization Manager** | Libvirt |
| **Virtual Machine Emulator** | QEMU |
| **Host User** | otaku |

---

# Step 1: Verify CPU Hardware Virtualization Support

Before installing the virtualization stack, the processor was checked to confirm that Intel VT-x or AMD-V virtualization extensions were available.

**Command Executed**

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

**Output**

```text
8
```

### Result

A non-zero value confirms that hardware virtualization is available and supported by the processor.

---

# Step 2: Install the KVM Virtualization Stack

The complete virtualization environment was installed using Ubuntu's package manager.

**Command Executed**

```bash
sudo apt update

sudo apt install qemu-kvm \
libvirt-daemon-system \
libvirt-clients \
virtinst \
bridge-utils -y
```

### Components Installed

- KVM Kernel Hypervisor
- QEMU Hardware Emulator
- Libvirt Daemon
- Libvirt Client Utilities
- Virtual Machine Installation Tools
- Bridge Networking Utilities

---

# Step 3: Verify the Libvirt Service

After installation, the Libvirt daemon status was inspected.

**Command Executed**

```bash
sudo systemctl status libvirtd
```

### Observed Status

```text
Loaded: loaded
Enabled: yes
TriggeredBy:
 • libvirtd.socket
 • libvirtd-admin.socket
 • libvirtd-ro.socket
```

The service appeared as:

```text
Active: inactive (dead)
```

### Explanation

Modern Ubuntu releases use **systemd socket activation**.

Instead of continuously running in memory, **libvirtd** starts automatically whenever a virtualization client (such as `virsh` or Virtual Machine Manager) requests access.

This behavior is expected and indicates that socket activation is functioning correctly rather than representing a service failure.

---

# Step 4: Configure User Permissions

To avoid managing virtual machines as the root user, the standard user account was granted access to the virtualization management groups.

**Commands Executed**

```bash
sudo usermod -aG kvm otaku

sudo usermod -aG libvirt otaku
```

The updated group memberships were applied immediately.

```bash
newgrp libvirt
```

---

# Step 5: Verify Hypervisor Communication

The Libvirt management interface was tested using the `virsh` command-line client.

**Command Executed**

```bash
virsh list --all
```

**Output**

```text
 Id   Name   State
--------------------
```

### Result

The empty table confirms:

- Communication with the KVM hypervisor is successful.
- The Libvirt management service is operational.
- No virtual machines have been created yet.

This represents the expected output for a newly deployed virtualization host.

---

# Skills Practiced

During this lesson the following Linux administration skills were completed:

- Verifying CPU virtualization capabilities
- Installing enterprise virtualization packages
- Deploying the KVM hypervisor
- Configuring the Libvirt management framework
- Managing Linux user group permissions
- Understanding socket-activated system services
- Testing hypervisor connectivity using `virsh`
- Preparing Ubuntu as a virtual machine host

---

# Enterprise Concepts Learned

## KVM

Kernel-based Virtual Machine (KVM) converts the Linux kernel into a Type-1 hypervisor capable of running multiple isolated operating systems simultaneously.

---

## QEMU

QEMU provides hardware emulation for virtual machines while KVM supplies hardware-assisted acceleration for near-native performance.

---

## Libvirt

Libvirt is the centralized virtualization management framework responsible for:

- Creating virtual machines
- Starting and stopping guests
- Snapshot management
- Virtual networking
- Storage pool management
- Hypervisor communication

---

## virsh

`virsh` is the official command-line interface for interacting with the Libvirt API.

Common administrative operations include:

```bash
virsh list --all
virsh start VM_NAME
virsh shutdown VM_NAME
virsh destroy VM_NAME
virsh reboot VM_NAME
virsh autostart VM_NAME
```

---

# Summary

In this lesson, Ubuntu was successfully transformed into a virtualization host by deploying the complete KVM and Libvirt stack.

Hardware virtualization support was verified, enterprise virtualization packages were installed, user permissions were configured securely, and communication with the hypervisor was successfully validated using the `virsh` management interface.

The system is now fully prepared for creating and managing Linux and Windows virtual machines for testing, development, networking labs, and server administration.
