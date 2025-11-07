# 🧾 host - DNS Client Tools

The `host` command is a simple DNS lookup utility used to convert hostnames to IP addresses and perform other DNS queries.

---

## 🔍 Basic `host` Commands

### 🌐 Lookup a Domain's IP Address
```bash
host google.com
```

---

## 📄 Query Specific DNS Record Types

### 🛰️ NS Record (Name Server)
Query the name server for a domain:
```bash
host -t ns google.com
```

---

### 📬 MX Record (Mail Exchange)
Query the mail exchange servers for a domain:
```bash
host -t mx google.com
```

---

### 📝 TXT Record (Text)
Query the TXT records for a domain:
```bash
host -t txt google.com
```
---

### 📝 SOA Record (Start of Authority)
Query the SOA records for a domain:
```bash
host -t soa google.com
```
---

### 📝 Query SOA Record Using a Specific DNS Server
Specifiy a Particular DNS server to the query the SOA record:
```bash
host -t soa google.com 64.6.64.6
```