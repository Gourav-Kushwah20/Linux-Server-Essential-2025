# 🧰 **Smb Client Tools**

This guide covers usage and examples for accessing and managing SMB/CIFS shares on Linux systems (especially RHEL-based distributions).
Tools covered include **smbclient**, **smbtree**, **smbget**, and **smbtar**.

---

# 📦 **Package Management**

---

## 🔍 Check Installed Samba Packages

* Use the following command to list all Samba-related packages currently installed:

```bash
rpm -qa | grep samba
```

---

## 📥 Install Samba Client

* Install the Samba client tools using the package manager:

```bash
yum install samba-client
```

```bash
apt install samba-client
```

---

## ✅ Verify Samba Client Installation

Check if the Samba client is installed:

```bash
rpm -qa | grep samba-client
```

View detailed package information:

```bash
rpm -qi samba-client
```

Display documentation files:

```bash
rpm -qd samba-client
```
List all installed files for the package
```bash
rpm -ql samba-client
```

---

## 🌳 **Browsing Shares With Smbtree**

**smbtree** scans the network and displays SMB shares in a tree format, similar to Windows' network neighborhood.

## ⚠️ **Note On Smbtree Limitations**

1. **main:** This utility doesn't work if NetBIOS name resolution is not configured.
2. If you are using SMB2 or SMB3, network browsing uses WSD/LLMNR, which is not yet supported by Samba.
3. **SMB1 is disabled by default** on the latest Windows versions for security reasons.
---

## 🔍 **Discover Network Shares**

### 📂 Scan and print the SMB workgroup/domain tree:

```bash
smbtree
```

### 📄 List share names only (without computer/user names):

```bash
smbtree -b
```

### 👤 Browse shares using a specific username:

```bash
smbtree -U win7
```

### 🏷️ Show shares in a specified workgroup:

```bash
smbtree -D workgroup
```

### 🌐 List all available workgroups on the network (requires NetBIOS):

```bash
smbtree -D workgroups
```
---

## 📝 **Summary:**

* 🚫 **SMB2/3 no longer uses NetBIOS.**
* 🔍 **WSD/LLMNR (used by Windows for discovery) is not supported by Samba.**
* ⚠️ **SMB1 is insecure and disabled by default in modern systems.**

---

## 🛠️ **Workaround: Access Shares Directly**

### 📄 List shares using UNC format and a username:

```bash
smbclient -L //hostname -U username
```
- Example:
```ini
smbclient -L //192.168.1.51 -U Administrator
Password for [SAMBA\Administrator]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	data            Disk      
	IPC$            IPC       Remote IPC
SMB1 disabled -- no workgroup available

```

### 📁 Mount a share using CIFS (NetBIOS not required):

```bash
mount -t cifs -o username=user,password=pass //192.168.1.x/share /mnt
```

---

## 📂 **Accessing Shares With Smbclient**

**smbclient** is a command-line utility with an FTP-style interface for accessing SMB shares.

---

## 🔎 **Explore Available Shares**

### ▶️ Launch **smbclient** without connecting:

```bash
smbclient
```

### ❓ Display help menu:

```bash
smbclient --help
```

### 🌐 List shares on a remote host (anonymous):

```bash
smbclient -L 192.168.1.51
```

### 📄 List shares using UNC format:

```bash
smbclient -L //192.168.1.51
```

---

### 👤 **List shares as a specific user:**

```bash
smbclient -L //192.168.1.51 -U win11
```

### 🧑‍🦳 **Guest (anonymous) access:**

```bash
smbclient -L 192.168.1.51 -U "" -N
```

---

## 🔗 **Connect To A Share**

### 🕶️ Anonymous access to a share:

- In Window `data` folder properties and >tools > share > user add > share.

```bash
smbclient //192.168.1.51/data
```

### 🔐 Authenticated access:

```bash
smbclient //192.168.1.51/data -U Administrator
```
```bash
┌──(root㉿ns1)-[~/Pictures]
└─# ls -lh                                       
total 356K
-rw-r--r-- 1 root root    0 Nov 19 10:17 abc.txt
-rw-r--r-- 1 root root 356K Nov 19 10:17 fzputtygen.exe
-rw-r--r-- 1 root root    0 Nov 19 10:17 index
```

```bash
smbclient //192.168.1.51/data -U Administrator
Password for [SAMBA\Administrator]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Nov 19 10:21:54 2025
  ..                                DHS        0  Wed Nov 19 09:55:17 2025
  smb-client                          D        0  Wed Nov 19 10:22:44 2025

		15533823 blocks of size 4096. 4352483 blocks available

smb: \> put abc.txt 
putting file abc.txt as \abc.txt (0.0 kB/s) (average 0.0 kB/s)
smb: \> ls
  .                                   D        0  Wed Nov 19 10:24:40 2025
  ..                                DHS        0  Wed Nov 19 09:55:17 2025
  abc.txt                             A        0  Wed Nov 19 10:24:40 2025
  smb-client                          D        0  Wed Nov 19 10:22:44 2025

		15533823 blocks of size 4096. 4352483 blocks available
smb: \> ls
  .                                   D        0  Wed Nov 19 10:25:21 2025
  ..                                DHS        0  Wed Nov 19 09:55:17 2025
  abc.txt                             A        0  Wed Nov 19 10:24:40 2025
  smb-client                          D        0  Wed Nov 19 10:22:44 2025
  windows.txt.txt                     A        0  Wed Nov 19 10:25:10 2025

		15533823 blocks of size 4096. 4352470 blocks available

smb: \> get windows.txt.txt

┌──(root㉿ns1)-[~/Pictures]
└─# ls    
abc.txt
fzputtygen.exe
index
windows.txt.txt
```
---

## 📂 **Example Smbclient Session**

### ⬇️ Download a single file:

```bash
get runasroot.sh
```

### ⬇️ Download multiple files:

```bash
mget templated-roadtrip.zip VBoxDarwinAdditions.pkg
```

### ⬆️ Upload a single file:

```bash
put mysql80-community-release-el7.3.noarch.rpm
```

### ⬆️ Upload multiple files:

```bash
mput *.rpm
```

---

## 📥 **Download Files With Smbget**

`smbget` is similar to `wget` for SMB. It allows downloading files from SMB URLs.

### 🔹 Download a file using SMB credentials:

```bash
smbget -U Administrator smb://192.168.1.51/data/smb-client/fzstorj.exe
```

---

## 🗄️ **Backup And Restore With Smbtar**

`smbtar` lets you archive or extract entire SMB shares in tar format.

### 📘 Display help information:

```bash
smbtar --help
```

### 📦 Backup a share:

```bash
smbtar -s 192.168.1.51 -x data -u Administrator -p Armour123 -t data.tar -v
```

### 🔧 Options:

* **-s** : SMB server address
* **-x** : Share name
* **-u / -p** : Username and password
* **-t** : Output archive file
* **-v** : Verbose output

---


## 🟩 Mounting SMB Shares With `mount.cifs`

Use the **cifs** kernel module to mount SMB shares as part of the Linux file system.

---

### 📌 Mount to `/mnt` using credentials:

```bash
mount -t cifs -o username=Administrator,password=@rmour123 //192.168.1.51/data /mnt
```
```
ls -lh
```
---

### 📁 Mount to a specific directory:

```bash
mount -t cifs -o username=Administrator,password=@rmour123 //192.168.1.51/data /mnt/d1
```

---

### 🔻 Unmount the share:

```bash
umount /mnt
```

---

## 📝 Notes

### 📦 Install `cifs-utils` for full CIFS support:

```bash
yum install cifs-utils
```

---

### 🔄 Mounts can be made persistent via `/etc/fstab`:
## Use for Startup add

```bash
vim /etc/fstab
```

```bash
//192.168.1.51/data /mnt cifs username=Administrator,password=@rmour123 0 0
```
- Show Error
```bash
mount -a
```
```bash
cd /mnt
```
---

### 🔐 For better security, use a credentials file:

```bash
vim /etc/smb-credentials
```

```bash
username=Administrator
password=@rmour123
```

---

## 📁 Mount using the credentials file:

```bash
mount -t cifs -o credentials=/etc/smb-credentials //192.168.1.51/data /mnt
```

* 🔄 Consider **autofs** for on-demand mounting.
* ⚠️ Avoid **SMB1**; prefer **SMB2** or **SMB3** for performance and security.
* 🛡️ Use version or security parameters if needed:


```bash
mount -t cifs -o username=Armour,password=123,vers=3.0 //192.168.1.51/data /mnt
```

