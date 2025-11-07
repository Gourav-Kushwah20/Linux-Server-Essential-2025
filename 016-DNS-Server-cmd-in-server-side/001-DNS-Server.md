# DNS Server Types
---
## ❌ Non-Authoritative (Recursive) Nameserver
Non-Authoritative (Recursive) nameserver are responsible for resolving DNS queries by contacting other nameservers to find the answer. They do **not** store original DNS records but rely on other sources.

### 🧠 Caching Nameserver
Caching Nameserver temporarily Stores DNS results temporarily to improve resolution time and reduce load on authoritative servers.

**Example:** Command to test DNS resolution using a caching nameserver.
```bash
dig example.com @8.8.8.8
```
---

### 🔁 Forwarding Nameserver
Forwarding Nameserver recieve DNS query and pass DNS queries to another nameserver (usually caching or authoritative) for resolution  .

**Example:** command to Configure a Forwarding Nameserver 
```bash
echo "nameserver 8.8.8.8" >> /etc/resolve.conf
```
---

## ✅ Authoritative Nameserver Configuration

Authoritative nameservers store and provide responses for DNS queries about domains they are responsible for. They hold the actual DNS records.

---

## 🧷 Primary Nameserver

The primary nameserver is the main source for DNS zone data and is responsible for updates and changes to DNS records.

**Example of a (`named.conf`) configuration for a primary nameserver:**
```conf
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";
};
```

---

## 🧯 Secondary Nameserver

The secondary nameserver obtains its zone data from the primary nameserver and acts as a backup in case the primary is unavailable.

**Example of a (`named.conf`) for a secondary nameserver:**
```conf
zone "example.com" {
    type slave;
    file "/etc/bind/db.example.com";
    masters { 192.168.1.1; };
};
```