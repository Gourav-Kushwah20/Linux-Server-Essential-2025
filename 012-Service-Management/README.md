# 🛠 Service Management in Linux

![alt text](image.png)

Managing services in Linux is a fundamental part of system administration.  
Services are background processes that provide core functionality such as networking, web hosting, logging, and more.

---

## 🔑 Key Points of a Linux Service
- ⚙️ **Background Process**: Runs in the background without user interaction.  
- 🖥 **System Management**: Provides core system functions (e.g., networking, logging).  
- 🛑 **Controlled by Init Systems**: Managed by **systemd**, **upstart**, or **SysVinit**.  
- 📂 **Configuration & Logs**: Config files usually in `/etc`, logs in `/var/log`.

---

## 📌 Managing Services with SysVinit (`/etc/init.d/`)

> Used in older Linux systems (Debian/Ubuntu before systemd, RHEL ≤6).

### 📋 List available scripts
```bash
cd /etc/init.d/
```

```bash
ls -lh /etc/init.d/
```
### 👀 View a service script
```bash
cat /etc/init.d/apache2
```
---
### 🎛 Service control commands
```bash
/etc/init.d/apache2 status
```
```bash
/etc/init.d/apache2 start
```
```bash
/etc/init.d/apache2 stop
```

```bash
/etc/init.d/apache2 restart
```
- reload Configuration
```bash
/etc/init.d/apache2 force-reload
```
---
## 🔹 Managing Services with `service` Command (Legacy)

Wrapper command for **SysVinit scripts**:

```bash
service apache2 status
```

```bash
service apache2 start
```

```bash
service apache2 stop
```

```bash
service apache2 restart
```

```bash
service apache2 reload
```

## 🔹 Managing Services with systemctl (systemd)

Modern distributions like RHEL 7+, CentOS 7+, Ubuntu 16.04+, and Debian Jessie+ use systemd.

### 🔍 Checking service status
```bash
systemctl status httpd.service
```
### ▶️ Starting and Stopping
```bash
systemctl start httpd.service
```
```bash
systemctl stop httpd.service
```

### 🔄 Restart and Reload
```bash
systemctl restart httpd.service
```
```bash
systemctl reload httpd.service
```

### ⚙️ Manage Startup Behavior
```bash
systemctl enable httpd.service   # Enable on boot
```
```bash
systemctl disable httpd.service  # Disable on boot
```
```bash
systemctl is-enabled httpd.service
```
---
## ⚡ Advanced Service Management in Linux

## 🔹 Listing Services with `systemctl`

### 📋 All active services
```bash
systemctl list-units --type=service
```
### ▶️ Running services
```bash
systemctl list-units --type=service --state=running
```

### ❌ Failed services
```bash
systemctl list-units --type=service --state=failed
```
---

## 🔹 Masking & Unmasking Services
### 🚫 Prevent service from running (even manually)
```bash
systemctl mask bluetooth
```

### ✅ Re-enable service
```bash
systemctl unmask bluetooth
```
---
## 🔹 Viewing Service Logs (with journalctl)
### 📜 Logs for a service
```bash
journalctl -u httpd.service
```

### ⏱ Logs since last hour
```bash
journalctl -u httpd.service --since "1 hour ago"
```

### 📡 Live log output
```bash
journalctl -u httpd.service -f
```
---

## 🔹 chkconfig (SysVinit Runlevel Management)

Used to enable/disable services at specific runlevels in SysVinit-based systems.

## Show all services
```bash
chkconfig --list
```

### Enable a service
```bash
chkconfig httpd on
```

### Disable a service
```bash
chkconfig httpd off
```