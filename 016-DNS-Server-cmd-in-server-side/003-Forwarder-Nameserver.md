# Forwarder-Nameserver

## Edit Configuration for Forwarders
- Edit The `named.conf` File:

```bash
vim /etc/named.conf
```

```bash
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//
acl ns_ip_add {
	127.0.0.1;
	192.168.1.21;
	192.168.1.22;
	192.168.1.23;
	192.168.1.24;
};

acl mynetwork {
	127.0.0.1;
	192.168.1.0/24;
};

options {
	listen-on port 53 { ns_ip_add; };
	listen-on-v6 port 53 { ::1; };
	directory 	"/var/named";
	dump-file 	"/var/named/data/cache_dump.db";
	statistics-file "/var/named/data/named_stats.txt";
	memstatistics-file "/var/named/data/named_mem_stats.txt";
	secroots-file	"/var/named/data/named.secroots";
	recursing-file	"/var/named/data/named.recursing";
	allow-query     { localhost; mynetwork; };

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
	forwarders {
        	8.8.8.8;
        	8.8.4.4;
   	 };

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

## ✅ Explanation of Forwarders

-  **forwarders { 8.8.8.8; 8.8.4.4; };**

	* This section tells the DNS server to forward queries it can't resolve locally to Google's public DNS servers (`8.8.8.8` and `8.8.4.4`).
	* Forwarders improve resolution time and efficiency by caching frequently used queries.
	
---

# 🔁 Restart Services

## Restart BIND Service

```bash
systemctl restart named.service
```

---

## ✅ Test DNS Queries

Use `dig` to test DNS queries:

```bash
dig google.com
```

---

## ✅ Capture DNS Traffic Using `tcpdump`

Capture DNS traffic on port 53 using `tcpdump`:

```bash
tcpdump -t udp
```
```bash
tcpdump
```
```bash
tcpdump
```
```bash
tcpdump -t -i enp0s3 port 53
```
```bash
tcpdump -n -t udp -i enp0s3 port 53
```

---

This setup configures a caching DNS server with forwarding enabled — useful for speeding up DNS resolution and reducing external DNS queries. 🚀
