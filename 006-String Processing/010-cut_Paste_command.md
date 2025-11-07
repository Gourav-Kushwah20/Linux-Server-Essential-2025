# ✂️ Cut and 📝 Paste Command Reference in Linux

## 🧾 Cut Command Examples

### Extract specific fields (e.g., IP addresses)
```bash
ifconfig | grep 'inet ' | cut -d ' ' -f 12,13
```
### Index all files on the system
```bash
find / > fileindex.db
```
### Display the full index
```bash
cat fileindex.db
```
### Filter for /var/log/ entries
```bash
cat fileindex.db | grep '/var/log/'
```
### View detailed listing of the root directory
```bash
ls -lh /
```
### Extract second-level directories
```bash
cut -d '/' -f2 fileindex.db | sort -h | uniq
```
### Get unique top-level and second-level directories
```bash
cat fileindex.db | grep '^/' | cut -d "/" -f 1-2 | sort | uniq
```
### Extract up to 4 levels in /var/log paths
```bash
cat fileindex.db | grep '^/var/log' | cut -d "/" -f 1-4 | sort | uniq
```
### Find all .txt files
```bash
grep '.txt$' fileindex.db
```
### Find all .pdf files
```bash
grep '\\.pdf$' fileindex.db
```
### List unique users from running processes (excluding header)
```bash
ps -aux | cut -f1 -d ' ' | sort | uniq | grep -v 'USER'
```
# 📋 Paste Command Examples

### Create firstname.txt
```bash
vim firstname.txt
# Content:
Anikt
Ayush
Dheeraj
Gourav
```

### Create lastname.txt
```bash
vim lastname.txt
# Content:
Barnwal
Raiwal
Chauchan
Joshi
```

### Combine both files line by line using TAB delimiter
```bash
paste firstname.txt lastname.txt
```

### Combine both files using a space as delimiter
```bash
paste -d " " firstname.txt lastname.txt
```
