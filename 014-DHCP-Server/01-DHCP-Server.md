# 💻 **DHCP Server Configuration**

This document provides a step-by-step guide to **install, configure, and manage a DHCP server** on a Linux-based system.
All commands include short explanations for better understanding.

---

### 🔹 **1. List all available repositories to ensure the DHCP server package is available:**

```bash
yum repolist all
```

---

## 🧩 **2. Verify if DHCP Package is Installed**

Check if the DHCP package is already installed:

```bash
rpm -qa | grep dhcp
```

---

## ⚙️ **3. Install DHCP Server**

Install the DHCP server package:

```bash
yum install dhcp-server.x86_64
```

---

## ✅ **4. Verify Installation**

Check if the DHCP package and the DHCP server are installed:

```bash
rpm -qa | grep dhcp
```
```bash
rpm -qa | grep dhcp-server
```

---

## 🧠 **5. Get Detailed Information About DHCP Server**

Display detailed information about the installed DHCP server package:

```bash
rpm -qi dhcp-server
```
List the documentation files included with the DHCP server:

```bash
rpm -qd dhcp-server
```
List the Configuration files included with the DHCP server:
```bash
rpm -qc dhcp-server
```
List all files installed by DHCP server packge:
```bash
rpm -ql dhcp-server
```

---
Here’s the text content from your image beautifully formatted in **Markdown** with emojis and highlighting for better readability:

---

## ⚙️ **Edit DHCP Configuration File**

Open the DHCP configuration file for editing:

```bash
vim /etc/dhcp/dhcpd.conf
```

---

## 🧾 **Sample DHCP Configuration (Range-of-DHCP-Server)**

Below is an example of a **basic** `dhcpd.conf` configuration file 👇

```bash
authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {

    # Specify the range of lease IP address
    range 192.168.1.50 192.168.1.250;

    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time (in seconds)
    default-lease-time 600;

    # Max lease time (in seconds)
    max-lease-time 7200;
}
```

---
## 📘 **Reference Example Configuration**

> 🧩 Open the example configuration file for reference:

```bash
vim /usr/share/doc/dhcp-server/dhcpd.conf.example
```

---

## 🚀 **6. Start and Check DHCP Service**

### 🔍 **Check Service Status**

```bash
systemctl status dhcpd.service
```

### ▶️ **Start Service**

```bash
systemctl start dhcpd.service
```

### 🔁 **Restart Service**

```bash
systemctl restart dhcpd.service
```

### ⚙️ **Enable Service to Start at Boot**

```bash
systemctl enable dhcpd.service
```

---

## 🌐 **7. Check Open Ports**

### 🔎 **Check which ports are open and listening**

```bash
netstat -nltup
```

### 📡 **Check if DHCP ports are open**

```bash
netstat -nltup | grep dhcp
```

---
## 8.Update Firewall Rules (Firewalld)
**Open DHCP Port in firewall:**

- Allow DHCP traffic through `firewalld`:
```bash
firewall-cmd --add-port=67/udp --permanent
```
- Reload the firewall to apply changes:
```bash
firewall-cmd --reload
```
- Check if the DHCP port is open:
```bash
firewall-cmd --list-ports
```
- Alternatively, check the detailed firewall configuration:
```bash
firewall-cmd --list-all
```

## 9.Check DHCP Leases

- Display current DHCP leases:
```bash
cat /var/lib/dhcpd/dhcpd.leases
```