# 🕵️‍♂️ `grep` Command in Linux

## 🔍 What is `grep`?

`grep` stands for **Global Regular Expression Print**.  
It is a powerful command-line utility used to **search for text patterns** within files. It prints lines that match the given pattern.

---

## ❓ Why use `grep`?

- 🔎 Search for text in files
- 🧹 Filter command output
- 📁 Audit configuration and system files
- 📊 Count occurrences of strings
- 🤖 Use in scripts for automation

---

## 🌍 Where is `grep` used?

- 🔐 Searching in user files like `/etc/passwd`, `/etc/shadow`
- 📜 Filtering log files (e.g., `/var/log/messages`)
- 🧪 Debugging and monitoring
- 📈 Data extraction and analysis
- 🛠️ Scripting and DevOps

---

## 🧪 Common `grep` Examples

### 🔍 Searches for `root` in `/etc/passwd`. Shows user entries related to the root user.
```bash
grep root /etc/passwd
```
### 🔍 Looks for `sec-learn` in `/etc/passwd`. Checks if a user with that name exists.
```bash
grep sec-learn /etc/passwd
```
### 📁 Searches multiple files for `root.` Helps in user and permission audits.
```bash
grep root /etc/passwd /etc/shadow /etc/group /etc/gshadow
```
### 🧾 Searches for root activity in system logs. Useful for monitoring root access.
```bash
grep root /var/log/messages
```
### 🔤 Case-sensitive search for `Root`. Won’t match `root`.
```bash
grep Root /var/log/messages
```
🔡 Case-sensitive search for `root`. Won’t match `Root` or `ROOT`.
```bash
grep root /var/log/messages
```
🔢 Counts how many times `root` appears in the log file.
```bash
grep root /var/log/messages | wc -l
```
### 🔡🆚🔤 Case-insensitive search. Matches root, Root, ROOT, etc.
```bash
grep -i root /var/log/messages
```
### ⚠️ Redundant use of cat. Use grep -i root /var/log/messages instead.
```bash
cat /var/log/messages | grep -i root
```
### 📊 Filters and counts case-insensitive matches of root.
```bash
cat /var/log/messages | grep -i root | wc -l
```
## 💡 Tips
- Use `-i` to ignore case differences.
- Combine with `wc -l` to count occurrences.
- Avoid unnecessary `cat` usage for performance.

--- 

## 📘 Advanced `grep` Command Usage with Examples & Output Filtering

This section contains advanced `grep` examples commonly used for system administration, log analysis, and command output filtering in Linux.

---

## 🌐 Networking

```bash
ifconfig | grep inet
```
🔎 Extracts lines containing `inet` from the `ifconfig` command.  
Used to **display IP addresses** of network interfaces.

---

## 🧑‍💻 Session Logs from `/var/log/messages`

```bash
grep "Started Session" /var/log/messages
```
🟢 Finds logs with the phrase `"Started Session"`, indicating session starts.

```bash
grep Started\ Session /var/log/messages
```
🟢 Same as above using an **escaped space** instead of quotes.

```bash
grep -E "(sec-learner|root)" /var/log/messages
```
👥 Searches for either `sec-learner` or `root` using **extended regex** (`-E`).

```bash
grep -E "(sec-learner|root)" /var/log/messages | grep Session
```
📋 First finds lines with `sec-learner` or `root`, then filters those containing `Session`.

```bash
grep Session /var/log/messages | grep root
```
🧮 Filters logs that contain both `Session` and `root`. Useful for auditing root user sessions.

---

## 🧠 Session Start Variants

```bash
grep -E "(Started Session | Starting Session)" /var/log/messages
```
📘 Matches both `"Started Session"` and `"Starting Session"` phrases.

```bash
grep -E "(Started Session | Starting Session)" /var/log/messages | grep -v root
```
🚫 Filters the same session logs but **excludes** lines containing `root`.

```bash
grep -E "(Started Session | Starting Session)" /var/log/messages | grep -vE "(root | sec-learner)"
```
🚫📛 Excludes lines with either `root` **or** `sec-learner`. Space-sensitive pattern.

```bash
grep -E "(Started Session | Starting Session)" /var/log/messages | grep -vE "(root|sec-learner)"
```
✔️✅ Same as above but space-free pattern. More accurate and preferred in most cases.

---

## 🔠 Wildcard Matching & Regex

```bash
grep root. /var/log/messages
```
🔤 Matches lines where `root` is followed by **any single character**.

```bash
grep root.. /var/log/messages
```
🔤 Matches lines where `root` is followed by **any two characters**.

```bash
grep "root\.." /var/log/messages
```
📎 Matches `root..` literally (escaped dot `\.`). Useful for patterns like `root..session`.

---

## ✅ Success Keyword Matches

```bash
grep "successfully\." /var/log/messages
```
🎯 Finds lines ending with `"successfully."`.

```bash
grep -v "successfully\.$" /var/log/messages
```
🚫 Excludes lines that **end** with `"successfully."`.

```bash
grep -v "successfully\.$" /var/log/messages | grep successfully
```
📊 Displays lines that contain `"successfully"` but **do not end** with it.

---

## 🗓️ Date-Based Filtering with Anchors

```bash
grep "Jul 19" /var/log/messages
```
📅 Finds all lines from `Jul 19`.

```bash
grep "^Jul 19" /var/log/messages | grep Starting
```
⏱️ Finds lines **starting with `Jul 19`**, then filters lines with `Starting`.

```bash
grep "^Jul 19" /var/log/messages | grep Starting | grep root
```
🔍 Filters further to show logs from `Jul 19` that **start with the date**, contain `Starting`, and mention `root`.

---

## 🧠 Special Regex Symbols Quick Ref

- `^` — Matches the **start** of a line  
- `$` — Matches the **end** of a line  
- `.` — Matches **any single character**  
- `\.` — Matches a **literal dot**

---

## 💡 Pro Tips

- ✅ Use quotes for phrases: `"some phrase"`
- 🔁 Chain `grep` commands for advanced filtering
- 🎯 Use `-E` for **extended regex**
- 🚫 Use `-v` to **exclude matches**
- 🧪 Test patterns before applying to large files

---



