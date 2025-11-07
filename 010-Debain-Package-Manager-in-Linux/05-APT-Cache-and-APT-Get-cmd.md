# 📦 Apt-Cache and apt-get

## 🔍 Apt-cache: Query Package Cache

- 🔎 Search for packages matching a given name or keyword.
```bash
apt-cache search Package-name
```

- 🐍 Search for all packages related to python3.
```bash
apt-cache search python3
```
- 📖 Display detailed information about the nmap package.
```bash
apt-cache show nmap
```
- 🔗 Show the dependencies of the curl package.
```bash
apt-cache depends curl
```
- 🔄 Show the reverse dependencies (which packages need libc6).
```bash
apt-cache rdepends libc6
```
- 📦 Display source package information for openssh.
```bash
apt-cache showsrc openssh
```
- 📌 Show installed and available versions of **nginx**, and the repository priority.
```bash
apt-cache policy nginx
```
---

## ⚙️ apt-get : Package Management

### 📌 Commands with Explanation
- 🔄 Update the local package index from repositories.
```bash
apt-get update
```
- ⬆️ Upgrade installed packages to the latest version (auto-confirm with `-y`).
```bash
apt-get upgrade -y
```
- ♻️ Upgrade packages, removing or installing new ones if required (auto-confirm).
```bash
apt-get full-upgrade -y
```
- 🌐 Install the apache2 web server.
```bash
apt-get install apache2
```
- 🗑️ Remove apache2 but keep its configuration files.
```bash
apt-get remove apache2
```
- 🔥 Remove apache2 along with its configuration files.
```bash
apt-get purge apache2
```
- 🧹 Remove automatically installed packages that are no longer required.
```bash
apt-get autoremove -y
```
- 📥 Download the .deb file for nmap without installing it.
```bash
apt-get download nmap
```
- 📂 Download the source code of openssl.
```bash
apt-get source openssl
```
- 📝 Show the changelog for the bash package.
```bash
apt-get changelog bash
```
- 🔍 Verify package dependencies and check for broken packages.
```bash
apt-get check
```
- 🩹 Fix broken dependencies by forcing installation of missing packages.
```bash
apt-get install -f
```
- 🗑️ Clean up incomplete or partial downloads in the APT cache.
```bash
rm /var/cache/apt/archives/partial/*
```
---