# 📥 TFTP-Client

## 🔄 TFTP Transfers Using a Client Like **tftp** or **atftp**

---

## 🛠️ Installing TFTP Client Utilities

### 📌 Install the basic TFTP client:

```bash
yum install tftp
```

```bash
apt install tftp
```

### ⭐ Install the Advanced TFTP client (**atftp**), which supports more features:

```bash
yum install atftp
```

```bash
apt install atftp
```

---

## 💬 Using Interactive TFTP Client

### ▶️ To enter interactive mode, simply run:

```bash
tftp <server-ip>
```

Once inside the TFTP prompt (`tftp>`), you can use various commands.
Here are the two you mentioned:

---

## 🔧 Setting Transfer Mode to Binary

```bash
tftp> binary
```

### 📘 Why it matters:

Setting transfer mode to **binary** (also called *octet mode*) is required when transferring:

* 🖼️ Images
* 📦 Archives
* ⚙️ Executables
* 🧩 Any non-text files

If you transfer binary files in the default ASCII mode (`netascii`),
they may get **corrupted** due to newline conversions.

- chech status
```bash
tftp> status
```
---
Here is the **Markdown version with emojis**, fully recreated from your screenshot:

---

## ⬇️ Testing File Download (GET Operation)

To test file downloading from a TFTP server using the classic **tftp** client:

```bash
tftp <server-ip>
```

Example:

```bash
tftp 192.168.1.34
```

### 🧪 Example interaction:

```bash
tftp> get testfile.txt
```

```bash
tftp> quit
```

---

### 🧵 Or in a one-liner:

```bash
tftp -v <server-ip> -c get testfile.txt
```

```bash
tftp -v 192.168.1.34 -c get lastlog
```

---

## ⬆️ Testing File Upload (PUT Operation)

To upload a file to the TFTP server (**write permission required** on server):

```bash
tftp <server-ip>
```

Example:

```bash
tftp 192.168.1.34
```

### 🧪 Example interaction:

```bash
tftp> put sample.txt
```

```bash
quit
```
---

### ▶️ Or use a one-liner (PUT upload):

```bash
tftp -v <server-ip> -c put sample.txt
```

Example:

```bash
tftp -v 192.168.1.34 -c put sample.txt
```

---

## ⬇️ Downloading a File (GET Operation) using **atftp**

```bash
atftp -g -r messages -l messages 192.168.1.34
```

### 📘 Options explained:

* **-g** : get (download) file from server
* **-r messages** : remote file name on the server
* **-l messages** : local file name to save as
* **192.168.1.34** : TFTP server IP address

---

## ⬆️ Uploading a File (PUT Operation) using **atftp**

```bash
atftp -p -r newfile.txt -l newfile.txt 192.168.1.34
```

### 📘 Options explained:

* **-p** : put (upload) file to server
* **-r newfile.txt** : remote file name to write on server
* **-l newfile.txt** : local file to upload
* **192.168.1.34** : TFTP server IP address

---

## 💡 Additional Tips

### 📢 If you want verbose/debug output:

```bash
atftp --verbose -g -r test.txt -l test.txt 192.168.1.34
```

### ✍️ For uploading

Ensure the **TFTP root directory** on the server has:

* ✔️ write permissions
* ✔️ SELinux not blocking transfers

---
