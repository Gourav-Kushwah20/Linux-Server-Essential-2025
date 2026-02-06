# 🗜️ **`bzip2` and `bunzip2` Commands**

The `bzip2` command compresses files using the **🧠 Burrows–Wheeler** compression algorithm, which generally provides **better compression ratios** than `gzip` 📉.  
The `bunzip2` command is used to **📤 decompress** `.bz2` files.

---

## 🧰 **`bzip2`**

### 🧾 **Syntax**
```bash
bzip2 [OPTIONS] file
```

---

### 📌 **Examples**

* **Compress a file 🆕**

```bash
bzip2 file.txt
```

* **Compress multiple files 📂**

```bash
bzip2 file1.txt file2.txt file3.txt
```

* **Compress a file while keeping the original (`-k`) 🔒**

```bash
bzip2 -k file.txt
```

* **Maximum compression (`-9`) 🗜️**

```bash
bzip2 -9 largefile.log
```

* **Fastest compression (lower ratio) ⚡**

```bash
bzip2 -1 quickfile.txt
```

* **Test a compressed file for corruption ✅**

```bash
bzip2 -t file.txt.bz2
```

* **Force overwrite an existing `.bz2` file 🔁**

```bash
bzip2 -f file.txt
```

---

## 📤 **`bunzip2`**

The `bunzip2` command is used to **decompress** `.bz2` files 📦➡️📄.

### 🧾 **Syntax**

```bash
bunzip2 [OPTIONS] file.bz2
```

---

### 📌 **Examples**

* **Decompress a `.bz2` file 📂**

```bash
bunzip2 file.txt.bz2
```

* **Keep the original compressed file while decompressing (`-k`) 🔒**

```bash
bunzip2 -k file.txt.bz2
```

* **Decompress multiple files at once 📁**

```bash
bunzip2 file1.txt.bz2 file2.txt.bz2
```

* **Test integrity of a `.bz2` file ✅**

```bash
bunzip2 -t file.txt.bz2
```

* **Decompress with verbose output 👀**

```bash
bunzip2 -v file.txt.bz2
```

---

## 📖 **Help & Manual Pages**

For more detailed information 📚:

```bash
bzip2 --help
bunzip2 --help
```

```bash
man bzip2
```
```
man bunzip2
```
