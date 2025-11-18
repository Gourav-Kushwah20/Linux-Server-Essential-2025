# 🚀 **Anonymous-FTP-Access-Configuration**

## 🔐 **Anonymous FTP Access Configuration on a Linux Server**

This guide walks you through installing, configuring, and enabling **anonymous FTP access** using **vsftpd** on a Red Hat–based system. It also includes firewall setup using **firewalld**. 🔧🐧

---

## ✅ **1. Check for Existing FTP Packages**

Check if any FTP-related packages are already installed:

```bash
rpm -qa | grep ftp
```

---

## 📦 **2. Install vsftpd and FTP Client**

Install the FTP server and client if they aren't already installed:

```bash
yum install vsftpd ftp
```

Verify installation:

```bash
rpm -qa | grep ftp
```

---

## 🔍 **3. Inspecting vsftpd Package Details**

📘 **Check detailed package information:**

```bash
rpm -qi vsftpd
```

📂 **List installed files:**

```bash
rpm -ql vsftpd
```

⚙️ **Check configuration files:**

```bash
rpm -qc vsftpd
```
---

## 🛠️ **Configure vsftpd**

### ✏️ Edit the main configuration file:

```bash
vim /etc/vsftpd/vsftpd.conf
```

---

### 📄 **Example configuration:**

```bash
anonymous_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
anon_upload_enable=YES
anon_mkdir_write_enable=YES
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
pasv_min_port=55000
pasv_max_port=55999
pasv_enable=YES
```

---

### ✅ **This configuration enables:**

* 👥 **Anonymous and local user login**
* 📤 **Upload and directory creation for anonymous users**
* 🌐 **Passive mode** (required for many FTP clients)

---

## 🔧 **Manage vsftpd Service**

### 🔍 **Check if the service is running:**

```bash
systemctl status vsftpd.service
```

---

### ▶️ **Start the service:**

```bash
systemctl start vsftpd.service
```

---

### 🔄 **Restart the service (e.g., after configuration changes):**

```bash
systemctl restart vsftpd.service
```

---

### ⚙️ **Enable the service at boot:**

```bash
systemctl enable vsftpd.service
```

---

## 🛰️ **6. Verify FTP Listening Ports**

Check if the FTP service is listening on the correct ports:

```bash
netstat -nltup | grep ftp
```

---

## 📁 **7. Set Up Anonymous Content**

### 📂 Navigate to the default FTP root:

```bash
cd /var/ftp/
```

mkdir -v /var/ftp/pub/data

### 🔑 Set correct ownership:

```bash
chown -Rv nobody:nobody /var/ftp/pub/data
```

### 🔓 Set permissions:

```bash
chmod -Rv 777 /var/ftp/pub/data
```

---

### 📦 Copy content for anonymous access:

```bash
cp -vr /var/www/html/site* /var/ftp/pub/data 
```

---

## 🗂️ **Check Open Files Used by FTP**

### 🔍 Verify FTP-related open files and sockets:

```bash
lsof | grep ftp
```

---

# 🛡️ **Configuring firewalld for FTP Access**

By default, **firewalld** is the firewall tool on many Linux distributions (CentOS 7+, RHEL 7+, Fedora, etc.).
To support **FTP**, especially **passive FTP**, you must allow the correct ports. 🔓

---

## 🚀 **Start and Enable firewalld**

### ▶️ Ensure the service is running:

```bash
systemctl start firewalld
```

### 🔄 Enable the service at boot:

```bash
systemctl enable firewalld
```

### 📌 Check its status:

```bash
systemctl status firewalld
```

---

## 🔐 **2. Allow FTP Service**

This opens **port 21**, used by FTP control connections.

```bash
firewall-cmd --permanent --add-service=ftp
```

---

## 🌐 **3. Allow Passive FTP Ports**

If passive mode is configured in `/etc/vsftpd/vsftpd.conf`
(e.g., **55000–55999**), allow that range explicitly:

```bash
firewall-cmd --permanent --add-port=55000-55999/tcp
```

---

## 🔄 **4. Reload firewalld to Apply Changes**

```bash
firewall-cmd --reload
```
---

## 📋 **Verify Rules**

### 📜 List all allowed services and ports:

```bash
firewall-cmd --list-all
```

---

## 🌐 **Optional: Allow Access Only in Specific Zones**

If you're using a specific zone (like **public**, **internal**, etc.), use:

```bash
firewall-cmd --zone=public --permanent --add-service=ftp
```
```bash
firewall-cmd --zone=public --permanent --add-port=55000-55999/tcp
```
```bash
firewall-cmd --reload
```

---

## 🛡️ **7. SELinux Consideration (If Enforcing)**

If SELinux is enabled, you may also need to allow FTP passive mode using:

```bash
setsebool -P ftp_home_dir=1
setsebool -P allow_ftpd_full_access=1
```

### 🔍 Confirm SELinux settings:

```bash
getsebool -a | grep ftp
```

---

## 📊 **Summary of Required Ports**

| **Service** | **Protocol** | **Port(s)** | **Description**          |
| ----------- | ------------ | ----------- | ------------------------ |
| FTP         | TCP          | 21          | Control connection       |
| FTP-PASV    | TCP          | 55000–55999 | Passive data connections |

---
