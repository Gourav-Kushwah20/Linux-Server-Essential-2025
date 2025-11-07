# Reverse Zone Configuration

## Edit DNS Zone Configuration

- Edit the `named.rfc1912.zones` file to define the reverse zone:
```bash
vim /etc/named.rfc1912.zones
```
Example content:
```bash
# Edit bottom on the lines
zone "1.168.192.in-addr.arpa" IN {
	type master;
	file "reverse.armour.local";
	allow-update { none; };
};
```
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

# Edit Now 
zone "1.168.192.in-addr.arpa" IN {
	type master;
	file "reverse.armour.local";
	allow-update { none; };
};
```
---

## Create Reverse zone file
```bash
cd /var/named/
```
- Copy the forward zone file and modify it for reverse mapping:
```bash
cp -v /var/named/forward.armour.local /var/named/reverse.armour.local
```
```bash
vim /var/named/reverse.armour.local
```
- Example content:
```bash
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

It is crucial to set the correct ownership and permissions so the BIND service (`named`) can read the zone file securely:

  * Set the group ownership of the reverse zone file to `named`:
    ```bash
    chgrp named /var/named/reverse.armour.local
    ```
  * Set the permissions to allow the owner (root, typically) to read/write and the `named` group to read the file (`rw-r-----`):
    ```bash
    chmod 640 /var/named/reverse.armour.local
    ```

## Restart BIND Service

The DNS service must be restarted to load the new zone configuration and file:

  * Restart the `named` service:
    ```bash
    systemctl restart named.service
    ```

## Check Configuration

  * Check the configuration for syntax errors:
    ```bash
    named-checkconf /etc/named.conf
    ```

## Verify Reverse Zone

  * Verify the reverse zone file, providing the zone name (`1.168.192.in-addr.arpa`) and the file path:
    ```bash
    named-checkzone 1.168.192.in-addr.arpa /var/named/reverse.armour.local
    ```
  * **Output example:** A successful check will show an "OK" message:
    ```
    zone 1.168.192.in-addr.arpa/IN: loaded serial 20250313
    OK
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

## Test Reverse Resolution

The **`dig`** utility is used to test **reverse DNS resolution**, which maps an IP address back to a hostname. The `-x` flag specifies a reverse lookup.

  * Test reverse DNS resolution using `dig`:
    ```bash
    dig -x 192.168.1.21
	```
	```bash
    dig -x 192.168.1.200
    ```
- Example Output
A successful reverse lookup (PTR record query) will show the hostname in the ANSWER SECTION.

```bash
; <<>> DiG 9.16.23-RH <<>> -x 192.168.1.21
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 31546
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 61942233edd18675010000006900dd9f76fc211005ff9da3 (good)
;; QUESTION SECTION:
;21.1.168.192.in-addr.arpa.	IN	PTR

;; AUTHORITY SECTION:
1.168.192.in-addr.arpa.	86400	IN	SOA	ns1.armour.local. root.armour.local. 20250313 3600 1800 604800 86400

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Oct 28 20:43:35 IST 2025
;; MSG SIZE  rcvd: 139

```
This output confirms that the IP address 192.168.1.100 correctly resolves back to the hostname dev.armour.local.

---
### Explanation

- `zone "1.168.192.in-addr.arpa" IN { ... }`

	- This creates a reverse zone for the `192.168.1.x` network.

	- The file parameter points to the reverse zone file (`reverse.armour.local`).

- PTR Records:

	- PTR – Maps an IP address to a hostname.

- Test Commands:

	- `dig -x 192.168.1.100` – Queries the DNS server for reverse resolution.

	- `named-checkzone` – Checks the validity of the reverse zone file.