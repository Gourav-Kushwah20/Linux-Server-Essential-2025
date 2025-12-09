# 🔐 Secure Copy (`scp`)

`scp` uses SSH to transfer files securely between systems.

---

## 📤 Copy a Single File to Remote

Upload a file to a remote server's directory:

```bash
scp file.txt root@192.168.1.33:/root/
```

```bash
scp ftp-server.py root@192.168.1.33:/tmp/
```

---

## 📤 Copy Multiple Files to Remote

Transfer several files at once:

```bash
scp file1.txt file2.txt root@192.168.1.33:/root/
```

```bash
scp image.png notes.md root@192.168.1.33:/var/www/html/
```

---

## 📁 Copy a Directory Recursively

Use `-r` to copy entire directories and their contents:

```bash
scp -r /local/dir root@192.168.1.33:/remote/dir
```

```bash
scp -r ~/Documents root@192.168.1.33:/tmp/
```

---

## 📥 Copy File from Remote to Local

Download files from a remote machine to your local system:

```bash
scp -r root@192.168.1.33:/root/file.txt /local/dir/
```

```bash
scp -r root@192.168.1.33:/var/www/html/ /tmp/
```
Limit bandwidth to avoid saturating the network.

```bash
scp -l 500 file.zip root@192.168.1.33:/tmp/
```

