# 🎨 GRC (Generic Colouriser) Installation & Configuration

## 📥 1. Download the Source
```bash
wget https://github.com/garabik/grc/archive/refs/tags/v1.13.tar.gz
```
## 📦 2. Extract the Archive
```bash
tar -xvf v1.13.tar.gz
```
### Check extracted files:
```bash
ls -l
drwxrwxr-x 5 root root  4096 Aug  7  2021 grc-1.13
-rw-r--r-- 1 root root 49224 Sep 13 14:29 v1.13.tar.gz
```
## 🗑️ 3. Remove Archive
```bash
rm -rvf v1.13.tar.gz
```
## 📂 4. Move to /opt
```bash
mv -v grc-1.13 /opt
```
## 🚶 5. Navigate to Directory
```bash
cd /opt/grc-1.13
```
### List contents:
```bash
ls -lh 
total 108K
lrwxrwxrwx 1 root root   16 Aug  7  2021 CHANGES -> debian/changelog
drwxrwxr-x 2 root root 4.0K Aug  7  2021 colourfiles
drwxrwxr-x 4 root root 4.0K Aug  7  2021 contrib
lrwxrwxrwx 1 root root   16 Aug  7  2021 COPYING -> debian/copyright
-rw-rw-r-- 1 root root  619 Aug  7  2021 CREDITS
drwxrwxr-x 3 root root 4.0K Aug  7  2021 debian
-rw-rw-r-- 1 root root  710 Aug  7  2021 _grc
-rwxrwxr-x 1 root root 5.1K Aug  7  2021 grc
-rw-rw-r-- 1 root root 1.5K Aug  7  2021 grc.1
-rwxrwxr-x 1 root root  11K Aug  7  2021 grcat
-rw-rw-r-- 1 root root 2.5K Aug  7  2021 grcat.1
-rw-rw-r-- 1 root root 4.0K Aug  7  2021 grc.conf
-rw-rw-r-- 1 root root  968 Aug  7  2021 grc.fish
-rw-rw-r-- 1 root root 1.8K Aug  7  2021 grc.sh
-rw-rw-r-- 1 root root 1.5K Aug  7  2021 grc.spec
-rw-rw-r-- 1 root root 1.3K Aug  7  2021 grc.spec.old
-rw-rw-r-- 1 root root  965 Aug  7  2021 grc.zsh
-rw-rw-r-- 1 root root 1021 Aug  7  2021 INSTALL
-rwxrwxr-x 1 root root  706 Aug  7  2021 install.sh
-rw-rw-r-- 1 root root 9.6K Aug  7  2021 README.markdown
-rw-rw-r-- 1 root root  12K Aug  7  2021 Regexp.txt
-rw-rw-r-- 1 root root   74 Aug  7  2021 TODO
```

## ⚙️ 6. Run Installer
```bash
./install.sh
```
## 📝 7. Configure GRC

#### Edit configuration file:
```bash
vim /etc/default/grc
```
Set:
```bash
GRC_ALIASES=true
```

#### Alternative:
```bash
echo "GRC_ALIASES=true" >> /etc/default/grc
```
## 📜 8. Copy Main Script
```bash
cp -v /opt/grc-1.13/grc.sh /etc/grc.sh
```

- '/opt/grc-1.13/grc.sh' -> '/etc/grc.sh'

#### View:
```bash
cat /etc/grc.sh
```

## 🖥️ 9. Update Shell Configs
#### 🐚 For Bash
```bash
echo "GRC_ALIASES=true" >> ~/.bashrc
echo '[[ -s "/etc/profile.d/grc.sh" ]] && source /etc/grc.sh' >> ~/.bashrc
```

```bash
source ~/.bashrc
```
## 🐚 For Zsh
```bash
echo '[[ -s "/etc/grc.zsh" ]] && source /etc/grc.zsh' >> ~/.zshrc
```

```bash
source ~/.zshrc
```
