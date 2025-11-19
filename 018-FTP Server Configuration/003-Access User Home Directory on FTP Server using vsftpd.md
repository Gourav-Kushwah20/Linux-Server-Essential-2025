# 📂 **Access User Home Directory on FTP Server using vsftpd**

This guide explains how to set up a basic FTP server using **vsftpd** on a Linux system.
The goal is to allow local users (e.g., `u1`, `u2`) to log in and securely access their own home directories.

---

## 🧑‍💻 **1. Create Local Users**

Create two test user accounts (`u1` and `u2`) for FTP access:

```bash
useradd u1
```

```bash
useradd u2
```

Set passwords for the new users:

```bash
passwd u1
```

```bash
passwd u2
```

> **Note:** Each user must have a valid password to authenticate via FTP.

---

## ⚙️ **2. Configure vsftpd**

Edit the vsftpd configuration file:

```bash
vim /etc/vsftpd/vsftpd.conf
```

### 📘 Example content for `/etc/vsftpd/vsftpd.conf`:

```ini
# Disallow anonymous logins
anonymous_enable=NO

# Enable local user login
local_enable=YES

# Allow FTP write commands (upload, delete, etc.)
write_enable=YES

# Set default permissions for uploaded files
local_umask=022

# Display directory-specific messages if any
dirmessage_enable=YES

# Enable logging of uploads/downloads
xferlog_enable=YES

# Ensure data connections use port 20
connect_from_port_20=YES

# Use standard FTP xferlog format
xferlog_std_format=YES

# Chroot local users to their home directories
chroot_local_user=YES

# Allow writeable chroot (required for writable home directories)
allow_writeable_chroot=YES

# Enable passive mode with defined port range
pasv_enable=YES
pasv_min_port=55000
pasv_max_port=55999

# PAM authentication service
pam_service_name=vsftpd

# Enable user list
userlist_enable=YES

# Enable IPv6 listener (disable 'listen=YES' if this is enabled)
listen=NO
listen_ipv6=YES
```

---

## 🛡️ **3. Check or Configure SELinux (If Applicable)**

### 🔍 View the current SELinux mode:

```bash
cat /etc/sysconfig/selinux
```

### 📄 Example output:

```
# SELinux status
SELINUX=disabled
SELINUXTYPE=targeted
```

### ⚠️ If SELinux is **enforcing**, you may need to enable the `ftp_home_dir` boolean:

```bash
setsebool -P ftp_home_dir 1
```

---

## 🔄 **4. Restart vsftpd Service**

### ✔️ Apply configuration changes by restarting vsftpd:

```bash
systemctl restart vsftpd.service
```

### 🔍 Check if the service is active:

```bash
systemctl status vsftpd.service
```

### 🚀 Enable vsftpd at boot:

```bash
systemctl enable vsftpd.service
```

---

## 📁 **5. Setup Proper Home Directory Permissions**

Ensure that user home directories exist and have correct permissions:

### 📂 Create home directories:

```bash
mkdir -p /home/u1 /home/u2
```

### 🔐 Assign ownership:

```bash
chown u1:u1 /home/u1
```
```bash
chown u2:u2 /home/u2
```
you can use `chmod 700` if you want their home directories to be private.

---

## 🔥 **6. Firewall Configuration**

This opens **port 21**, used for FTP control connections.

### ✔️ Allow FTP service:

```bash
firewall-cmd --permanent --add-service=ftp
```

### ✔️ If using firewalld, allow the passive port range:

```bash
firewall-cmd --permanent --add-port=55000-55999/tcp
```

### 🔄 Reload firewall:

```bash
firewall-cmd --reload
```

> This ensures that FTP passive mode works properly, especially behind NAT.

---

## 🧪 **7. Test FTP Login**

Test the FTP server using a client like the `ftp` command:

```bash
ftp 192.168.1.21 
```

### Example login:

```
Name (192.168.1.50:u1): u1
Password: 123
```

If successful, you will be placed in:

```
/home/u1
```

and have read/write access (based on permissions).

---

## 🛠️ **8. Troubleshooting Tips**

## ❌ FTP Login Fails

### 🔍 Check authentication logs:

```bash
journalctl -xe | grep vsftpd
```

### ✅ Ensure the user is **not** listed in:

```
/etc/vsftpd/ftpusers
```

(Users in this file are denied FTP access.)

---

## 📁 **Directory Listing Fails**

If directory listing does not work:

* 🔥 Passive ports might be blocked by a firewall.
* 🔐 Ensure proper permissions on home directories.
* 🛡️ Ensure SELinux (if enabled) is not preventing access.

---

## 📰 **9. Optional: Add Banner Message**

You can add a custom banner message to display to users on login:

```ini
ftpd_banner=Welcome to the Armour FTP server.
```

Add this line to your: `/etc/vsftpd/vsftpd.conf`


---

## 📚 **10. Useful Resources**

* **vsftpd man page**
* **Logs:**

  * `/var/log/vsftpd.log`
  * `/var/log/xferlog`
  * `/var/log/secure`

---
