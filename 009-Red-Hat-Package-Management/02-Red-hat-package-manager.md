# 🐧 Red Hat Package Manager (RPM)

## 📌 Overview
**RPM (Red Hat Package Manager)** is a widely used package management system primarily designed for **Red Hat-based Linux distributions** such as **RHEL, CentOS, Fedora**, and others.  
It is also used across various UNIX and Linux systems.  

---

## ✨ Key Features of RPM

- **File Extension:**  
  `.rpm` is the standard file extension used for RPM packages.  

- **Usage:**  
  RPM handles **installing, uninstalling, upgrading, querying, listing, and verifying** packages on Linux systems.  
  RPM packages may contain:
  - Software  
  - Documentation  
  - Source code  

- **RPM File Types:**  
  - **Binary RPM:** Contains a precompiled software application or library.  
  - **Source RPM (SRPM):** Contains the source code and build instructions to compile software locally.  

- **Architecture Types:**  
  - `i386` → for **32-bit systems**  
  - `x86_64` → for **64-bit systems**  
  - `noarch` → for **architecture-independent software**  

- **Package Contents:**  
  - Application binaries or libraries  
  - Documentation (e.g., man pages)  
  - Configuration files  
  - **SPEC file (`.spec`)** → contains metadata, build instructions, and packaging scripts  

---

## 📂 RPM Database
- RPM maintains an **internal database** in `/var/lib/rpm` 
- This database tracks:  
    - Installed packages  
    - Package versions  
    - Metadata  

✅ This database is critical for **package verification, querying, and management**.  
 
--

## 📦 RPM Package Naming Convention

### Example:
  ```bash
  httpd-2.4.6-93.el7.centos.x86_64.rpm
  ```

### Breakdown:
- **httpd** → Package name (Apache HTTP Server)  
- **2.4.6** → Package version  
- **93** → Package release number (incremented for builds/updates)  
- **el7** → Target distribution and version (Enterprise Linux 7)  
- **centos** → Distribution variant (CentOS)  
- **x86_64** → Architecture (64-bit)  
- **.rpm** → File format  

---

## 🔧 Common RPM Commands

## 📌 Querying Packages
- **Check if a package is installed:**
  ```bash
  rpm -q package-name
  #Example
  Command:- rpm -q vim-enhanced 
  #vim-enhanced-8.2.2637-22.el9.x86_64
  ```
- **List of all installed packages:**
  ```bash
  rpm -qa
  ```
- **Search installed packages for `ssh`:**
  ```bash
  Command:- rpm -qa | grep ssh
  #Example:
  rpm -qa | grep ssh
  #libssh-config-0.10.4-13.el9.noarch
  #libssh-0.10.4-13.el9.x86_64
  #openssh-8.7p1-46.el9.x86_64
  #openssh-clients-8.7p1-46.el9.x86_64
  #openssh-server-8.7p1-46.el9.x86_64
  #libssh2-1.11.0-1.el9.x86_64
  ```

- **Check detailed package info:**
  ```bash
  Command:- rpm -qi package-name
  #Example:
  Command:- rpm -qi openssh-server-8.7p1-46.el9.x86_64

  Name        : openssh-server
  Version     : 8.7p1
  Release     : 46.el9
  Architecture: x86_64
  Install Date: Friday 01 August 2025 08:52:18 AM
  Group       : Unspecified
  Size        : 1094720
  License     : BSD
  Signature   : RSA/SHA256, Wednesday 23 July 2025 03:13:36 PM, Key ID 05b555b38483c65d
  Source RPM  : openssh-8.7p1-46.el9.src.rpm
  Build Date  : Tuesday 22 July 2025 09:19:36 PM
  Build Host  : x86-01.stream.rdu2.redhat.com
  Packager    : builder@centos.org
  Vendor      : CentOS
  URL         : http://www.openssh.com/portable.html
  Summary     : An open source SSH server daemon
  Description :
  OpenSSH is a free version of SSH (Secure SHell), a program for logging
  into and executing commands on a remote machine. This package contains
  the secure shell daemon (sshd). The sshd daemon allows SSH clients to
  securely connect to your SSH server.
  ```

- **List files in a package:**
  ```bash
  rpm -ql package-name

  #Example:
  Command:- rpm -ql openssh-server-8.7p1-46.el9.x86_64
  /etc/pam.d/sshd
  /etc/ssh/sshd_config
  /etc/ssh/sshd_config.d
  /etc/ssh/sshd_config.d/50-redhat.conf
  /etc/sysconfig/sshd
  /usr/lib/.build-id
  /usr/lib/.build-id/88
  /usr/lib/.build-id/88/71231ca13092911aa7f422f013b4fecfe46542
  /usr/lib/.build-id/e9
  /usr/lib/.build-id/e9/06fa88746632d54f9c73f2b391a9f33784fcdf
  /usr/lib/systemd/system/sshd-keygen.target
  /usr/lib/systemd/system/sshd-keygen@.service
  /usr/lib/systemd/system/sshd.service
  /usr/lib/systemd/system/sshd.socket
  /usr/lib/systemd/system/sshd@.service
  /usr/lib/sysusers.d/openssh-server.conf
  /usr/libexec/openssh/sftp-server
  /usr/libexec/openssh/sshd-keygen
  /usr/sbin/sshd
  /usr/share/empty.sshd
  /usr/share/man/man5/moduli.5.gz
  /usr/share/man/man5/sshd_config.5.gz
  /usr/share/man/man8/sftp-server.8.gz
  /usr/share/man/man8/sshd.8.gz
  ```

- **List configuration files of a package:**
  ```bash
  rpm -qc package-name
  #Example:
  Command:- rpm -qc openssh-server-8.7p1-46.el9.x86_64
  /etc/pam.d/sshd
  /etc/ssh/sshd_config
  /etc/ssh/sshd_config.d/50-redhat.conf
  /etc/sysconfig/sshd
  ```

- List documentation files of a package:
  ```bash
  rpm -qd package-name
  #Example:
  Commands:- rpm -qd openssh-server-8.7p1-46.el9.x86_64
  /usr/share/man/man5/moduli.5.gz
  /usr/share/man/man5/sshd_config.5.gz
  /usr/share/man/man8/sftp-server.8.gz
  /usr/share/man/man8/sshd.8.gz
  ```

- Verify package integrity:
  ```bash
  rpm -V package-name

  # rpm -V openssh-server
  ```

- Find which package owns a file:
  ```bash
  rpm -qf /etc/passwd
  #Example:- 
  Commands:- rpm -qf /etc/ssh
  openssh-8.7p1-46.el9.x86_64
  ```
---

## 📥 Installing Packages
- First Donwload Package:`httpd`
  ```bash
   wget https://mirror.stream.centos.org/9-stream/AppStream/x86_64/os/Packages/httpd-2.4.62-7.el9.x86_64.rpm
  ``` 
## **📦 RPM Package Check Before Installation (httpd)**

#### 🔍 Step 1: List package file
```bash
ls -lh
```
### output:
```bash
total 48K
-rw-r--r--. 1 root root 46K Aug 21 15:43 httpd-2.4.62-7.el9.x86_64.rpm
```
#### 📑 Step 2: Query package info
```bash
#Query package info
rpm -qpi httpd-2.4.62-7.el9.x86_64.rpm
```
### Output:
- Name        : httpd
- Version     : 2.4.62
- Release     : 7.el9
- Architecture: x86_64
- Install Date: (not installed)
- Group       : Unspecified
- Size        : 60691
- License     : ASL 2.0
- Signature   : RSA/SHA256, Thursday 21 August 2025 03:42:15 PM, Key ID 05b555b38483c65d
- Source RPM  : httpd-2.4.62-7.el9.src.rpm
- Build Date  : Monday 18 August 2025 03:26:38 PM
- Build Host  : x86-04.stream.rdu2.redhat.com
- Packager    : builder@centos.org
- Vendor      : CentOS
- URL         : https://httpd.apache.org/
- Summary     : Apache HTTP Server

**Description :**

The Apache HTTP Server is a powerful, efficient, and extensible
web server.

#### 📂 Step 3: List of all package files
```bash
rpm -ql httpd-2.4.62-7.el9.x86_64.rpm
```
#### Key files include:

- /etc/httpd/conf.modules.d/00-brotli.conf
- /etc/httpd/conf.modules.d/00-systemd.conf
- /usr/lib/.build-id
- /usr/lib/.build-id/12
- /usr/lib/.build-id/12/d4ba3f396a830b5da79b99a160ec14fd7e5e44
- /usr/lib/.build-id/a5
- /usr/lib/.build-id/a5/378c3728dfddd86e426941bea2284576e584af
- /usr/lib/systemd/system/htcacheclean.service
- /usr/lib/systemd/system/httpd.service
- /usr/lib/systemd/system/httpd.socket
- /usr/lib/systemd/system/httpd@.service
- /usr/lib64/httpd/modules/mod_brotli.so
- /usr/lib64/httpd/modules/mod_systemd.so
- /usr/share/man/man5/httpd.conf.5.gz
- /usr/share/man/man8/apachectl.8.gz
- /usr/share/man/man8/fcgistarter.8.gz
- /usr/share/man/man8/htcacheclean.8.gz
- /usr/share/man/man8/htcacheclean.service.8.gz
- /usr/share/man/man8/httpd.8.gz
- /usr/share/man/man8/httpd.service.8.gz
- /usr/share/man/man8/httpd.socket.8.gz
- /usr/share/man/man8/httpd@.service.8.gz
- /usr/share/man/man8/rotatelogs.8.gz
- /usr/share/man/man8/suexec.8.gz

### ⚙ Step 4: List configuration files
```bash
rpm -qc httpd-2.4.62-7.el9.x86_64.rpm
```
### Output:
- /etc/httpd/conf.modules.d/00-brotli.conf
- /etc/httpd/conf.modules.d/00-systemd.conf

### 📘 Step 5: List documentation files
```bash
rpm -qd httpd-2.4.62-7.el9.x86_64.rpm
```
### Output:
- /usr/share/man/man5/httpd.conf.5.gz
- /usr/share/man/man8/apachectl.8.gz
- /usr/share/man/man8/fcgistarter.8.gz
- /usr/share/man/man8/htcacheclean.8.gz
- /usr/share/man/man8/htcacheclean.service.8.gz
- /usr/share/man/man8/httpd.8.gz
- /usr/share/man/man8/httpd.service.8.gz
- /usr/share/man/man8/httpd.socket.8.gz
- /usr/share/man/man8/httpd@.service.8.gz
- /usr/share/man/man8/rotatelogs.8.gz
- /usr/share/man/man8/suexec.8.gz


### ⚙️ Step 5:Check Dependencies of a Package (before installation)
```bash
rpm -qpR httpd-2.4.62-7.el9.x86_64.rpm
```
###  Output:

- /bin/sh
- /bin/sh
- /bin/sh
- /bin/sh
- config(httpd) = 2.4.62-7.el9
- httpd-core = 0:2.4.62-7.el9
- libbrotlienc.so.1()(64bit)
- libc.so.6()(64bit)
- libc.so.6(GLIBC_2.2.5)(64bit)
- libc.so.6(GLIBC_2.4)(64bit)
- libsystemd.so.0()(64bit)
- libsystemd.so.0(LIBSYSTEMD_209)(64bit)
- rpmlib(CompressedFileNames) <= 3.0.4-1
- rpmlib(FileDigests) <= 4.6.0-1
- rpmlib(PayloadFilesHavePrefix) <= 4.0-1
- rpmlib(PayloadIsZstd) <= 5.4.18-1
- rtld(GNU_HASH)
- system-logos-httpd
- systemd
- systemd
- systemd


### 📝 Explanation:

- /bin/sh → Requires shell to execute scripts

- `config(httpd)` → Ensures config files for `httpd` are managed

- `httpd-core` → Depends on the httpd core package

- `libbrotlienc.so.1` → Brotli compression library

- `libc.so.6, libsystemd.so.0 `→ Core system libraries

- `systemd `→ Required for service management

- `rpmlib(...)` → Internal RPM library capabilities

### ⚙️ Step 6:List Licence files included in the packages:
```bash
rpm -qpL httpd-2.4.62-7.el9.x86_64.rpm
```
--- 

### List all installed packages sorted by install date

```bash
rpm -qa --last
```
### Show capabilities provided by openssh-server
```bash
rpm -q --provides openssh-server
```

### Dump detailed information about openssh-server
```bash
rpm -q --dump openssh-server
```

### Query file states for openssh-server
```bash
rpm -qs openssh-server```
```
---
<!-- 
## Installing Packages:
- Install a package:
  ```bash
  rpm -i package.rpm
  ```

- Install with verbose output:
  ```bash
  rpm -ivh package.rpm
  ```

- Upgrade (or install if missing):
  ```bash
  rpm -Uvh package.rpm
  ```

- Install ignoring dependencies ⚠️ (not recommended):
  ```bash
  rpm --nodeps -ivh package.rpm
  ```
---

## 🗑 Removing Packages

- Remove a package:
  ```bash
  rpm -e package-name
  ```

- Remove with verbose output:
  ```bash
  rpm -evh package-name
  ```

- Remove ignoring dependencies ⚠️:
  ```bash
  rpm --nodeps -evh package-name
  ```
--- -->

dpkg

cat /etc/os-release

PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
VERSION_ID="2025.3"
VERSION="2025.3"
VERSION_CODENAME=kali-rolling
ID=kali
ID_LIKE=debian
HOME_URL="https://www.kali.org/"
SUPPORT_URL="https://forums.kali.org/"
BUG_REPORT_URL="https://bugs.kali.org/"
ANSI_COLOR="1;31"

