# 🧭 Multiple Zones Configuration (BIND)

## 🛠️ Edit DNS Zones Configuration

Edit the `named.rfc1912.zones` file to add (`infosec.local`) zone:

```bash
vim /etc/named.rfc1912.zones
```

Add the following block to define the zone:

```conf
zone "infosec.local" IN {
    type master;
    file "forward.infosec.local";
    allow-update { none; };
};
```

---

## 📁 Create Forward Zone File

### 🔄 Copy Existing Zone File

Use the existing forward zone file as a template:

```bash
cp -v /var/named/forward.armour.local /var/named/forward.infosec.local
```

### ✏️ Edit the New Zone File

Open the new zone file for editing:

```bash
vim /var/named/forward.infosec.local
```

Update the contents as needed. Example:

```dns
$TTL 1D
$ORIGIN infosec.local.
@   IN  SOA ns1.armour.local. root.armour.local. (
        20250313 ; serial
        3600     ; refresh
        1800     ; retry
        604800   ; expire
        86400 )  ; minimum

@       IN  NS   ns1.armour.local.
infosec.local. IN A 192.168.1.21
www     IN  CNAME infosec.local.

; Mail exchange
@       IN  MX   10 mail.armour.local.

; Text record for SPF
@       IN  TXT  "v=spf1 mx a ~all"

; Additional services
ftp         IN  CNAME armour.local.
dev         IN  A    192.168.1.100
fileserver  IN  A    192.168.1.110
db          IN  A    192.168.1.120
test        IN  A    192.168.1.130
vpn         IN  A    192.168.1.140
git         IN  A    192.168.1.150
webapp      IN  A    192.168.1.160
logs        IN  A    192.168.1.170
```

---
## Set Permissions

  * Set the group ownership of the reverse zone file to `named`:
    ```bash
    chgrp named /var/named/forward.infosec.local
    ```
  * Set the permissions to allow the owner (root, typically) to read/write and the `named` group to read the file (`rw-r-----`):
    ```bash
    cchmod 640 /var/named/forward.infosec.local
    ```
---
## Set Permision

- Set permissions for the zone file:
```bash
chmod 640 /var/named/forward.infosec.local
```

```bash
chmod 640 /var/named/forward.infosec.local
```
---
## Validate Configuration

- Check the Configuration:

```bash
named-checkconf /etc/named.conf
```
```bash
named-checkconf /etc/named.rfc1912.zones
```

```bash
named-checkzone infosec.local /var/named/forward.infosec.local 
```
Example:
```bash
zone infosec.local/IN: loaded serial 20250313
OK
```

---
## Restart BIND
- Restart the `named` services:
```bash
systemctl restart named.service
```

---

- Test forward DNS resolution:
```bash
dig infosec.local
```

```bash
dig mx infosec.local
```

```bash
dig txt infosec.local
```
Example:
```bash
; <<>> DiG 9.16.23-RH <<>> infosec.local
;; ANSWER SECTION:
infosec.local.		86400	IN	A	192.168.1.21
```

---

# Setup for ai.local

## 🛠️ Edit DNS Zones Configuration

Edit the `named.rfc1912.zones` file to add (`ai.local`) zone:

```bash
vim /etc/named.rfc1912.zones
```

```conf
zone "ai.local" IN {
    type master;
    file "forward.ai.local";
    allow-update { none; };
};
```

---

## 📁 Create Forward Zone File

### 🔄 Copy Existing Zone File

Use the existing forward zone file as a template:

```bash
cp -v /var/named/forward.infosec.local /var/named/forward.ai.local
```

### ✏️ Edit the New Zone File

Open the new zone file for editing:

```bash
vim /var/named/forward.ai.local
```

Update the contents as needed. Example:

```bash
$TTL 1D
$ORIGIN ai.local.
@   IN  SOA ns1.armour.local. root.armour.local. (
        20250313 ; serial
        3600     ; refresh
        1800     ; retry
        604800   ; expire
        86400 )  ; minimum

@       IN  NS   ns1.armour.local.
ai.local. IN A 192.168.1.23
www     IN  CNAME ai.local.

; Mail exchange
@       IN  MX   10 mail.armour.local.

; Text record for SPF
@       IN  TXT  "v=spf1 mx a ~all"

; Additional services
ftp         IN  CNAME armour.local.
dev         IN  A    192.168.1.100
fileserver  IN  A    192.168.1.110
db          IN  A    192.168.1.120
test        IN  A    192.168.1.130
vpn         IN  A    192.168.1.140
git         IN  A    192.168.1.150
webapp      IN  A    192.168.1.160
logs        IN  A    192.168.1.170
```
---

## Set Permision

- Set permissions for the zone file:
```bash
chgrp named /var/named/forward.ai.local 
```

```bash
chmod 640 /var/named/forward.ai.local 
```
---

## Validate Configuration

- Check the Configuration:

```bash
named-checkconf /etc/named.conf
```
```bash
named-checkconf /etc/named.rfc1912.zones
```
```bash
named-checkzone ai.local /var/named/forward.ai.local 
```
Example:
```bash
zone ai.local/IN: loaded serial 20250313
OK
```

---
## Restart BIND
- Restart the `named` services:
```bash
systemctl restart named.service
```
---
## Test the Setup
- Test forward DNS resolution:
```bash
dig ai.local
```

```bash
dig mx ai.local
```
```bash
dig txt ai.local
```
Example:
```bash
; <<>> DiG 9.16.23-RH <<>> ai.local
;; ANSWER SECTION:
ai.local.		86400	IN	A	192.168.1.21
```
---
## Firewall Configuration

To allow external clients to communicate with the DNS server, you must open **port 53** (the standard port for DNS) for both **TCP** and **UDP** protocols.

### UFW (Uncomplicated Firewall)

For systems using UFW, the commands are:

```bash
ufw allow 53/tcp
```
```bash
ufw allow 53/udp
```

### Firewalld

For systems using Firewalld, the commands are:

```bash
firewall-cmd --add-port=53/tcp --permanent
```
```bash
firewall-cmd --add-port=53/udp --permanent
```
```bash
firewall-cmd --reload
```
