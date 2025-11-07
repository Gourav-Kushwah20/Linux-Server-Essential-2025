# File Finding and Comparison Commands in Linux

## 📌 Locate Command

The **locate** command is used to quickly find files by name using a prebuilt database.

### Install and Setup
```bash
yum install mlocate
updatedb
```

### Usage Examples
```bash
locate -S                                # Show database statistics
ls -lh /var/lib/mlocate/mlocate.db       # Check locate database file
locate message                           # Search for 'message'
locate passwd                            # Search for 'passwd'
locate pass | grep etc                   # Filter results using grep
locate root                              # Search for 'root'
locate -i root                           # Case-insensitive search
locate -i /etc/pas*                      # Wildcard search
locate -r '\.txt'                        # Regex search for .txt files
locate -r '\.conf'                       # Regex search for .conf files
locate -r 'ple$'                         # Regex: files ending with 'ple'
locate -r '\.html'                       # Regex search for .html files
locate -r 'passwd$'                      # Regex: file ending with 'passwd'
```

---

## 📌 Which Command

The **which** command shows the location of executables in the PATH.

### Usage
```bash
which --help                             # Show help
echo $PATH                               # Show PATH variable
/root/.local/bin:/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin

which vim                                # /usr/bin/vim
which nmap                               # not found in PATH
which python                             # /usr/bin/python
which nano                               # /usr/bin/nano
```

---

## 📌 Whereis Command

The **whereis** command locates binaries, source, and man pages for a command.

### Usage
```bash
whereis -h                               # Help
whereis ls                               # ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz
whereis python                           # python: /usr/bin/python /usr/share/man/man1/python.1.gz
whereis php                              # php: (not found)
```

---

## 📌 Comm Command

The **comm** command compares two sorted files line by line.

```bash
comm file1.txt firstname.txt
```

---

## 📌 Diff Command

The **diff** command shows the differences between two files.

```bash
diff -c firstname.txt lastname.txt
```

Example output:
```diff
*** firstname.txt	2025-07-20 19:01:34.258600355 +0530
--- lastname.txt	2025-07-20 19:02:30.354310901 +0530
***************
*** 1,4 ****
! Anikt
! Ayush
! Dheeraj
! Gourav
--- 1,4 ----
! Barnwal
! Raiwal
! Chauchan
! Joshi
```

---

## 📌 Best Way to Compare Files

Use **vimdiff** to open and compare two files side by side:

```bash
vimdiff firstname.txt lastname.txt
```

👉 Opens 2 files for editing with highlighted differences.