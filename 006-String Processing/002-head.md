# 🔍 String Processing and Finding Files

## 🧠 `head` Commands

The `head` command is used to display the first few lines or bytes of a file. By default, it shows the first 10 lines.

---
### 📄 Basic Usage
```bash
cat /etc/passwd
```
### Displays the entire content of `/etc/passwd.`
```bash
head /etc/passwd
```

### Shows the first 10 lines of `/etc/passwd.`
```bash
head -n 5 /etc/passwd
```
Displays the first 5 lines of /etc/passwd.
### 📁 Multiple Files
Displays the first 10 lines of each listed file.
```bash
head /var/log/messages /etc/passwd /etc/shadow /etc/gshadow /etc/group
```
Shows the first 4 lines from each specified file.

```bash
head -n 4 /var/log/messages /etc/passwd /etc/shadow /etc/gshadow /etc/group
```
### 🔍 Verbose Output
```bash
head -v /etc/passwd
```
Includes the filename in the output header, useful when reading from multiple files.
### 🔢 Character Count Instead of Lines
```bash
head -c 50 /etc/passwd
```
Outputs the first 50 characters of /etc/passwd.

```bash
head -c 4 /etc/passwd /etc/group /etc/shadow
```
Outputs the first 4 characters from each file listed.