# 📦 **Archiving and Compression**

Archiving and compression are essential techniques for managing files efficiently in Linux 🐧.

- **🗂️ Archiving**: The process of combining multiple files and directories into a single file.
- **🗜️ Compression**: Reducing the file size of files or archives to save disk space.

Linux provides several tools for archiving and compressing files, including  
`zip`, `gzip`, `bzip2`, `tar`, and `7zip`.

---

## 🧷 **`zip`**

To use the `zip` command in **CentOS**, follow these steps 👇

### 🔧 Installing Zip

> ⚠️ By default, CentOS does not come with the `zip` utility installed.

Install it using:
```bash
sudo yum install zip
```

The `zip` command compresses files into a `.zip` archive 📁.

### 🧾 **Syntax**

```bash
zip [OPTIONS] archive_name.zip file1 file2 ...
```

### 📌 **Examples**

1. **Create a zip archive 🆕**

```bash
zip my_archive.zip file1.txt file2.txt
```

2. **Zip a directory recursively (`-r`) 📂**

```bash
zip -r my_archive.zip my_directory/
```

3. **Add files to an existing archive ➕**

```bash
zip -u my_archive.zip newfile.txt
```

4. **Password-protect a zip file 🔐**

```bash
zip -e secure.zip file.txt
```

5. **Maximum compression (`-9`) 🗜️**

```bash
zip -9 best_compression.zip file1.txt file2.txt
```

6. **Exclude specific files (e.g., `.log`) 🚫**

```bash
zip -r my_archive.zip my_folder -x "*.log"
```

7. **Delete a file from an archive 🗑️**

```bash
zip -d my_archive.zip unwanted.txt
```

---

## 📤 **`unzip`**

The `unzip` command extracts `.zip` archives 📦➡️📂.

### 🧾 **Syntax**

```bash
unzip [OPTIONS] archive_name.zip
```

### 📌 **Examples**

1. **Extract a zip archive 📂**

```bash
unzip my_archive.zip
```

2. **Extract to a specific directory 📍**

```bash
unzip my_archive.zip -d /path/to/destination/
```

3. **List contents of a zip archive 📋**

```bash
unzip -l my_archive.zip
```

4. **Extract a single file 🎯**

```bash
unzip my_archive.zip file1.txt
```

5. **Extract without overwriting existing files 🚧**

```bash
unzip -n my_archive.zip
```

6. **Force overwrite existing files 🔁**

```bash
unzip -o my_archive.zip
```

7. **Extract password-protected zip 🔐**

```bash
unzip -P mypassword secure.zip
```

8. **Exclude specific files while extracting 🚫**

```bash
unzip my_archive.zip -x "unwanted.txt"
```

---

## 🛠️ **Checking and Repairing Zip Files**

* **Test integrity of a zip file ✅**

```bash
unzip -t my_archive.zip
```

* **Fix a corrupted zip file 🧩**

```bash
zip -FF corrupted.zip --out fixed.zip
```

---

## 📖 **Manual Pages**

For more detailed information, check the manual pages 📚:

```bash
man zip
```
```
man unzip
```

