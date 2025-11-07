# Forward-Zone
## Forward Zone Configuration

### Set hostname
- Set the DNS server’s hostname in /etc/hostname.

```bash
vim /etc/hostname
```

Example content:
```bash
ns1.armour.local
```
---
### Edit hosts file
- Add the hostname mapping in `/etc/hosts`:

```bash
vim /etc/hosts
``` 

Example content:

```bash
127.0.0.1   localhost ns1 ns1.armour.local
::1         localhost localhost6 ns1 ns1.armour.local
192.168.1.31 ns1 ns1.armour.local
```
--- 
### Edit DNS zones Configuration
- Edit the `named.rfc1912.zones` file to define the forward zone:

```bash
vim /etc/named.rfc1912.zones
```

Example content:

```bash
/// named.rfc1912.zones:
//
// Provided by Red Hat caching-nameserver package 
//
// ISC BIND named zone configuration for zones recommended by
// RFC 1912 section 4.1 : localhost TLDs and address zones
// and https://tools.ietf.org/html/rfc6303
// (c)2007 R W Franks
// 
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//
// Note: empty-zones-enable yes; option is default.
// If private ranges should be forwarded, add 
// disable-empty-zone "."; into options
// 

zone "localhost.localdomain" IN {
	type master;
	file "named.localhost";
	allow-update { none; };
};

zone "localhost" IN {
	type master;
	file "named.localhost";
	allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
	type master;
	file "named.loopback";
	allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
	type master;
	file "named.loopback";
	allow-update { none; };
};

zone "0.in-addr.arpa" IN {
	type master;
	file "named.empty";
	allow-update { none; };
};
# Edit Below Content in this File
zone "armour.local" IN {
	type master;
	file "forward.armour.local";
	allow-update { none; };
};
```

---

## Create Forward Zone File

### Copy template
Copy the example zone file and modify it under `/var/named`.

```bash
cd /var/named/                          # go to zone directory [web:22]
```
```bash
cp -v named.localhost forward.armour.local   # copy template file [web:22]
```
```bash
vim /var/named/forward.armour.local      # edit forward zone [web:22]
```

### Example content
This sample includes SOA, NS, A, CNAME, MX, and TXT (SPF) records with standard timers.

```bash
# Forward zone file: armour.local
$TTL 1D
@   IN  SOA ns1.armour.local. root.armour.local. (
        20250313 ; serial
        3600     ; refresh
        1800     ; retry
        604800   ; expire
        86400 )  ; minimum

@       IN  NS   ns1.armour.local.
ns1     IN  A    192.168.1.21
armour.local. IN A 192.168.1.21
www     IN  CNAME armour.local.
router  IN  A    192.168.1.1
emp1    IN  A    192.168.1.200
emp2    IN  A    192.168.1.201

; Mail exchange
@       IN  MX   10 mail.armour.local.
mail    IN  A    192.168.1.22

; SPF policy
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

## Set permissions

- set group ownership for the zone file
```bash
chgrp named /var/named/forward.armour.local   # restrict to 'named' group [web:18]
```

-  set secure permissions (rw for owner, r for group)
```bash
chmod 640 /var/named/forward.armour.local     # recommended by RHEL docs [web:18]
```
---
## Restart service
- restart BIND (use named-chroot if applicable)
```bash
systemctl restart named.service               # apply changes safely [web:39]
```
---
## Check configuration
- Check the Configuration for syntax errors:

- validate global named.conf syntax
```bash
named-checkconf                               # checks config syntax only [web:42]
```

- optional targeted checks
```bash
named-checkconf /etc/named.conf               # explicit file check [web:42]
```
```bash
named-checkconf /etc/named.rfc1912.zones      # include file sanity check [web:18]
```
```bash
named-checkconf /etc/named.root.key           # key file syntax check [web:18]
```
---
## Verify forward zone:

- verify the forward zone file:
```bash
named-checkzone armour.local /var/named/forward.armour.local
```
```bash
zone armour.local/IN: loaded serial 20250313
OK
```
--- 

## Firewall Configuration
- Alow DNS traffic Through the firewall:
```bash
ufw allow 53/tcp
```
```bash
ufw allow 53/udp
```
---
### Firewalld:
```bash
firewall-cmd --add-port=53/tcp --permanent
```
```bash
firewall-cmd --add-port=53/udp --permanent
```

```bash
firewall-cmd --reload
```
---
## Test Resolution:

- Test forward DNS resolution with `dig`:
```bash
dig @192.168.1.21 armour.local
```
```bash
dig @192.168.1.21 emp1.armour.local
```

```bash
dig @192.168.1.21 ftp.armour.local
```

```bash
dig mx @192.168.1.21 armour.local
```

- Example output:

```bash
; <<>> DiG 9.16.23-RH <<>> @192.168.1.21 ftp.armour.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 54747
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: af260b576147e667010000006900dae31d8979e398d042ff (good)
;; QUESTION SECTION:
;ftp.armour.local.		IN	A

;; ANSWER SECTION:
ftp.armour.local.	86400	IN	CNAME	armour.local.
armour.local.		86400	IN	A	192.168.1.21

;; Query time: 0 msec
;; SERVER: 192.168.1.21#53(192.168.1.21)
;; WHEN: Tue Oct 28 20:31:55 IST 2025
;; MSG SIZE  rcvd: 103
```
## Explanation

* **zone "armour.local" IN { ... }**
    * This creates a **forward zone** for the **armour.local** domain.
    * The file parameter points to the forward zone file ( **forward.armour.local** ).

---

## Zone Records:

* **SOA** – **S**tart **o**f **A**uthority record.
* **NS** – **N**ameserver record.
* **A** – **IPv4** address record.
* **CNAME** – **C**anonical **Name** / Alias record.
* **MX** – **M**ail e**x**change record.
* **TXT** – **T**e**xt** record for **SPF** configuration.

---

## Test Commands:

* **dig @192.168.1.31** – Queries the DNS server directly.
* **named-checkzone** – Checks the validity of the zone file.

---

This config sets up a **forward zone** for **armour.local** with various resource records (**A**, **CNAME**, **MX**, **TXT**) and proper firewall rules. ✅