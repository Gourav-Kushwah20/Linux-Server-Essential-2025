# DNS and Reounting Configuration
### DNS Configuration
## 1️⃣ NetworkManager (Generic / Fedora / RHEL / CentOS / Ubuntu with NM)

📌 Set DNS in the **NetworkManager connection profile**:

```ini
[connection]
id=enp0s3
uuid=1234-5678
type=ethernet

[ipv4]
method=manual
addresses=192.168.1.100/24
gateway=192.168.1.1
dns=8.8.8.8;8.8.4.4;
```

### ⚡ Apply changes:

```bash
nmcli connection reload
```

```bash
nmcli connection up enp0s3
```
---

## 2️⃣ CentOS / RHEL (ifcfg files)

📌 Edit file:
```bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

### 📄 Example configuration:
```bash
BOOTPROTO=static
IPADDR=192.168.1.100
PREFIX=24
GATEWAY=192.168.1.1
DNS1=8.8.8.8
DNS2=8.8.4.4
```

🔄 Restart network:
```bash
systemctl restart network
```
---

## 3️⃣ Debian / Ubuntu (interfaces file)

📌 Edit the interfaces file:

```bash
sudo vim /etc/network/interfaces
```

### 📄 Example static configuration:
```bash
auto eth0
iface eth0 inet static
    address 192.168.1.100/24
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

🔄 Restart networking:
```bash
systemctl restart networking
```
---

## 🛣️ Routing `Gateway`
### 1️⃣ Static Route in Config

- Add default gateway:
```bash
gateway=192.168.1.1
```

- Or define specific routes in route files
(e.g., `/etc/sysconfig/network-scripts/route-enp0s3` on <b>RHEL</b>).

---

## 2️⃣ Commands (Temporary Routes)

👉 Add default route:
```bash
ip route add default via 192.168.1.1 dev eth0
```

👉 Add specific route:
```bash
ip route add 10.10.10.0/24 via 192.168.1.254 dev eth0
```

👉 Legacy route command (older systems):
```bash
route add -net 10.10.10.0/24 gw 192.168.1.254 dev eth0
```
👉 Show rounting table:
```bash
ip route show 
```
---

## 3. Debain (interfaces files)

```bash
vim /etc/network/interfaces
```
```bash
iface enp0s3 inet static
   address 192.168.1.50
   netmask 255.255.255.0
   network 192.168.1.0 
   broadcast 192.168.1.255
   gateway 192.168.1.1
   dns-nameservers 8.8.8.8
```

🔄 Restart network:
```bash
systemctl restart network
```
---