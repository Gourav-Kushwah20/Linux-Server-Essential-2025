# 📦 **`tar` (Tape Archive) Command**

The `tar` command is used for **🗂️ archiving** multiple files and directories into a single file.  
It is commonly combined with compression tools like **`gzip`**, **`bzip2`**, and **`xz`** to create compressed archives such as `.tar.gz`, `.tar.bz2`, or `.tar.xz` 🗜️.

---

## 🧰 **`tar`**

### 🧾 **Syntax**
```bash
tar [OPTIONS] archive_name.tar file1 file2 ...
```

---

## 📌 **Examples**

### 📁 Create Archives

* **Create an uncompressed archive 🆕**

```bash
tar -cf archive.tar file1.txt file2.txt
```

* **Create an archive from a directory 📂**

```bash
tar -cf archive.tar my_folder/
```

---

### 🗜️ Create Compressed Archives

* **With `gzip` (`.tar.gz`)**

```bash
tar -czf archive.tar.gz my_folder/
```

* **With `bzip2` (`.tar.bz2`)**

```bash
tar -cjf archive.tar.bz2 my_folder/
```

* **With `xz` (`.tar.xz`)**

```bash
tar -cJf archive.tar.xz my_folder/
```

---

### 📋 View Archive Contents

* **List contents of a tar archive**

```bash
tar -tf archive.tar
```

---

### 📤 Extract Archives

* **Extract an archive**

```bash
tar -xf archive.tar
```

* **Extract to a specific directory 📍**

```bash
tar -xf archive.tar -C /path/to/destination/
```

* **Extract a `.tar.gz` archive**

```bash
tar -xzf archive.tar.gz
```

* **Extract a `.tar.bz2` archive**

```bash
tar -xjf archive.tar.bz2
```

* **Extract a `.tar.xz` archive**

```bash
tar -xJf archive.tar.xz
```

---

### ➕ Modify Archives

* **Append files to an existing archive**

```bash
tar -rf archive.tar newfile.txt
```

* **Delete a file from an archive 🗑️**

```bash
tar --delete -f archive.tar file1.txt
```

---

### 👀 Show Progress

* **Verbose mode (shows progress while archiving)**

```bash
tar -cvf archive.tar my_folder/
```

---

## 📖 **Help & Manual**

For more detailed information 📚:

```bash
tar --help
```

```
man tar
```