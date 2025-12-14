# 🦑 Squid-Transparent-Proxy

## 🌐 Squid Transparent Proxy

> **A transparent proxy intercepts HTTP/HTTPS traffic *without configuring clients’ browsers*.**
> It requires:
> 🔸 **Router/firewall NAT rules**
> 🔸 **Squid configured for interception**

---

## 🧩 DNS Configuration for Squid Transparent Proxy with BIND

To configure your DNS server using **ISC BIND**, you must update the `named.conf` configuration file and ensure the DNS server is properly set up to:

* Handle DNS queries from your local network
* Forward DNS requests where needed

---

## ✏️ Edit the `named.conf` File

Open the configuration file:

```bash
vim /etc/named.conf
```

---

## 🔧 Update the `named.conf` Configuration

Below is the updated configuration for the `named.conf` file.
This setup configures your DNS server as a **caching-only nameserver** for specific IP ranges and networks.

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
    192.168.2.1;
};

acl mynetwork {
	127.0.0.1;
	192.168.1.0/24;
    192.168.2.0/24;
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

## 📘 Explanation of Key Sections

## 🧾 ACL Definitions

* **`acl ns_ip_add`**
  🔐 Defines a list of IP addresses (e.g., `192.168.2.1`) that are allowed to access the DNS server.
  This improves security by ensuring only trusted IPs can interact with the server.

* **`acl mynetwork`**
  🌐 Defines the network blocks (subnets) allowed to query the DNS server.
  In this case, the local networks **`192.168.1.0/24`** and **`192.168.2.0/24`** are permitted.

---

## ⚙️ Options Section

This section contains general DNS server settings, including listening ports, recursion, and DNSSEC.

* **`listen-on port 53 { ns_ip_add; };`**
  🎧 The DNS server listens on port **53** only for IP addresses defined in `ns_ip_add`.

* **`allow-query { mynetwork; };`**
  🚦 Limits DNS queries to clients within the `mynetwork` ACL.

* 🪵 **`Logging`** 🐞 Configures DNS logging, useful for debugging and monitoring.

* **🌍`zone "." IN`**
  🌐 Root zone configuration using a **hint file** for root DNS servers.

* **`include`**
  ➕ Includes additional configuration files for zones and root keys.

---

## 🔄 Restart the Named Service

After updating the configuration, restart the **named** service to apply changes:

```bash
systemctl restart named.service
```

---

## ✅ Verify the DNS Server Is Running

Check the status of the **named** service:

```bash
systemctl status named.service
```

🧪 Additionally, you can test DNS resolution using:

* `dig`
* `nslookup`

to ensure the DNS server is resolving queries correctly.

---
Here is the **clean Markdown version** of your screenshot — clearly formatted with emojis:

---

## 🦑 Set Up Squid Proxy

First, configure Squid to listen on port **3128** and enable **transparent proxying**.

---

## ✏️ Edit Squid Configuration File

Open the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

---

## ➕ Configure Squid for Transparent Proxy

Add the following lines to ensure Squid listens for transparent proxy connections:

```bash
http_port 3128
http_port 3128 transparent
```

---

## 🔁 Restart Squid to Apply Changes

After modifying the Squid configuration, restart Squid to apply the changes:

```bash
systemctl restart squid.service
```

---

## 🔥 Configure Firewalld

You need to modify your **firewalld** configuration to set up **NAT** and **port forwarding** for the transparent proxy.

---

## 🔀 Enable IP Forwarding

IP forwarding allows the system to route traffic between network interfaces.

### ▶️ Enable IP forwarding immediately:

```bash
sysctl -w net.ipv4.ip_forward=1
```

---

### 📝 Make the change permanent

Edit the sysctl configuration file:

```bash
vim /etc/sysctl.conf
```

Add or ensure the following line exists:

```bash
net.ipv4.ip_forward = 1
```

---

### ✅ Apply the changes:

```bash
sysctl -p
```
---

## 🔥 Configure Firewalld Zones


## 🌐 Add Internet Interface to Public Zone

Identify the interface connected to the internet (e.g., **`enp0s3`**) and add it to the **public** zone:

```bash
firewall-cmd --zone=public --add-interface=enp0s3 --permanent
```

---

## 🏠 Add LAN Interface to Trusted Zone

Identify the LAN interface (e.g., **`enp0s8`**) and add it to the **trusted** zone:

```bash
firewall-cmd --zone=trusted --add-interface=enp0s8 --permanent
```

---

## 🔁 Allow Loopback Traffic

Allow loopback traffic (important for DNS and internal communications):

```bash
firewall-cmd --zone=trusted --add-source=127.0.0.1 --permanent
```

---

## 🌍 Allow DNS and Allow UDP Traffic

Allow DNS and UDP traffic, which is required for Squid to resolve domain names:

```bash
firewall-cmd --zone=public --add-port=53/udp --permanent
```

```bash
firewall-cmd --zone=public --add-service=dns --permanent
```

---

## 🔄 Enable NAT (Masquerading) for Outgoing Connections

Enable masquerading for outgoing traffic.
This allows LAN clients to access the internet via the Squid proxy:

```bash
firewall-cmd --zone=public --add-masquerade --permanent
```
---

## 🔁 Forward HTTP Traffic to Squid

To forward incoming **HTTP traffic (port 80)** from the LAN to **Squid (port 3128)**, use the following commands.


## 🌐 Forward HTTP Traffic from LAN to Squid

```bash
firewall-cmd --zone=trusted --add-forward-port=port=80:proto=tcp:toport=3128 --permanent
```

## 🌍 Forward Incoming HTTP Traffic from Internet to Squid

Ensure incoming HTTP traffic from the internet is redirected to Squid:

```bash
firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=3128 --permanent
```

## ❌ Remove Forward HTTP Traffic to Squid (Optional)

To remove the forwarding rule:

```bash
firewall-cmd --zone=public --remove-forward-port=port=80:proto=tcp:toport=3128 --permanent
```

## 🔄 Reload Firewalld to Apply Changes

After adding all rules, reload **firewalld**:

```bash
firewall-cmd --reload
```

---

## ✅ Verify Configuration

## 🔍 Verify Firewalld Rules

Check if firewall rules were applied correctly:

```bash
firewall-cmd --list-all
```

---

## 🧪 Test the Proxy

### 🌐 Test from a LAN client

Configure the browser to use the Squid proxy:

* **IP Address:** `192.168.2.1`
* **Port:** `3128`

Ensure traffic is intercepted and handled by Squid.

🎉 **You’ve completed the Squid Transparent Proxy setup!**

