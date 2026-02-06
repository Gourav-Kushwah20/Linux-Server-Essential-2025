# 🐧 Linux Directory Structure
![Image](https://tecadmin.net/wp-content/uploads/2022/06/linux-filesystem-hierarchy.png?utm_source=chatgpt.com)

![Image](https://www.linuxfoundation.org/hs-fs/hubfs/Imported_Blog_Media/standard-unix-filesystem-hierarchy-1.png?height=1001\&name=standard-unix-filesystem-hierarchy-1.png\&width=1817\&utm_source=chatgpt.com)

Linux follows a **Filesystem Hierarchy Standard (FHS)** 📁 that defines where files and directories are located and their purpose.

- Everything in Linux starts from the **root directory `/`**.

---

## 🌳 Root Directory `/`

* The **top-level** directory in Linux
* All files and folders exist **under `/`**
* Think of it as the **base of the tree** 🌲

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

---

## 📂 Important Linux Directories 

### 🔹 `/bin` – Essential User Binaries

* Common commands used by all users
* Examples: `ls`, `cp`, `mv`, `cat`, `bash`

📌 *Required for system to run in single-user mode*

---

### 🔹 `/sbin` – System Binaries 🛠️

* Commands for **system administration**
* Examples: `reboot`, `fsck`, `iptables`

👤 Mostly used by **root user**

---

### 🔹 `/etc` – Configuration Files ⚙️

* System-wide configuration files
* Examples:

  * `/etc/passwd`
  * `/etc/ssh/sshd_config`
  * `/etc/fstab`

❌ No executable binaries here (mostly text files)

---

### 🔹 `/home` – User Home Directories 🏠

* Personal directories for normal users
* Example:

  ```bash
  /home/sohan
  /home/user1
  ```

📁 Stores documents, downloads, desktop files, etc.

---

### 🔹 `/root` – Root User Home 👑

* Home directory of the **root (administrator)** user
* Different from `/home`

---

### 🔹 `/boot` – Boot Loader Files 🚀

* Files required to boot the system
* Contains:

  * Kernel (`vmlinuz`)
  * GRUB files

⚠️ Be careful while modifying this directory

---

### 🔹 `/dev` – Device Files 🔌

* Represents hardware devices as files
* Examples:

  * `/dev/sda` → Hard disk
  * `/dev/tty` → Terminal
  * `/dev/null`

📟 Everything is treated as a **file**

---

### 🔹 `/lib` & `/lib64` – Shared Libraries 📚

* Libraries required by `/bin` and `/sbin`
* Similar to `.dll` files in Windows

---

### 🔹 `/usr` – User System Resources 👥

![Image](https://i.sstatic.net/GOD5o.png?utm_source=chatgpt.com)

Contains applications and utilities used by users:

* `/usr/bin` – User commands
* `/usr/lib` – Libraries
* `/usr/share` – Documentation, icons, man pages

📌 Not user home directory (common confusion)

---

### 🔹 `/var` – Variable Data 🔄

* Data that **changes frequently**
* Examples:

  * `/var/log` → Log files 📜
  * `/var/spool` → Mail, print queues
  * `/var/cache`

---

### 🔹 `/tmp` – Temporary Files ⏳

* Stores temporary data
* Files may be deleted on reboot

🧹 Used by applications and users

---

### 🔹 `/proc` – Process Information 🧠

* Virtual filesystem (not real files)
* Shows system and process info
* Example:

  ```bash
  /proc/cpuinfo
  /proc/meminfo
  ```

📊 Used to inspect system status

---

### 🔹 `/sys` – System Information 🖥️

* Interface to kernel and hardware
* Used by advanced system tools

---

### 🔹 `/mnt` – Temporary Mount Point 📌

* Used to mount filesystems temporarily
* Example:

  ```bash
  mount /dev/sdb1 /mnt
  ```

---

### 🔹 `/media` – Removable Media 💽

* Automatically mounted devices
* Examples:

  * USB drives
  * CDs/DVDs

---

### 🔹 `/opt` – Optional Software 📦

* Third-party or custom applications
* Example:

  ```bash
  /opt/google
  /opt/oracle
  ```

---

### 🔹 `/run` – Runtime Data ⚡

* Stores runtime information
* Recreated on every boot
* Examples: PID files, sockets

---

## ✅ Summary Table

| Directory | Purpose              |
| --------- | -------------------- |
| `/`       | Root of filesystem   |
| `/bin`    | User commands        |
| `/sbin`   | Admin commands       |
| `/etc`    | Config files         |
| `/home`   | User data            |
| `/root`   | Root user home       |
| `/boot`   | Boot files           |
| `/dev`    | Devices              |
| `/lib`    | Libraries            |
| `/usr`    | User programs        |
| `/var`    | Logs & variable data |
| `/tmp`    | Temporary files      |
| `/proc`   | Process info         |
| `/sys`    | Kernel info          |
| `/mnt`    | Manual mounts        |
| `/media`  | Removable media      |
| `/opt`    | Optional software    |

---

📘 **Tip:**

> In Linux, **everything is a file** – including devices, processes, and hardware! 🧩
