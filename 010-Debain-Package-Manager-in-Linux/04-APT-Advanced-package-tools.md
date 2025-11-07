# 🚀 Advanced Package Tool (APT) Guide

The **Advanced Package Tool (APT)** is the package management system used in Debian and its derivatives (like Ubuntu).  
It allows you to 📦 install, 🔄 update, and ❌ remove software packages easily.

---

## 🖥️ System Information

###  Check basic system details:
- 📌 Shows operating system info (e.g., Debian version, codename).
```bash
cat /etc/os-release
```

- 📌 Displays CPU architecture (e.g., amd64, arm64).
```bash
dpkg --print-architecture
```
---

## 📂 Managing APT Repositories

Repositories tell your system where to download software from.
- 📖 View main repository file.
```bash
cat /etc/apt/sources.list
```
- 📂 List additional repo files.
```bash
/etc/apt/sources.list.d
```
- ✍️ Edit the repository list.
```bash
nano /etc/apt/sources.list
```

**📝 Example `/etc/apt/sources.list`**

```bash
#deb cdrom:[Debian GNU/Linux 13.1.0 _Trixie_ - Official amd64 NETINST with firmware 20250906-10:22]/ trixie contrib main non-free-firmware

deb http://deb.debian.org/debian/ trixie main non-free-firmware
deb-src http://deb.debian.org/debian/ trixie main non-free-firmware

deb http://security.debian.org/debian-security trixie-security main non-free-firmware
deb-src http://security.debian.org/debian-security trixie-security main non-free-firmware

# trixie-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
deb http://deb.debian.org/debian/ trixie-updates main non-free-firmware
deb-src http://deb.debian.org/debian/ trixie-updates main non-free-firmware
```
### 🔑 Explanation:

- **deb**→ Binary packages (precompiled software).

- **deb-src** → Source code packages.

- **main** → ✅ Free & open-source software.

- **non-free-firmware** → ⚙️ Proprietary firmware/drivers.

- **security** → 🔒 Security updates.

- **updates** → 🆕 Latest updates before next release.

---

- 🔑 Install Debian Archive Keyring
```bash
apt install debain-archive-keyring
```
✔️ Ensures APT can verify package authenticity with GPG keys.

⚠️ Without it, you may see NO_PUBKEY errors when updating.

---

## 📦 Basic Package Management (APT Cheatsheet)

- 🔄 Refresh package lists from repositories.
```bash
apt update
```

- ⬆️ Show packages with available updates.
```bash
apt list --upgradable
```

- ⚡ Upgrade installed packages (auto-confirm with -y).
```bash
apt upgrade -y
```

- ♻️ Upgrade with dependency changes (may install/remove packages).
```bash
apt full-upgrade -y
```

- 🖥️ Show OS release info (version, codename).
```bash
cat /etc/os-release
```
---

## 🔍 Installing & Searching Packages
- 📡 Install the nmap package.
```bash
apt install nmap
```
- 📦 Install all packages matching `nmap*`.
```bash
apt install nmap*
```
- 🐍 Make python command point to `python3.`
```bash
apt install python-is-python3
```
- 🔎 Search for packages related to `python`.
```bash
apt search python
```
---

## ❌ Removing Packages
- 🗑️ Remove `nmap` (keep config files).
```bash
apt remove nmap
```

- 📋 Check if nmap is installed.
```bash
dpkg -l | grep nmap
```

- 🌐 Install Apache2 web server.
```bash
apt install apache2
```

- 🗑️ Remove Apache2 (keep configs).
```bash
apt remove apache2
```

- 📋 Verify Apache2 installation status.
```bash
dpkg -l | grep apache2
```

- 🔥 Completely remove Apache2 with configs.
```bash
apt purge apache2 -y
```

- 🧹 Clean up unused dependencies.
```bash
apt autoremove -y
```
- 📋 Double-check if Apache2 is removed.
```bash
dpkg -l | grep apache2
```
---

## 🔁 Reinstalling & Downloading Packages
- 🔄 Reinstall Apache2 package.
```bash
apt reinstall apache2
```
- 📥 Download nmap `.deb` package without installing.
```bash
apt download nmap
```
- 📂 Install a local `.deb` package.
```bash
apt install ./nmap_7.95+dfsg-3_amd64.deb 
```
---
## 🔍 Searching and Package Information (APT)

### 📌 Commands with Explanation

- 🔎 Search for packages related to apache2.
```bash
apt search apache2
```
- 📖 Display detailed information about the `python3` package (version, dependencies, description, etc.).
```bash
apt show python3
```
- 🔗 Show packages that python3 depends on.
```bash
apt depends python3 
```

- 🔄 Show packages that depend on python3 (`reverse dependencies`).
```bash
apt rdepends python3
```

- 📦 Display information about the **source package** for python3.
```bash
apt showsrc python3
```

- 📂 Download the source code for python3 into the current directory.
```bash
apt source python3
```
- 🗂️ List downloaded files with human-readable sizes.
```bash
ls -lh
```
- 📝 Show the changelog of the python3 package.
```bash
apt changelog python3
```
---

## 🧹 Cache and Cleanup (APT)

### 📌 Commands with Explanation
- 🧽 Remove all package files from the local cache (`/var/cache/apt/archives`).
```bash
apt clean
```
- 🗑️ Remove only outdated package files from the cache (those no longer downloadable).
```bash
apt autoclean
```
- 📊 Show the size of the APT cache directory in a human-readable format.
```bash
du -sh /var/cache/apt/archives
```
---
## 🛠️ Fixing Common APT Issues

### 📌 Commands with Explanation
- 🔧 Fix broken dependencies by installing missing or incomplete packages.
```bash
apt --fix-broken install
```
- ⚙️ Reconfigure all unpacked but not fully installed packages.
```bash
dpkg --configure -a
```
- 🩹 Force the installation of missing dependencies (similar to `--fix-broken`).
```bash
apt-get install -f
```
--- 
## 📑 Useful APT List Commands

### 📌 Commands with Explanation

- 📋 List all available and known packages.
```bash
apt list
```
- ✅ List all installed packages on the system.
```bash
apt list --installed
```
- ⬆️ Show packages that have newer versions available.
```bash
apt list --upgradable
```
- 🔢 Count the total number of available packages.
```bash
apt list | wc -l
```
- 🔢 Count the total number of installed packages.
```bash
apt list --installed | wc -l
```
- 📌 Show installed version and available versions of python3 from repositories.
```bash
apt policy python3
```
--- 
## ⚡ Advanced APT Usage

### 📌 Commands with Explanation

- 🧪 Simulate the installation of php without actually installing it (dry run).
```bash
apt install --simulate php
````
- ⛔ Put the nmap package on hold (prevent it from being upgraded).
```bash
apt-mark hold nmap
```
- ✅ Remove the hold from nmap, allowing upgrades again.
```bash
apt-mark unhold nmap
```
- 📋 Show all packages currently on hold.
```bash
apt-mark showhold 
```
---


