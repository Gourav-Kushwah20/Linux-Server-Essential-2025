# 🖥️ Samba Server Setup On RHEL/CentOS-Based Systems

This guide outlines how to install, configure, manage, and test a Samba server on RHEL or CentOS using `yum` and related tools. Samba allows Linux systems to share files and printers with Windows clients over the SMB/CIFS protocol.

---

## 📦 Installation

### 👉 Install the base Samba package:

```bash
yum install samba
```

### 👉 Install all related Samba packages (clients, tools, etc.):

```bash
yum install samba*
```

---

## 🔍 Verifying Samba Installation

### 📦 List Installed Samba Packages

```bash
rpm -qa | grep samba
```

---

### ℹ️ Get Detailed Info On Samba Package

```bash
rpm -qi samba
```

---

### 📁 List Samba Configuration Files

```bash
rpm -qc samba
```

---

### 📚 List Samba Documentation Files

```bash
rpm -qd samba
```
---

### 📚 List All Installed by Samba Packages

```bash
rpm -ql samba
```
---

## ⚙️ Configuration

## 📝 Edit Samba Configuration File

**Open the main Samba configuration file using a text editor:**

```bash
vim /etc/samba/smb.conf
```

Refer to **/etc/samba/smb.conf.example** or the **man smb.conf** for detailed syntax.

---

## 📄 Example Minimal Configuration

```ini
[global]
    workgroup = SAMBA
    security = user
    passdb backend = tdbsam
    printing = cups
    printcap name = cups
    load printers = yes
    cups options = raw

[homes]
    comment = Home Directories
    valid users = %S, %D%w%S
    browseable = No
    read only = No
    inherit acls = Yes

[printers]
    comment = All Printers
    path = /var/tmp
    printable = Yes
    create mask = 0600
    browseable = No
```

---

## 👥 Managing Samba Users

### ➤ Create A Linux User

```bash
useradd smb-user1
```
```bash
useradd infosec
```
---

### 🔍 View Samba Password Command Help

```bash
smbpasswd -h
```

**Or**

```bash
smbpasswd --help
```

---

## ➕ Add Users To Samba

> Each user must first exist as a Linux user:

```bash
smbpasswd -a smb-user1
```

```bash
smbpasswd -a sec-learner
```

```bash
smbpasswd -a infosec
```

---

## 🔧 Managing Samba Services

### 📌 Check Samba Service Status

```bash
systemctl status smb.service
```

---

### ▶️ Start Samba Service
```bash
systemctl start smb.service
````

---

### 🔄 Enable Samba Service On Boot

```bash
systemctl enable smb.service
```

---

## 🌐 Networking

### 🔍 Check Listening SMB Ports

```bash
netstat -nltup
```

**Filter for Samba-related ports:**

```bash
netstat -nltup | grep smb
```

---

## 🔥 Firewall Configuration With Firewalld

Samba uses ports **137/udp, 138/udp, 139/tcp, and 445/tcp**.

---

### 🚀 Start And Enable Firewalld

```bash
systemctl start firewalld
```

```bash
systemctl enable firewalld
```

---

### 🛡️ Check Firewall State

```bash
firewall-cmd --state
```
- check the list of port all in firewall:
```bash
firewall-cmd --list-ports
```
---

### 🔥 Add Samba Services To The Firewall (Permanent)

```bash
firewall-cmd --permanent --add-service=samba
```

You can also add individual ports if preferred:

```bash
firewall-cmd --permanent --add-port=137/udp
firewall-cmd --permanent --add-port=138/udp
firewall-cmd --permanent --add-port=139/tcp
firewall-cmd --permanent --add-port=445/tcp
```

---

### 🔄 Reload Firewall To Apply Changes

```bash
firewall-cmd --reload
```

---

### 📋 Verify Active Rules

```bash
firewall-cmd --list-all
```

---

## 🧪 Testing Samba Shares From A Client

### 📁 List Available Shares (Anonymous)

```bash
smbclient -L 192.168.1.21
```

---

### 🔐 List Shares With User Authentication

```bash
smbclient -L 192.168.1.21 -U sec-learner
```

```bash
smbclient -L 192.168.1.21 -U infosec
```

---

### 🔗 Connect To A Share

```bash
smbclient //192.168.1.21/infosec -U infosec
```

---

### 📂 List Files And Directories

```bash
ls
```

---

### 📁 Change Directory

```bash
cd <foldername>
```

**Example:**

```bash
cd reports
```

---

### 📥 Download A File From The Share

```bash
get <remote_filename>
```

**Example:**

```bash
get confidential.txt
```

---

### 🧺 Upload A File To The Share

```bash
put <local_filename>
```

**Example:**

```bash
put notes.txt
```
---

### 📥 Download Multiple Files

```bash
mget *.txt
```

* You will be prompted for each file. Use `prompt` to toggle confirmation:

```bash
prompt
```

---

### 📤 Upload Multiple Files

```bash
mput *.pdf
```

---

### 🔙 Go Back To Previous Directory

```bash
cd ..
```

---

### 🏠 Go To Root Of Share

```bash
cd /
```

---

### 🗂️ Create A New Directory

```bash
mkdir <dirname>
```

**Example:**

```bash
mkdir backups
```

---

### ❌ Remove A File

```bash
del <filename>
```
### Exit The Samba Shell
```bash
exit
```
---
## 🔐 Samba Password And TDB Database Management

### 📂 Navigate To Samba Private Database Directory
```bash
cd /var/lib/samba/private
````

**Files present:**

```bash
passdb.tdb   secrets.ldb   secrets.tdb
```

### 📜 List Samba User Info Verbosely

```bash
pdbedit -L -v
```

### 📄 List Samba Users In smbpasswd Format

```bash
pdbedit -L -w
```

---

## 📘 Understanding Samba's Database Files

### **passdb.tdb**

* **Purpose:** Stores Samba user account information (passwords, flags, etc.)
* **Backend:** Used when `passdb backend = tdbsam` is set in `smb.conf`
* **Location:** Usually found in `/var/lib/samba/private/` or `/var/lib/samba/`

---

## 🔐 **secrets.tdb**

**Purpose:** Contains internal secrets used by Samba:

* Domain membership keys
* Trust passwords
* LDAP bind credentials

**Highly sensitive:** Should be readable by root only.

**Location:** Typically in `/var/lib/samba/private/`

---

## 🔐 **secrets.ldb**

**Purpose:** Used in Samba AD DC environments.

**Part of:** Samba’s LDB (LDAP-like) database when acting as an Active Directory domain controller.

**Stores:** Domain controller secrets, machine credentials, replication metadata, etc.

**Location:** Usually in `/var/lib/samba/private/`

---

### 📌 *If you're using Samba as an AD DC* (with `samba-tool domain provision`), **secrets.ldb** replaces or supplements **secrets.tdb**.

---

### 👓 View Contents (Advanced Tool Required)

```bash
ldbsearch -H /var/lib/samba/private/secrets.ldb
```

---

## 🛡️ **Best Practices**

* Do **not** edit these files manually.
* Always **backup** before upgrading or migrating Samba.
* **Restrict permissions:**

```bash
chmod 600 /var/lib/samba/private/*
```

```bash
chown root:root /var/lib/samba/private/*
```


---

# 🛠️📦 TDB Tools For Database Management

## 📥 Install TDB Tools

Install using **yum**:

```bash
yum install tdb-tools
```

Install using **apt**:

```bash
apt install tdb-tools
```

---

## 🔍 Verify TDB Tools Installation

Check if installed:

```bash
rpm -qa | grep tdb-tools
```

View package info:

```bash
rpm -qi tdb-tools
```

List files installed by package:

```bash
rpm -ql tdb-tools
```

---

## 🗂️ Dump Samba TDB Databases

Dump **passdb.tdb**:

```bash
tdbdump /var/lib/samba/private/passdb.tdb
```

Dump **secrets.tdb**:

```bash
tdbdump /var/lib/samba/private/secrets.tdb
```

Or from current working directory:

Dump **passdb.tdb**:

```bash
tdbdump passdb.tdb
```

Dump **secrets.tdb**:

```bash
tdbdump secrets.tdb
```

---