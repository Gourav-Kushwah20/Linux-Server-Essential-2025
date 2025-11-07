# 🧭 Slave-DNS-Server

## `Master` and Slave DNS Server

This guide covers the configuration of a **Master DNS Server** and a **Slave DNS Server** using **BIND** (Berkeley Internet Name Domain).

---

## 🖥️ Server Details

| **Role**    | **IP Address** | **Description**      |
| ----------- | -------------- | -------------------- |
| Master DNS  | 192.168.1.21   | Primary DNS server   |
| Slave DNS   | 192.168.1.22   | Secondary DNS server |
| Domain Name | armour.local   | Example domain       |

---

## ⚙️ Master DNS Configuration

### 🪪 Step 1: Set the Hostname

Set the hostname for the Master DNS server:

```bash
hostnamectl set-hostname ns1.armour.local
```

---

### 📦 Step 2: Install BIND

Install the BIND package:

```bash
yum install bind -y
```

---

### 🧩 Step 3: Configure the DNS Zones

Edit the zone configuration file:

```bash
vim /etc/named.rfc1912.zones
```

add context:
```bash
zone "armour.local" IN {
        type master;
        file "forward.armour.local";
        allow-update { none; };
        allow-transfer { 192.168.1.21; };
};

zone "infosec.local" IN {
        type master;
        file "forward.infosec.local";
        allow-update { none; };
        allow-transfer { 192.168.1.21; };
};

zone "ai.local" IN {
        type master;
        file "forward.ai.local";
        allow-update { none; };
        allow-transfer { 192.168.1.21; };
};

```
Like this:
```bash
// named.rfc1912.zones:
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

zone "armour.local" IN {
        type master;
        file "forward.armour.local";
        allow-update { none; };
        allow-transfer { 192.168.1.21; };
};

zone "infosec.local" IN {
        type master;
        file "forward.infosec.local";
        allow-update { none; };
        allow-transfer { 192.168.1.21; };
};

zone "ai.local" IN {
        type master;
        file "forward.ai.local";
        allow-update { none; };
        allow-transfer { 192.168.1.21; };
};

zone "1.168.192.in-addr.arpa" IN {
	type master;
	file "reverse.armour.local";
	allow-update { none; };
};

```

---

### 🧩 Step 4: Create the Zone files

Create the forward lookup zone file:

### Forward Zone for `armour.local`

```bash
vim /var/named/forward.armour.local
```
Edit:
```bash
@       IN  NS   ns1.armour.local.
@	    IN  NS	 ns2.armour.local.
ns1     IN  A    192.168.1.21
ns2     IN  A    192.168.1.41
```

Example:
``` bash
$TTL 1D
@   IN  SOA ns1.armour.local. root.armour.local. (
        20250313 ; serial
        3600     ; refresh
        1800     ; retry
        604800   ; expire
        86400 )  ; minimum

@       IN  NS   ns1.armour.local.
@	IN  NS	 ns2.armour.local.
ns1     IN  A    192.168.1.21
ns2     IN  A    192.168.1.22
armour.local. IN A 192.168.1.21
www     IN  CNAME armour.local.
router  IN  A    192.168.1.1
emp1    IN  A    192.168.1.200
emp2    IN  A    192.168.1.201

; Mail exchange
@       IN  MX   10 mail.armour.local.
mail    IN  A    192.168.1.201

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
### Forward Zone for `infosec.local`

```bash
vim /var/named/forward.infosec.local
```
Edit Content:
```bash
@    IN   NS ns2.armour.local
```
Like this:
```bash
$TTL 1D
$ORIGIN infosec.local.
@   IN  SOA ns1.armour.local. root.armour.local. (
        20250313 ; serial
        3600     ; refresh
        1800     ; retry
        604800   ; expire
        86400 )  ; minimum

@       IN  NS   ns1.armour.local.
@	IN  NS   ns2.armour.local.
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

### Forward Zone for `ai.local`

```bash
vim /var/named/forward.ai.local
```
Edit Content:
```bash
@    IN   NS ns2.armour.local
```
Like this:
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
@ 	IN  NS   ns2.armour.local.
ai.local. IN A 192.168.1.21
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

---

## Validate Configuration

- Check the Configuration:

```bash
named-checkconf 
```
```bash
named-checkconf /etc/named.conf
```
```bash
named-checkconf /etc/named.rfc1912.zones
```
```bash
named-checkzone armour.local /var/named/forward.armour.local 
```
```bash
named-checkzone infosec.local /var/named/forward.infosec.local 
```

```bash
named-checkzone ai.local /var/named/forward.ai.local
```
- Example:

```bash
zone ai.local/IN: loaded serial 20250313
OK
```
---

## ⚙️ Slave DNS Configuration (Secondary DNS Server)

### 🪪 Step 1: Set the Hostname

Set the hostname for the Master DNS server:

```bash
hostnamectl set-hostname ns2.armour.local
```

---

### 📦 Step 2: Install BIND

Install the BIND package:

```bash
yum install bind -y
```

---

### 🧩 Step 3: Configure `/etc/hosts`

Edit the `/etc/hosts` file:

```bash
vim /etc/hosts
```
```bash
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.41	ns2   ns2.armour.local
```
---
### 🧩 Step 4:Configure BIND `named.conf`

### Edit Content:
```bash
vim /etc/named.conf
```

- Example:
```bash
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
	listen-on port 53 { 127.0.0.1; 192.168.1.41; };
	listen-on-v6 port 53 { ::1; };
	directory 	"/var/named";
	dump-file 	"/var/named/data/cache_dump.db";
	statistics-file "/var/named/data/named_stats.txt";
	memstatistics-file "/var/named/data/named_mem_stats.txt";
	secroots-file	"/var/named/data/named.secroots";
	recursing-file	"/var/named/data/named.recursing";
	allow-query     { localhost; 192.168.1.0/24;};

	/* 
	 - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
	 - If you are building a RECURSIVE (caching) DNS server, you need to enable 
	   recursion. 
	 - If your recursive DNS server has a public IP address, you MUST enable access 
	   control to limit queries to your legitimate users. Failing to do so will
	   cause your server to become part of large scale DNS amplification 
	   attacks. Implementing BCP38 within your network would greatly
	   reduce such attack surface 
	*/
	recursion yes;

	dnssec-validation yes;

	managed-keys-directory "/var/named/dynamic";
	geoip-directory "/usr/share/GeoIP";

	pid-file "/run/named/named.pid";
	session-keyfile "/run/named/session.key";

	/* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
	include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
	type hint;
	file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```
---
### Step4: Configure the Zones on the Slaves
-  Edit
```bash
vim /etc/named.rfc1912.zones
```

Example:
```bash

zone "armour.local" IN {
    type slave;
    masters { 192.168.1.41; };
    file "slaves/forward.armour.local";
};

zone "infosec.local" IN {
    type slave;
    masters { 192.168.1.41; };
    file "slaves/forward.infosec.local";
};

zone "ai.local" IN {
    type slave;
    masters { 192.168.1.41; };
    file "slaves/forward.ai.local";
};
```
---

### Step5: Confgure Firewall Rules(Firewalld)
- Allow DNS Traffic (TCP and UDP Port 53):

```bash
firewall-cmd --permanent --add-port=53/tcp
```
```bash
firewall-cmd --permanent --add-port=53/udp
```
```bash
firewall-cmd --reload
```
---
### Step 6: Validate Configuration
- Check the Configuration:
```bash
named-checkconf 
```

```bash
named-checkconf /etc/named.conf
```
```bash
named-checkconf /etc/named.rfc1912.zones
```
---

### Step7: Restart the Service and Enable `BIND`:
```bash
systemctl restart named.service
```
```bash
systemctl enable named.service
```
Output:-Created symlink /etc/systemd/system/multi-user.target.wants/named.service → /usr/lib/systemd/system/named.service.

---
### Step8: Test DNS Resolution

- Test DNS resolution from the slave:
```bash
dig armour.local @192.168.1.41
```
```bash
dig infosec.local @192.168.1.41
```
```bash
dig ai.local @192.168.1.41
```
---