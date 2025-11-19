
# 🌐 **FTP-Client-Usage**

## 📘 **FTP Client Usage**
![alt text](img/image-2.png)
This guide provides a step-by-step walkthrough of using the **ftp** command-line client to:

* Connect to an FTP server
* Transfer files
* Switch modes
* Manage remote files

---

## 🔗 **Connect to an FTP Server**

### ▶️ To begin an FTP session, use:

```bash
ftp 192.168.1.21
```

---

## 🔑 **Login Credentials**

When prompted, enter a valid username and password.
For **anonymous access**, use one of these usernames:

* ftp
* anonymous
* anon

Use an **email-style string** as the password.

Example:

```
Username: ftp
Password: aaa.com
```

---

## 🧭 **Available FTP Commands**

### 📘 **List All FTP Commands**

```bash
?
```

Displays a list of all FTP commands supported by the client.

---

### 📂 **View Current Remote Directory**

Shows the present working directory on the remote FTP server:

```bash
pwd
```

---

### 📁 **List Files and Directories**

Displays the contents of the current directory on the remote server:

```bash
ls
```

---

## ⬇️ **Downloading Files From FTP Server**

## 📄 **Download a Single File**

```bash
get about.html
```

---

## 📥 **Download Multiple Specific Files**

```bash
mget contact.html index.html products.html
```

Fetches the listed files from the server.

---

## 🌀 **Download with Wildcard Patterns**

```bash
mget *.css
```

```bash
mget *.conf
```

Downloads all `.css` or `.conf` files in the current directory.

---

## ⬆️ **Uploading Files to Server**

Before uploading, switch to **binary mode** to prevent file corruption.

```bash
binary
```

---
## Before we Upload check the Configuration file:
```bash
vim /etc/vsftpd/vsftpd.conf
```
```apache
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
```bash
grep -v '#' /etc/vsftpd/vsftpd.conf
```
```bash
systemctl restart vsftpd.service
```

---
## 📄 **Upload a Single File**

Sends the specified file to the FTP server: (/opt/share)

```bash
put chisel
```

---

## 📤 **Upload Multiple Files**

Transfers multiple files or matches based on a pattern:

```bash
mput tuned.log.1 tuned.log hello.py
```

Wildcard example:

```bash
mput *.log
```

---

## 🔄 **Switching Transfer Modes**

## 🔤 **Switch to ASCII Mode**

```bash
ascii
```

**Response:**

```bash
200 Switching to ASCII mode.
```

Used for transferring plain text files.

---

## 💾 **Switch to Binary Mode**

```bash
binary
```
- 200 Switching to Binary mode.
---

## 🗑️ **Deleting Remote Files**

### ❌ Delete a Single File

Removes the specified file from the FTP server:

```bash
delete nc64.exe
```

---

### 🧹 Delete Multiple Files

Deletes multiple files in one go.
```bash
mdelete ScreamingFrogSEOSpider-21.3.exe httrack-3.49.2.exe httrack_x64.3.49.2.exe
```


---

## 🔚 **Ending the FTP Session**

```bash
bye
```

or

```bash
quit
```

End the FTP session and closes the connection to the server.

---

# 📘 **vsftpd Configuration Options Explained**

| **Directive**                 | **Description**                                                                                                  |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `anonymous_enable=YES`        | Allows anonymous users to log in to the FTP server using the username **anonymous**.                             |
| `local_enable=YES`            | Enables login for local system users (accounts available on the server).                                         |
| `write_enable=YES`            | Allows commands that modify the filesystem, such as file uploads, deletions, and directory creation.             |
| `local_umask=022`             | Sets the default permission mask for local users. Files are created with `644` and directories with `755`.       |
| `anon_upload_enable=YES`      | Permits anonymous users to upload files (requires proper write permissions on upload directory).                 |
| `anon_mkdir_write_enable=YES` | Allows anonymous users to create new directories.                                                                |
| `dirmessage_enable=YES`       | Displays contents of a `.message` file to users when entering a directory—useful for directory-specific notices. |
| `xferlog_enable=YES`          | Enables logging of all file upload and download activities.                                                      |
| `connect_from_port_20=YES`    | Ensures active mode data connections originate from standard FTP data port (20).                                 |
| `xferlog_std_format=YES`      | Uses standard FTP xferlog format for log files, compatible with most log analyzers.                              |
| `listen=NO`                   | Disables listening on IPv4 sockets (used when IPv6 is enabled).                                                  |
| `listen_ipv6=YES`             | Enables listening on IPv6 sockets (and IPv4 if supported).                                                       |
| `pam_service_name=vsftpd`     | Specifies the PAM service used for user authentication.                                                          |
| `userlist_enable=YES`         | Enables use of `/etc/vsftpd.user_list` to allow or deny access to specified users.                               |
| `pasv_min_port=55000`         | Sets the lower limit of the passive mode data connection port range.                                             |
| `pasv_max_port=55999`         | Sets the upper limit of the passive mode data connection port range.                                             |
| `pasv_enable=YES`             | Enables passive mode FTP, allowing clients behind NAT/firewalls easier access.                                   |

---

## 📝 **Additional Tips**

* 💾 Always use **binary** mode for non-text files to avoid corruption.
* 🌀 Use **wildcards** (e.g., `*.conf`) with `mget` / `mput` / `mdelete` for batch operations.
* 📁 Ensure directory permissions allow uploads/downloads on the server side.
* 🚪 Log out with **bye** or **quit** to end the session.

---

