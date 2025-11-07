# Firewalld 🔥

**Firewalld** is a firewall management tool for Linux that provides a dynamic way to configure and manage firewall rules. It is a modern replacement for **iptables** and is commonly used in Red Hat-based distributions like RHEL, CentOS, Fedora, and Rocky Linux.

---

## Key Features of Firewalld ✨
- 🛡️ **Zone-Based Management**: Assign different security levels to network interfaces.
- ⚡ **Dynamic Firewall**: Change rules without dropping active connections.
- 🔌 **D-Bus Interface**: Services and apps can request firewall changes dynamically.
- 🌐 **IPv4 & IPv6 Support**: Works with both protocols.
- 🧩 **Rich Rules**: More granular control than iptables.
- 📝 **Easy Commands**: Uses `firewall-cmd` for management.

---

## Checking Firewalld Status 🔍
- Check if firewalld is installed:
  ```bash
  rpm -qa | grep firewalld
  ```
- Display package info:
  ```bash
  rpm -qi firewalld
  ```
- List installed files:
  ```bash
  rpm -ql firewalld
  ```
- List config files:
  ```bash
  rpm -qc firewalld
  ```
- List documentation files:
  ```bash
  rpm -qd firewalld
  ```
- Check service status:
  ```bash
  systemctl status firewalld
  ```
- Disable and stop iptables:
  ```bash
  systemctl disable iptables
  systemctl stop iptables
  ```
- Enable and start firewalld:
  ```bash
  systemctl enable firewalld
  systemctl start firewalld
  ```
- Verify status again:
  ```bash
  systemctl status firewalld
  ```

---

## Installing and Managing Firewalld 🛠️
- Install firewalld:
  ```bash
  yum install firewalld -y
  ```
- Restart firewalld:
  ```bash
  systemctl restart firewalld
  ```
- Reload firewalld to apply changes:
  ```bash
  firewall-cmd --reload
  ```

---

## Listing Firewalld Rules 📋
- Show active zones:
  ```bash
  firewall-cmd --get-active-zones
  ```
- List all rules for default zone:
  ```bash
  firewall-cmd --list-all
  ```

---

## Adding Firewall Rules ➕
- Allow FTP (port 21):
  ```bash
  firewall-cmd --permanent --add-port=21/tcp
  ```
- Allow HTTP (port 80):
  ```bash
  firewall-cmd --permanent --add-port=80/tcp
  ```
- Allow HTTPS (port 443):
  ```bash
  firewall-cmd --permanent --add-port=443/tcp
  ```
- Allow SSH (port 22):
  ```bash
  firewall-cmd --permanent --add-port=22/tcp
  ```
- Allow DNS (UDP 53):
  ```bash
  firewall-cmd --permanent --add-port=53/udp
  ```
- Allow DHCP (UDP 67):
  ```bash
  firewall-cmd --permanent --add-port=67/udp
  ```
- Reload firewalld:
  ```bash
  firewall-cmd --reload
  ```

---

## Blocking Specific IPs or Subnets 🚫
- Block a specific IP:
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.1.6" drop'
  ```
- Block a subnet:
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="20.20.20.0/24" drop'
  ```

---

## Blocking Outgoing Traffic ⛔
- Block outbound to Google DNS:
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv4" destination address="8.8.8.8" reject'
  ```
- Block outbound HTTPS to Google IP range:
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv4" destination address="172.217.217.0/24" port protocol="tcp" port="443" reject'
  ```

---

## Deleting Firewall Rules 🗑️
- Remove allowed port:
  ```bash
  firewall-cmd --permanent --remove-port=80/tcp
  ```
- Remove allowed service:
  ```bash
  firewall-cmd --permanent --remove-service=http
  ```
- Remove rich rule:
  ```bash
  firewall-cmd --permanent --remove-rich-rule='rule family="ipv4" source address="192.168.1.6" drop'
  ```
  ```bash
  firewall-cmd --permanent --remove-rich-rule='rule family="ipv4" source address="20.20.20.0/24" drop'
  ```

---

## Changing Zones in Firewalld 🌎
- Change default zone:
  ```bash
  firewall-cmd --set-default-zone=public
  ```
- Assign interface to zone:
  ```bash
  firewall-cmd --zone=trusted --change-interface=eth0 --permanent
  firewall-cmd --reload
  ```

---

## Reloading and Saving Rules 💾
- Reload firewalld:
  ```bash
  firewall-cmd --reload
  ```
- Save runtime rules to permanent:
  ```bash
  firewall-cmd --runtime-to-permanent
  ```

---

## Restoring Firewalld Rules 🔄
- Reset to factory defaults:
  ```bash
  firewall-cmd --complete-reload
  ```
- Clear rules when offline:
  ```bash
  firewall-cmd --offline-cmd --clear
  ```

---
## Allowing and Denying Services 🟢🔴

- ✅ **Allow NFS service (instead of specifying multiple ports):**
  ```bash
  firewall-cmd --permanent --add-service=nfs
  ```

- ❌ **Deny (remove) Samba service:**
  ```bash
  firewall-cmd --permanent --remove-service=samba
  ```
---
## Port Ranges 🔢

- 🔓 **Allow port range 3000–3010 for TCP:**
  ```bash
  firewall-cmd --permanent --add-port=3000-3010/tcp
  ```
---
## Forwarding Rules 🔀

- 📤 **Forward all traffic from port 8080 to port 80:**
  ```bash
  firewall-cmd --permanent --add-forward-port=port=8080:proto=tcp:toport=80
  ```

- 🛫 **Forward port 2222 on local system to SSH (22) on another host:**
  ```bash
  firewall-cmd --permanent --add-forward-port=port=2222:proto=tcp:toaddr=192.168.1.100:toport=22
  ```
---
## Masquerading (NAT) 🎭

- 🌐 **Enable masquerading in a zone:**
  ```bash
  firewall-cmd --zone=public --add-masquerade --permanent
  ```
- 🚫 **Disable masquerading**
  ```bash
  firewall-cmd --zone=public --remove-masquerade --permanent
  ```
---
## Rich Rules – Logging & Rate Limiting 📝⏱️

- 📄 **Log packets from a specific source:**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.1.50" log prefix="FW-DROP" level="info" drop'
  ```

- 🔒 **Limit SSH login attempts (max 3 connections per minute):**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule service name="ssh" limit value="3/m" accept'
  ```

---

## ICMP Rules 📡

- 🚫 **Block all ping requests:**
  ```bash
  firewall-cmd --permanent --add-icmp-block=echo-request
  ```

- ✅ **Allow ping again:**
  ```bash
  firewall-cmd --permanent --remove-icmp-block=echo-request
  ```

- 📋 **List available ICMP types:**
  ```bash
  firewall-cmd --get-icmptypes
  ```

---

## Interface & Zone Management 🌐🛡️

- ➕ **Add interface to internal zone:**
  ```bash
  firewall-cmd --zone=internal --add-interface=enp0s3 --permanent
  ```
- 🖥️ List interfaces per zone:
  ```bash
  firewall-cmd --get-active-zones
  ```
---

## IPv6 Rules 🌐

- 🚫 Block all incoming traffic from IPv6 subnet:
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv6" source address="2001:db8::/32" drop'
  ```
---

## Temporary Rules (Runtime Only) ⏳

- ✅ Allow HTTP for this session only:
  ```bash
  firewall-cmd --add-service=http
  ```

- ❌ Remove it again:
  ```bash
  firewall-cmd --remove-service=http
  ```
---

## Zone Management (Granular Control) 🛡️

- 📋 List all available zones:
  ```bash
  firewall-cmd --get-zones
  ```

- 🔍 Show details of a specific zone:
  ```bash
  firewall-cmd --zone=public --list-all
  ```

- ✨ **Create a custom zone:**
  ```bash
  firewall-cmd --permanent --new-zone=dmz-test
  ```

- 🗑️ **Delete a custom zone:**
  ```bash
  firewall-cmd --permanent --delete-zone=dmz-test
  ```

---

## Source-Based Rules 🌍

- 🔗 **Allow traffic only from a specific subnet:**
  ```bash
  firewall-cmd --permanent --zone=public --add-source=192.168.10.0/24
  ```

- ❌ **Remove source from zone:**
  ```bash
  firewall-cmd --permanent --zone=public --remove-source=192.168.10.0/24
  ```
---

## Source Port Rules 🎯

- 🟡 **Allow packets coming from source port 12345 (uncommon case):**
  ```bash
  firewall-cmd --permanent --add-source-port=12345/tcp
  ```

- 🔢 **Allow a range of source ports:**
  ```bash
  firewall-cmd --permanent --add-source-port=10000-10100/udp
  ```

---

## Helpers (Protocols Requiring Tracking) 🧩

- 📋 **List all helpers:**
  ```bash
  firewall-cmd --get-helpers
  ```
- ➕ **Add FTP helper (for passive FTP):**
  ```bash
  firewall-cmd --permanent --add-helper=ftp
  ```

---

## Complex Rich Rules 🛡️

- 🔒 **Allow SSH only from subnet, drop others:**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" service name="ssh" accept'
  firewall-cmd --permanent --add-rich-rule='rule service name="ssh" drop'
  ```

- 📑 **Allow HTTP/HTTPS but log before accepting:**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule service name="http" log prefix="HTTP-ACCEPT " level="info" accept'
  firewall-cmd --permanent --add-rich-rule='rule service name="https" log prefix="HTTPS-ACCEPT " level="info" accept'
  ```

- 📡 **Limit ICMP echo requests (max 5 per second):**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule protocol value="icmp" limit value="5/s" accept'
  ```

---

## Lockdown Mode (Restrictive Control) 🚨

- 🔐 **Enable lockdown (only allow services in lockdown whitelist):**
  ```bash
  firewall-cmd --lockdown-on
  ```

- 🔓 **Disable lockdown:**
  ```bash
  firewall-cmd --lockdown-off
  ```

## Debugging & Monitoring 🐞🔍

- 📝 **List runtime configuration for a zone:**
  ```bash
  firewall-cmd --list-all --zone=public
  ```

- 📋 **Show all runtime rules in detail:**
  ```bash
  firewall-cmd --list-all-zones
  ```

- 📡 **Watch logs in real-time (with journalctl):**
  ```bash
  journalctl -xe -u firewalld
  ```

- 🧪 **Test effective rules in nftables backend:**
  ```bash
  nft list ruleset
  ```

---

## Advanced IPv6 Examples 🌐

- 🔒 **Drop all IPv6 traffic except SSH:**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv6" service name="ssh" accept'
  firewall-cmd --permanent --add-rich-rule='rule family="ipv6" drop'
  ```

- 🚫 **Block multicast (ff00::/8):**
  ```bash
  firewall-cmd --permanent --add-rich-rule='rule family="ipv6" source address="ff00::/8" drop'
  ```
