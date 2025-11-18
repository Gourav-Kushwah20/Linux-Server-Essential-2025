
# 🌐 **FTP-Client-Usage**

## 📘 **FTP Client Usage**

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

## 📝 **Additional Tips**

* 💾 Always use **binary** mode for non-text files to avoid corruption.
* 🌀 Use **wildcards** (e.g., `*.conf`) with `mget` / `mput` / `mdelete` for batch operations.
* 📁 Ensure directory permissions allow uploads/downloads on the server side.
* 🚪 Log out with **bye** or **quit** to end the session.

---

