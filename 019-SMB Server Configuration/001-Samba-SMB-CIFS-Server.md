# 🖥️ **Samba-SMB-CIFS-Server**

## 📁 **Samba / SMB / CIFS Server**

Common Internet File System (**CIFS**) is a dialect of the **Server Message Block (SMB)** protocol.
It is widely used for shared access to **files**, **printers**, and **serial ports** between systems on a local network.

* 📦 **SMB (Server Message Block):** A network protocol for sharing files, printers, and more between nodes.
* 🪟 **CIFS:** A Windows-originated implementation of SMB.
* 🐧 **Samba:** An open-source project implementing SMB/CIFS on Linux/Unix, enabling file and printer sharing with Windows clients.

---

## 🔧 **Use Cases**

* 🔄 Seamless file sharing between **Linux**, **Windows**, and **macOS**
* 🖨️ Printer sharing across mixed OS environments
* 🏢 Integration with **Active Directory**
* 🌐 Hosting cross-platform network shares for users and applications

---

## 📂 **Common Paths And Configs**

## 📘 **Main Samba Configuration File**

All configuration is managed through:

```bash
/etc/samba/smb.conf
```

---

## 📦 **Basic Share Example**

This configuration defines a shared directory with guest access:

```ini
[shared]
path = /srv/samba/shared
browseable = yes
read only = no
guest ok = yes
```

- path : Filesystem path to be shared.
- browseable : Whether the share is visible during network browsing.
- read only : If set to no , allows write access.
- guest ok : Enables guest access without login.
---

## 🛠️ **Common Commands**

---

## 📥 **Install Samba (Debian/Ubuntu)**

Install Samba and its dependencies:

```bash
sudo apt install samba
```

---

## 🔍 **Check Samba Version**

Verify the installed Samba version:

```bash
smbd --version
```

---

## 👤 **Add A Samba User**

Add a user to Samba (must exist as a system user first):

```bash
sudo smbpasswd -a <username>
```

---

## 🔄 **Restart Samba Services**

Apply changes by restarting the SMB and NetBIOS services:

```bash
sudo systemctl restart smbd nmbd
```

---

## 🧪 **Test Samba Configuration**

Check your **smb.conf** for syntax errors:

```bash
testparm
```

---

## 🔐 **Security And Permissions**

* 🔧 Ensure proper Linux file permissions (`chmod`, `chown`) are set on shared folders.
* 📘 Use these common parameters for access control in **smb.conf**:

```ini
valid users = username
read only = yes
write list = user1, user2
```

* 🔒 Enable password encryption:

```ini
encrypt passwords = yes
```

* 🛡️ Always prefer **SMBv2** or **SMBv3** to avoid vulnerabilities of **SMB1**.
* 🚫 Use firewall rules to restrict access to SMB ports (137–139, 445).

---

## 🌐 **Mounting SMB Shares (Client-Side)**

## 🕒 **Temporary Mount (CLI)**

To mount a remote SMB share to **/mnt/shared** using credentials:

```bash
sudo mount -t cifs //192.168.1.100/shared /mnt/shared -o username=user,password=pass,iocharset=utf8,vers=3.0
```

### Options:

* 👤 **username / password** : Auth credentials.
* 🔢 **vers=3.0** : Use SMBv3.
* 🌍 **iocharset=utf8** : UTF-8 encoding for file names.

---

# 🔁 **Persistent Mount With Fstab**

* To persist across reboots, add the following to **/etc/fstab**:

```bash
//192.168.1.100/shared /mnt/shared cifs username=user,password=pass,iocharset=utf8,vers=3.0 0 0
```

---

### 🔒 **Optional Security Tip:**

Use a **credentials file** instead of embedding credentials in fstab.

#### 📄 Example **/etc/samba/cred-user**:

```ini
username=user
password=pass
```

---

### 📌 Mount with:

```bash
mount -t cifs //192.168.1.100/shared /mnt/shared -o credentials=/etc/samba/cred-user,vers=3.0
```

---

### 🔐 Ensure that the credentials file is owned by root and has restricted permissions:

```bash
chown root:root /etc/samba/cred-user
```

```bash
chmod 600 /etc/samba/cred-user
```

---
