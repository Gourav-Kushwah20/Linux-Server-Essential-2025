# 🧭 DNS-Client-Tools

## 📌 DNS Client Tools Commands
Below are the common commands used to manage and query DNS client tools:

---

### 🔍 Check Installed BIND Packages
Use the following command to list installed BIND packages:
```bash
rpm -qa | grep bind
```
---

### 🔍 Check Installed BIND Utilities
Use this command to list installed `bind-utils` packages:
```bash
rpm -qa | grep bind-utils
```
---

### 📦 Install BIND Utilities
To install BIND utilities using `yum`:
```bash
yum install bind-utils
```
---

### 📁 List Files Installed by bind-utils
List all files installed by the `bind-utils` package:
```bash
rpm -ql bind-utils
```
---

## ⚙️ List Configuration Files for bind-utils
To list configuration files for the `bind-utils` package:
```bash
rpm -qc bind-utils
```

---

## 📚 List Documentation Files for bind-utils
To list documentation files for the `bind-utils` package:
```bash
rpm -qd bind-utils
```

---

## 🛠️ Common DNS Client Tools Installed with bind-utils
These are the commonly used DNS client tools included with `bind-utils`:

| Tool Path             | Description                                           |
|-----------------------|-------------------------------------------------------|
| `/usr/bin/delv`       | DNS lookup and validation utility                     |
| `/usr/bin/dig`        | Query DNS servers for information about hostnames     |
| `/usr/bin/host`       | Simple utility for performing DNS lookups             |
| `/usr/bin/mdig`       | Multiple query version of dig                         |
| `/usr/bin/nslookup`   | Legacy tool to query DNS servers                      |
| `/usr/bin/nsupdate`   | Dynamic DNS update utility                            |

---

## 🧾 DNS Records
DNS records define how domain names are mapped to IP addresses and other resources.

---

### 🅰️ A (Address Mapping) Records
- Specifies an **IPv4** address for a given host.  
- **Example:**  
  ```
  example.com ➝ 192.168.1.1
  ```

---

### 🧬 AAAA (IPv6 Address) Records
- Specifies an **IPv6** address for a given host.  
- **Example:**  
  ```
  example.com ➝ 2001:0db8:ff00:0042:8329
  ```

---

### 🔗 CNAME (Canonical Name) Records
- Specifies an **alias** for another domain name.  
- **Example:**  
  ```
  wp.armourinfosec.com ➝ armourinfosec.wordpress.com ➝ 20.20.20.20
  ```

---

### 🛰️ NS (Name Server) Records
- Specifies an **authoritative name server** for a given domain.  
- **Example:**  
  ```
  example.com ➝ ns1.example.com
  ```

---

### 🔁 PTR (Pointer) Records
- Used for **reverse DNS lookups** (mapping IP addresses to hostnames).  
- **Example:**  
  ```
  192.168.1.1 ➝ example.com
  ```
---

### 🏁 SOA (Start of Authority) Records
Provides authoritative information about a DNS zone, including the primary nameserver and zone serial number.

**Example:**
```dns
example.com. IN SOA ns1.example.com. admin.example.com. (
    2025010111 ; serial
    3600       ; refresh
    7200       ; retry
    1209600    ; expire
    86400      ; minimum TTL
)
```

---

### 💻 HINFO (Host Information) Records
Provides information about a host's hardware and operating system.

**Example:**
```dns
example.com. IN HINFO "Intel i7" "Linux"
```

---

### 📬 MX (Mail Exchanger) Records
Specifies a mail server responsible for receiving email messages.

**Example:**
```dns
example.com. IN MX 10 mail.example.com.
```

---

### 📝 TXT (Text) Records
Holds arbitrary text data for a domain.  
Often used for SPF (Sender Policy Framework) or DKIM (DomainKeys Identified Mail) records.

**Example:**
```dns
example.com. IN TXT "v=spf1 include:_spf.google.com ~all"
```
