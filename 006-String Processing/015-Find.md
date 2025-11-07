# Linux `find` Command – Examples with Explanation

The `find` command is used to **search files and directories** in Linux, based on name, type, size, permissions, user, etc.

---

## Syntax
```bash
find [where to start searching from] [expression determines what to find] [-options] [what to find]
```

---

## 🔹 Basic Examples
```bash
find .
```
➡️ Search in the current directory and list all files/directories.

```bash
find . -name passwd
```
➡️ Search for a file named `passwd` in the current directory.

```bash
find / -name passwd
```
➡️ Search for `passwd` starting from root (`/`).

```bash
find / -name X.log
```
➡️ Search for `X.log` file anywhere in the system.

```bash
find / -name root
```
➡️ Search for file/directory named `root`.

```bash
find / -name pas*
```
➡️ Search files starting with `pas` (e.g., passwd, pass.txt).

```bash
find / -name *.log
```
➡️ Search all files ending with `.log`.

```bash
find /etc/ -name *\.conf
```
➡️ Search all `.conf` configuration files in `/etc/`.
 
---

## 🔹 Case Sensitivity
```bash
find / -name passwd
```
➡️ Case-sensitive search.

```bash
find / -iname passwd
```
➡️ Case-insensitive search (matches `passwd`, `Passwd`, `PASSWD`).

```bash
find / -iname ROOT
```
➡️ Finds `ROOT`, `root`, `Root`, etc.

```bash
find / -iname pass* -type f
```
➡️ Search files starting with `pass`.

```bash
find / -iname pass* -type d
```
➡️ Search directories starting with `pass`.

---

## 🔹 File Type Check
```bash
file /usr/share/licenses/passwd/COPYING
```
➡️ `file` command tells the type of file.

---

## 🔹 Redirect Errors
```bash
find / -iname pass\* 2> /dev/null
```
➡️ Ignore permission-denied errors.

---

## 🔹 Search by User
```bash
find / -iname pass* -user root
```
➡️ Files owned by user `root`.

```bash
find / -user sec-learner -type f -name *bash*
```
➡️ Files owned by `sec-learner` with `bash` in name.

**Normal users may get errors**, so redirect:
```bash
find / -user sec-learner -type f -name *bash* 2> /dev/null
```

---

## 🔹 Empty Files & Directories
```bash
find / -empty
```
➡️ Find all empty files/directories.

```bash
find / -type f -empty
```
➡️ Empty files only.

```bash
find / -type d -empty
```
➡️ Empty directories only.

---

## 🔹 Permission Filters
```bash
find / -readable
```
➡️ Find readable files/directories.

```bash
find / -writable
```
➡️ Find writable files/directories.

```bash
find / -executable
```
➡️ Find executable files/directories.

```bash
find / -readable -type f
find / -readable -type d
find / -readable -user sec-learner
```
➡️ Filter readable files, dirs, or by user.

```bash
find / -readable -writable -executable -user sec-learner
```
➡️ Files readable, writable, executable by `sec-learner`.

**Example (shadow file check):**
```bash
find / -readable | grep shadow
```
- Root user → can see `/etc/shadow`.  
- Normal user → likely won’t see it.

---

## 🔹 Specific Permissions
```bash
find / -perm 1777
```
➡️ World-writable sticky bit directories (like `/tmp`).

```bash
find / -perm 755
```
➡️ Directories with `rwxr-xr-x` permissions.

```bash
find / -uid 1000
```
➡️ Files owned by user ID 1000.

```bash
find / -gid 1000
```
➡️ Files owned by group ID 1000.

```bash
find / -perm -o=r
```
➡️ Files world-readable.

```bash
find / -perm -o=rwx
```
➡️ Files world-readable, writable, and executable.

---

## 🔹 Using `-exec`
```bash
find /etc/passwd -exec date \;
```
➡️ Execute `date` command on the file.

```bash
find /etc/passwd -exec id \;
```
➡️ Execute `id` command on the file.

---

## 🔹 Search Depth
```bash
find / -maxdepth 2 -name pass\*
```
➡️ Search up to 2 levels deep.

```bash
find / -maxdepth 3 -name pass\*
```
➡️ Search up to 3 levels deep.