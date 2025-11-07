## **Archiving and Compression**

Archiving and compression are essential techniques for managing files efficiently in Linux.

- **Archiving**: The process of combining multiple files and directories into a single file.
- **Compression**: Reducing the file size or archive to save space.

Linux provides several tools for archiving and compressing files, including `zip`, `gzip`, `bzip2`, `tar`, and `7zip`.
Below are the details of these commands and their usage.


## **`zip`**  

To use the zip command in CentOS, 

- follow these steps:- Installing Zip
By default, CentOS does not come with the zip utility installed.

**You can install it using:**

```bash
sudo yum install zip
```

The `zip` command compresses files into a `.zip` archive.  

### **Syntax:**  
```bash
zip [OPTIONS] archive_name.zip file1 file2 ...
```

### **Examples:**  

- **Create a zip archive:**  
   ```bash
   zip my_archive.zip file1.txt file2.txt
   ```

- **Zip a directory recursively:[-r]**  
   ```bash
   zip -r my_archive.zip my_directory/
   ```

- **Add files to an existing archive:**  
   ```bash
   zip -u my_archive.zip newfile.txt
   ```

4. **Password-protect a zip file: (creates a password-protected
file.)**  
   ```bash
   zip -e secure.zip file.txt
   ```

5. **Compress multiple files with maximum compression:**  
   ```bash
   zip -9 best_compression.zip file1.txt file2.txt
   ```

6. **Exclude specific files when zipping: (Excludes all files matching the .log extension
)**  
   ```bash
   zip -r my_archive.zip my_folder -x "*.log"
   ```

7. **Delete a file from an archive: (removes unwanted.txt from my_archive.zip
)**  
   ```bash
   zip -d my_archive.zip unwanted.txt
   ```


## **`unzip`**  

The `unzip` command extracts `.zip` archives.  

### **Syntax:**  
```bash
unzip [OPTIONS] archive_name.zip
```

### **Examples:**  

1. **Extract a zip archive:**  
   ```bash
   unzip my_archive.zip
   ```

2. **Extract to a specific directory:**  
   ```bash
   unzip my_archive.zip -d /path/to/destination/
   ```

3. **List contents of a zip archive: (Show the content in list form)**  
   ```bash
   unzip -l my_archive.zip
   ```

4. **Extract a single file from a zip archive:(In this zip file find file1.txt and unzip the file)**  
   ```bash
   unzip my_archive.zip file1.txt
   ```

5. **Extract files without overwriting existing ones:**  
   ```bash
   unzip -n my_archive.zip
   ```

6. **Force overwrite existing files while extracting:**  
   ```bash
   unzip -o my_archive.zip
   ```

7. **Extract password-protected zip file:**  
   ```bash
   unzip -P mypassword secure.zip
   ```

8. **Exclude specific files when extracting:**  
   ```bash
   unzip my_archive.zip -x "unwanted.txt"
   ```


## **Checking and Repairing Zip Files**  

- **Test integrity of a zip file:**  
  ```bash
  unzip -t my_archive.zip
  ```

- **Fix a corrupted zip file:**  
  ```bash
  zip -FF corrupted.zip --out fixed.zip
  ```

For more details, check the manual:  
```bash
man zip
man unzip
```  