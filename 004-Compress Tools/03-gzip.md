# 🗜️ **`gzip` and `gunzip` Commands**

The `gzip` command is used for **🗜️ compressing** files, while `gunzip` is used for **📤 decompressing** `.gz` files.  
`gzip` uses the **Lempel-Ziv (LZ77) compression algorithm**, which makes it **⚡ fast** and **📉 efficient** for reducing file sizes.

---

## 🧰 **`gzip`**

### 🧾 **Syntax**
```bash
gzip [OPTIONS] file
```

### 🔧 **Installing gzip**

```bash
sudo yum install gzip
```

---

### 📌 **Examples**

* **Compress a file 🆕**

```bash
gzip file.txt
```

* **Compress multiple files 📂**

```bash
gzip file1.txt file2.txt file3.txt
```

* **Compress a file while keeping the original (`-k`) 🔒**

```bash
gzip -k file.txt
```

* **Maximum compression (`-9`) 🗜️**

```bash
gzip -9 largefile.log
```

* **Fastest compression (lower ratio) ⚡**

```bash
gzip -1 quickfile.txt
```

* **Test a compressed file for corruption ✅**

```bash
gzip -t file.txt.gz
```

* **Force overwrite an existing `.gz` file 🔁**

```bash
gzip -f file.txt
```

---

## 📤 **`gunzip`**

The `gunzip` command is used to **decompress** `.gz` files 📦➡️📄.

### 🧾 **Syntax**

```bash
gunzip [OPTIONS] file.gz
```

---

### 📌 **Examples**

* **Decompress a `.gz` file 📂**

```bash
gunzip file.txt.gz
```

* **Keep the original compressed file while decompressing (`-k`) 🔒**

```bash
gunzip -k file.txt.gz
```

* **Decompress multiple `.gz` files 📁**

```bash
gunzip file1.txt.gz file2.txt.gz
```

* **Test integrity of a `.gz` file ✅**

```bash
gunzip -t file.txt.gz
```

* **Decompress with verbose output 👀**

```bash
gunzip -v file.txt.gz
```

---

## 📖 **Help & Manual Pages**

For more detailed information 📚:

```bash
gzip --help
```
```
gunzip --help
```

```bash
man gzip
```

```
man gunzip
```