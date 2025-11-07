# 🧑‍💻 User Deletion in Linux

### 🔹 View last 3 lines of user/group files

```bash
tail -v -n 3 /etc/passwd /etc/shadow /etc/group /etc/gshadow
```
- `tail -n 3` → shows the last 3 lines

- `-v` → prints the filename before output

`Files:`

- `/etc/passwd` → user account info (username, UID, GID, shell)

- `/etc/shadow `→ encrypted passwords + account aging

- `/etc/group `→ group info

- `/etc/gshadow `→ secure group passwords

---

### 🔹 Help for userdel

```bash
userdel -h
```
Shows available options:

- `-r `→ remove home directory and mail spool

- `-f `→ force removal (if user logged in or has processes)

- `-Z `→ remove SELinux user mapping

---

### 🔹 Delete a user (keep files)
```bash
userdel u15
```
- Deletes u15 from `/etc/passwd `& `/etc/shadow`

- Keeps home directory and files
---
### 🔹 Delete a user with home directory

```bash
userdel -r u1
```
Deletes u1, their home directory, and mail spool

Leaves files they own outside `/home/u1` untouched
---
### 🔹 Force delete a user (even if logged in)
```bash
userdel -f -r u1
```
`-f` → kills processes owned by the user if needed

`-r` → removes home and mail spool

⚠️ Very destructive — use with caution

---

### ✅ Summary

`userdel uX `→ remove account, keep files

`userdel -r uX` → remove account + home

`userdel -f -r uX` → force remove, even if logged in

---

# 🧑‍💻 deluser Command (Debian-based systems)

### 🔹 Delete a User (keep home directory)

Removes user u15 without deleting their home directory.

### 🔹 Delete a User and Home Directory
```bash
deluser --remove-home u1
```

Removes user u1 and deletes their home directory and mail spool.

### 🔹 Delete User + Home + All Files
```bash
deluser --remove-home --remove-all-files u16
```

Removes user u16, deletes their home directory, and removes them from all groups.

### 🔹 Remove User from a Group
```bash
deluser u17 developers
```

Removes user u17 from the supplementary group developers.

### 🔹 Force Delete a User
```bash
deluser --force u18
```

Forcefully removes user u18, even if they are currently logged in.