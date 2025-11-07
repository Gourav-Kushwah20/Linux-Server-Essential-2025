# 🌐 Linux-Network-Configuration

## 🔧 [Linux Network Configuration](#)
Basic network configuration involves managing **IP addresses, DNS, routing rules, and interface settings** using both commands and configuration files.

---

## 📌 Core Network Commands

| 💻 Command                              | 📝 Description                                                         |
|-----------------------------------------|------------------------------------------------------------------------|
| `nmtui`                                 | 🖥️ Opens a text-based interface for managing network settings.          |
| `hostname`                              | 🏷️ Displays or sets the system hostname.                               |
| `hostname -i` / `hostname -I`           | 🔗 Shows IP address(es) linked to the hostname.                        |
| `ip addr show` / `ip ad`                | 🌍 Displays all network interfaces and assigned IPs.                   |
| `ifconfig`                              | 🕰️ Legacy tool for network interface details (deprecated, but still in use). |
| `cat /etc/resolv.conf`                  | 📜 Shows DNS resolver configuration.                                   |
| `route -n` / `ip route`                 | 🛣️ Lists current routing table and gateways.                           |
| `curl -4 ifconfig.me` / `curl -4 icanhazip.com` | 🌐 Retrieves the system’s public IPv4 address.             |

---
## ⚙️ Static IP Configuration `CENTOS 9`

### 1️⃣ Using NetworkManager (`.nmconnection` files)

✏️ Edit the following file:

```bash
/etc/NetworkManager/system-connections/<connection>.nmconnection
```

### 📄 Example configuration:
```bash
[connection]
id=enp0s3
uuid=2492ff55-8af4-37db-ad3f-354652d4a3b7
type=ethernet
autoconnect-priority=-999
interface-name=enp0s3
timestamp=1758080889

[ethernet]

[ipv4]
address1=192.168.1.21/24 #Edit Ip Address
dns=8.8.8.8;        #add dns 8.8.4.4;
gateway=192.168.1.1
method=manual

[ipv6]
addr-gen-mode=eui64
method=disabled

[proxy]

```
--- 
## 2. Using Using ifcfg Files (CentOs 7 / RHEL 7)

```bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

```bash
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=92a9be99-ddba-4e75-a601-a385f8dcb88f
DEVICE=enp0s3
ONBOOT=yes
IPADDR=192.168.1.36
PREFIX=24
GATEWAY=192.168.1.1
DNS1=8.8.8.8
PROXY_METHOD=none
BROWSER_ONLY=no
```
---

## 3.Assiging Multiple IP Address: (CentOS 7 / RHEL 7)
```bash
vim /etc/NetworkManager/system-connections/enp0s3.nmconnection
```

### edit and add `address1`,`address2`,`address3` IPv4
```bash
[connection]
id=enp0s3
uuid=2492ff55-8af4-37db-ad3f-354652d4a3b7
type=ethernet
autoconnect-priority=-999
interface-name=enp0s3
timestamp=1758082100

[ethernet]

[ipv4]
address1=192.168.1.21/24
address2=192.168.1.22/24
address3=192.168.1.23/24
dns=8.8.8.8;
gateway=192.168.1.1
method=manual

[ipv6]
addr-gen-mode=eui64
method=disabled

[proxy]
```
#### Save and reboot

---
 
## Another Way to Assign to multiple `IPv4` in (CENTOS 7)

```bash
cp -v /etc/sysconfig/network-scripts/ifcfg-enp0s3 /etc/sysconfig/network-scripts/ifcfg-enp0s3:1
```

### Create alias files like: `/etc/sysconfig/network-scripts/ifcfg-enp0s3:1`

```bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3:1
```

```bash
TYPE=Ethernet
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=92a9be99-ddba-4e75-a601-a385f8dcb88f
DEVICE=enp0s3:1
ONBOOT=yes
IPADDR=192.168.1.51
PREFIX=24
GATEWAY=192.168.1.1
DNS1=8.8.8.8
PROXY_METHOD=none
BROWSER_ONLY=no
```

Copy 
```bash
cp -v /etc/sysconfig/network-scripts/ifcfg-enp0s3 /etc/sysconfig/network-scripts/ifcfg-enp0s3:2
```
and `/etc/sysconfig/network-scripts/ifcfg-enp0s3:2`

```bash
BOOTPROTO=static
DEVICE=enp0s3:2
ONBOOT=yes
IPADDR=192.168.1.52
PREFIX=24
```
---

