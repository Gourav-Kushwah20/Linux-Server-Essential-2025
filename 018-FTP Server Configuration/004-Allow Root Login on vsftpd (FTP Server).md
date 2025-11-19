# 🔐 **Allow Root Login on vsftpd (FTP Server)**

By default, **vsftpd** denies FTP access to privileged users like **root** for security reasons.
However, in controlled environments (labs, VMs, offline systems), you may want to enable root FTP login.

> ⚠️ **Warning:** Allowing root to log in via FTP is a **major security risk**.
> It should **never** be enabled on production or internet-facing servers.
> Proceed **only** in isolated, secure environments.

---

## 📝 **Edit the `/etc/vsftpd/ftpusers` File**

This file lists users who are **always denied** FTP access—even with a valid shell and password.

To allow root login, you must **remove or comment out** the `root` entry.

Open the file:

```bash
vim /etc/vsftpd/ftpusers
```

---

## 🚫 **Original Example (root is blocked):**

```txt
# Users that are not allowed to login via ftp
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
```

---

## ✔️ **Updated Version (root is allowed):**

```txt
# Users that are not allowed to login via ftp
#root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
```

Root is now allowed to log in via FTP.

---
## 📝 **Edit the `/etc/vsftpd/user_list` File**

This file *also* lists users who are denied access by **default**, unless configured otherwise using the `userlist_deny` setting.

To allow **root** to log in via FTP, you must **comment out** the `root` entry here as well.

Open the file:

```bash
vim /etc/vsftpd/user_list
```

---

## 📄 **Original (root is denied):**

```txt
# vsftpd userlist
# If userlist_deny=NO, only allow users in this file
# If userlist_deny=YES (default), never allow users in this file, and
# do not even prompt for a password.
# Note that the default vsftpd pam config also checks /etc/vsftpd/ftpusers
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
```

---

## ✔️ **Updated Version (root allowed):**

```txt
# vsftpd userlist
# If userlist_deny=NO, only allow users in this file
# If userlist_deny=YES (default), never allow users in this file, and
# do not even prompt for a password.
# Note that the default vsftpd pam config also checks /etc/vsftpd/ftpusers
#root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
```

---


## 🔄 **Restart the vsftpd Service**

### ✔️ After making changes, restart vsftpd to apply them:

```bash
systemctl restart vsftpd.service
```

### 🔍 Check its status:

```bash
systemctl status vsftpd.service
```

---

## 🔐 **Optional: Verify Root Shell and Password**

Make sure the **root account has a valid login shell and password**.

### 📌 Check root shell:

```bash
grep root /etc/passwd
```

### ✅ Expected output:

```
root:x:0:0:root:/root:/bin/bash
```

### 🔑 Set (or reset) the root password:

```bash
passwd root
```

---

## 🧪 **Test Root FTP Login**

You can now attempt to log in via FTP using the root account:

```bash
ftp 192.168.1.50
```

### Credentials:

```
Username: root
Password: [your root password]
```

---
