# 🔐 LDAP Server Setup

## 🖥️ Ldap Server Setup


## 🚫 Disable IPv6

To **disable IPv6** on **CentOS 9**, follow any one of the methods below 👇



## ⚙️ Method 1: Disable IPv6 Using GRUB (Recommended)

### ✏️ Edit GRUB configuration
```bash
vim /etc/default/grub
````

---

### ➕ Add `ipv6.disable=1` to `GRUB_CMDLINE_LINUX`

```bash
GRUB_CMDLINE_LINUX="ipv6.disable=1 ..."
```

📌 *(Keep existing parameters and just add `ipv6.disable=1`)*

---

### 🔄 Regenerate GRUB configuration

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

---

### 🔁 Reboot the system

```bash
reboot
```

---

## ⚙️ OR: Disable IPv6 Using `sysctl`

### ✏️ Edit sysctl configuration

```bash
vim /etc/sysctl.conf
```

---

### ➕ Add the following lines

```ini
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

---

### 🚀 Apply changes immediately

```bash
sysctl -p
```

---

## ✅ Verification (Optional)

Check IPv6 status:

```bash
ip a
```

If IPv6 is disabled, no `inet6` entries will appear ✅

---

## 🌐 Configure DNS Server with BIND9

BIND9 is available in **CentOS repositories** and is commonly used to configure a DNS server.

---

## 📦 Install BIND9

### 🔽 Install BIND and utilities
```bash
yum install bind bind-utils -y
```

---

### ▶️ Enable and start the DNS service

```bash
systemctl enable named
```
```bash
systemctl start named
```

---

## ⚙️ Configure DNS Zone

### ✏️ Edit local DNS configuration

```bash
vim /etc/named.conf
```

---

### 🔓 Allow DNS queries from local network

```conf
options {
    ...
    allow-query { localhost; 192.168.1.0/24; };
};
```

📌 *This allows DNS requests from localhost and the local LAN.*

---

### 🗂️ Define a New DNS Zone

```bash
vim /etc/named/server.local.zone
```

📁 This file will contain **DNS zone records** (A, PTR, NS, etc.)

---

## ✅ Summary

* 📦 BIND9 installed successfully
* ▶️ `named` service enabled & started
* 🌐 DNS queries allowed for local network
* 🗂️ Zone file ready for configuration

🚀 **DNS Server setup with BIND9 is ready to proceed**

```

If you want, I can also:
- 🧠 Add **forward & reverse zone file examples**
- 🔐 Integrate **DNS with LDAP**
- 📘 Convert this into **exam notes**
- 📄 Prepare **full DNS + LDAP documentation**

Just tell me 👍😊
```
