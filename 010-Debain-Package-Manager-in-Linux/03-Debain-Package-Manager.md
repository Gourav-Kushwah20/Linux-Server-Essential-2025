
# 📦 Debian Package Manager (dpkg)

The **Debian Package Manager (dpkg)** is the core low-level tool for handling `.deb` packages in Debian-based systems such as **Ubuntu**, **Kali Linux**, and **Raspberry Pi OS**.  
It works directly with packages, without automatically resolving dependencies, which is why tools like **apt** or **apt-get** are commonly used alongside it.

---

## 🖥️ System Information Commands

🔍 View distribution release information:
```bash
ls -lh /etc/*-release
```
```bash
cat /etc/os-release
```
🖥️ Print system architecture:
```bash
dpkg --print-architecture
```

# 🌐 Installing Nmap using dpkg

## 📥 Step 1: Download the Nmap package
```bash
wget http://http.us.debian.org/debian/pool/main/n/nmap/nmap_7.95+dfsg-3_amd64.deb
```
## ⚙️ Step 2: Install the downloaded package
```bash
dpkg -i nmap_7.95+dfsg-3_amd64.deb
```
> 💡 If you see dependency errors, install the missing packages manually or run:

## 📥 Step 3: Download the Nmap common files(Dependencies)

```bash
wget http://ftp.us.debian.org/debian/pool/main/n/nmap/nmap-common_7.95+dfsg-3_all.deb
```
```bash
dpkg -i nmap-common_7.95+dfsg-3_all.deb
```
or 

```bash
apt install ./nmap_7.95+dfsg-3_amd64.deb
```

## ⚙️ Basic dpkg Commands
## 📥 Installing and Removing

### 📌 Install a .deb package:
```bash
dpkg -i package_name.deb
```

### 🛠️ Fix missing dependencies:
```bash
apt-get install -f
```

###  Remove packages
- Remove a package **and its config files** (purge):
```bash
dpkg --purge <package_name>
```
---

## Querying Packages
### List all installed packages
```bash
dpkg -l
```


### Count total installed packages
```bash
dpkg -l | wc -l
```


### Search for a package in the list
```bash
dpkg -l | grep <package_name>
```


### Check if a package is installed (status)
```bash
dpkg -s <package_name>
```


### Get details of an installed package
```bash
dpkg -p <package_name>
```

### List files installed by a package
```bash
dpkg -L <package_name>
```


### Find which package owns a file
```bash
dpkg -S /path/to/file
# or using dpkg-query
dpkg-query --search '/path/to/file'
#dpkg-query --search /usr/bin/passwd
```
---

### Find Which Package Owns a File
```bash
dpkg -S /path/to/file
dpkg -S /usr/bin/zip

dpkg-query --search '/path/to/file'
dpkg-query --search /usr/bin/zip
```
## Working with .deb Files
### Download a `.deb` file
```bash
wget <package_url>.deb
```

### Show package contents without installing
```bash
dpkg --contents package.deb
dpkg-deb -c package.deb
```

### Extract contents of a package
```bash
dpkg -x package.deb output_directory/
dpkg-deb -xv package.deb /tmp/
```
## Fixing Issues
### Reconfigure an installed package
```bash
dpkg-reconfigure <package_name>
```

### Repair a broken installation
```bash
dpkg --configure -a
```
## Useful Tips for Package Discovery

### Find the binary path
```bash
which <command_name>
```

```bash
which zsh
```

```bash
dpkg -S /usr/bin/zsh
```

```bash
dpkg -s zsh
```
## Locate all related files

```bash
whereis <command_name>
```
