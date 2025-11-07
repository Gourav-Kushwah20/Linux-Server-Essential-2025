# 🔍 nslookup - DNS Client Tools

`nslookup` (Name Server Lookup) is a network administration tool for querying the Domain Name System (DNS) to obtain domain name or IP address mapping and other DNS records.

---

## 🌐 Basic `nslookup` Commands

### 🧭 Lookup a Domain's IP Address
Use `nslookup` to find the IP address of a domain:
```bash
nslookup google.com
```

---

### 🛰️ Lookup a Domain Using a Specific DNS Server
Query a domain using a specific DNS server:
```bash
nslookup google.com 66.6.64.6
```

---

## 📄 Query Specific DNS Record Types with `nslookup`

### 🅰️ A Record (IPv4)
Query the A record (IPv4 address) of a domain:
```bash
nslookup -type=A google.com
```

---

### 🧬 AAAA Record (IPv6)
Query the AAAA record (IPv6 address) of a domain:
```bash
nslookup -type=AAAA google.com
```
---

### 🛰️ NS Record (Name Server)
Query the name server for a domain:
```bash
nslookup -type=NS google.com
```

---

### 📬 MX Record (Mail Exchange)
Query the mail exchange servers for a domain:
```bash
nslookup -type=MX google.com
```

---

### 🏁 SOA Record (Start of Authority)
Query the SOA record for a domain:
```bash
nslookup -type=SOA google.com
```

---

### 📝 TXT Record (Text)
Query the TXT records for a domain:
```bash
nslookup -type=TXT google.com
```

---

## 🌐 Other Useful Lookups

### 🔹 Lookup a Subdomain
Query a specific subdomain:
```bash
nslookup -type=ANY google.com
```
---

## 🧑‍💻 Interactive `nslookup` Mode

You can use `nslookup` in interactive mode to perform multiple queries without reopening the tool each time.

---

### 🚀 Start Interactive Mode
Launch `nslookup` without any arguments:
```bash
nslookup
``` 
---

### 🧪 Example Interactive Commands

#### 🔍 Query a Specific Domain

```bash
facebook.com
```

#### 🛰️ Set Query Type to NS Records
```bash
set type=ns
```
```bash
facebook.com
```


#### 📝 Set Query Type to TXT Records
```bash
set type=txt
```
```bash
facebook.com
```

#### 🌐 Use a Specific DNS Server
```bash
server 64.6.64.6
```
