# ClamAV-Antivirus 🦠

## Clam Antivirus (ClamAV) 🛡️

ClamAV is an open-source antivirus engine designed to detect trojans, viruses, malware, and other malicious threats on Linux systems. This guide covers installation, configuration, updating, and scanning using ClamAV.

---

### 1. Download Test Files 📥

- EICAR provides standard anti-malware test files to safely verify ClamAV functionality:
- [Download from EICAR](https://www.eicar.org/) 🔗

---

### 2. Check Repositories 📦

Before installing ClamAV, ensure the required repositories are enabled:

```bash
yum repolist all
```

---

### 3. Install ClamAV 🛠️ 

Install ClamAV and its related packages:

```bash
yum install clamav*
```

---

### 4. Verify Installation ✅

Check installed ClamAV package details:

```bash
rpm -qi clamav
```

List files installed by ClamAV:

```bash
rpm -ql clamav
```

---

### 5. Updating Virus Database 🔄

- 🗂️ **Install FreshClam to keep ClamAV's virus database updated:**
  ```bash
  yum install clamav-freshclam
  ```

- 🔃 **Run FreshClam manually to fetch the latest virus definitions:**
  ```bash
  freshclam
  ```

*(💡 Consider enabling the FreshClam service for automatic updates.)*

---

### 6. Scanning for Threats 🕵️

- 🧹 **Basic scan:**
  ```bash
  clamscan
  ```

- 📁 **Scan a specific directory:**
  ```bash
  clamscan /root/
  ```

- 🔄 **Recursive scan:**
  ```bash
  clamscan -r /root/
  ```

- 🚨 **Scan directory containing suspected threats:**
  ```bash
  clamscan -r /root/webshells/
  ```
---

### 7. Validate ClamAV with Test Malware Files 🧪

- 🧲 **Download harmless EICAR test files to validate ClamAV:**
  ```bash
  wget --no-check-certificate https://secure.eicar.org/eicar.com
  ```
  ```bash
  wget --no-check-certificate https://secure.eicar.org/eicar.com.txt
  ```
  ```bash
  wget --no-check-certificate https://secure.eicar.org/eicar_com.zip
  ```

- 🔍 **Scan downloaded test files:**
  ```bash
  clamscan eicar.com eicar.com.txt eicar_com.zip
  ```
- 🦠 **Show only infected files:**
  ```bash
  clamscan -ir /root/
  ```
- 📑 **Log infected files to a log file:**
  ```bash
  clamscan -ir /root/ -l /root/clamav.log
  cat clamav.log
  ```

- ⚠️ **Scan and remove infected files (use with caution):**
  ```bash
  clamscan --remove -ir /root/
  ```

- 🗑️ **Remove infections from a specific directory:**
  ```bash
  clamscan --remove -r /root/webshells/
  ```
---