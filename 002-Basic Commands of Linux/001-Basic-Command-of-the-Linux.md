# 🐧 Basic Linux Commands
---

## 📂 `pwd`

The `pwd` (Print Working Directory) command is used to display the absolute path of the current working directory.

```bash
pwd
```

---

## 📁 `ls`

The `ls` command is used to list the contents of a directory. It provides information about files and directories such as names, permissions, sizes, and timestamps.

### 🔹 Syntax

```bash
ls [options] [directory]
```

### 🔹 Examples

* 📌 List files/directories in current location

  ```bash
  ls
  ```
* 📋 Detailed view

  ```bash
  ls -l
  ```
* 👀 Show hidden files

  ```bash
  ls -a
  ```
* 📊 Human-readable sizes

  ```bash
  ls -lh
  ```
* 🔁 Recursive listing

  ```bash
  ls -R
  ```
* ⏱️ Sort by modification time

  ```bash
  ls -lt
  ```
* 📂 List only directories

  ```bash
  ls -d */
  ```

---

## 📍 `cd`

Changes the current directory.

### 🔹 Syntax

```bash
cd [directory]
```

### 🔹 Examples

* 📂 Go to a specific folder

  ```bash
  cd /home/user/documents
  ```
* ⬆️ Move up one level

  ```bash
  cd ..
  ```
* 🌐 Go to root

  ```bash
  cd /
  ```
* 🔙 Return to previous directory

  ```bash
  cd -
  ```
* 🏠 Go to home directory

  ```bash
  cd ~
  ```

---

## 💻 System Information

## 🗓️ `date`

Displays the current date and time.

```bash
date
```

## ⏳ `time`

Displays execution time of a command.

```bash
time
```

## 📅 `cal`

Displays a calendar for the current month.

```bash
cal
```

## ⏱️ `uptime`

Shows how long the system has been running.

```bash
uptime
```

## 🧠 `uname -a`

Displays system and kernel information.

```bash
uname -a
```

## ⚙️ `lscpu`

Displays CPU architecture details.

```bash
lscpu
```

## 🔌 `lsusb`

Lists USB devices connected to the system.

```bash
lsusb
```

## 💽 `df -h`

Shows disk space usage in a human-readable format.

```bash
df -h
```

---

## 👤 User and Session Information

## 🙋 `whoami`

Displays the currently logged-in user.

```bash
whoami
```

## 👥 `who`

Lists all logged-in users.

```bash
who
```

## 🆔 `id`

Displays user ID and group information.

```bash
id user
```

---

## ⚙️ Process and Memory Information

## 📊 `ps -aux`

Lists all running processes.

```bash
ps -aux
```

## 🔄 `top`

Shows real-time system statistics.

```bash
top
```

## 🧮 `free -h`

Displays memory and swap usage in a human-readable format.

```bash
free -h
```

---

## 📚 Help and Documentation

## 📖 `man`

Displays the manual for a command.

```bash
man [command]
```

**Example:**

```bash
man ls
```

## 📘 `info`

Provides detailed information about a command.

```bash
info [command]
```

**Example:**

```bash
info ls
```

## 🆘 `help`

Displays brief help for built-in commands.

```bash
help [command]
```

**Example:**

```bash
help cd
```

---

## 🔌 System Shutdown and Reboot

## ⛔ `shutdown`

Shuts down or reboots the system.

### 🔹 Syntax

```bash
shutdown [options] [time]
```

### 🔹 Examples

* 🔴 Power off immediately

  ```bash
  shutdown -P now
  ```
* 🔁 Restart immediately

  ```bash
  shutdown -r now
  ```
* ❌ Cancel scheduled shutdown

  ```bash
  shutdown -c
  ```

## 🔄 `reboot`

Reboots the system.

```bash
reboot
```

---

✅ **Tip:** Use `man` or `--help` with any command to explore more options!
