# 🌐 Network Configuration on `Debian System`

On Debian-based systems (Ubuntu/Debian), network settings are managed using **Netplan** (Ubuntu 18.04+), or `/etc/network/interfaces` for older Debian versions.

---

## ⚡ Static IP Configuration `IPv4`

📄 Edit the config file:

```bash
sudo vim /etc/network/interfaces
```

### ✏️ Example configuration:

 This file describes the network interfaces

```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug enp0s3
#iface enp0s3 inet dhcp

iface enp0s3 inet static
	address 192.168.1.50
	netmask 255.255.255.0
	network 192.168.1.0 
	broadcast 192.168.1.255
	gateway 192.168.1.1
	dns-nameservers 8.8.8.8
iface enp0s3 inet6 auto


# This is an autoconfigured IPv6 interface
#iface enp0s3 inet6 auto
````
---

## ⚡ Dynamic IPv4 Configuration DHCP

```bash
vim /etc/network/interfaces
```
## uncomment this line `iface enp0s3 inet dhcp `

### remove     
 ```bash
 iface enp0s3 inet static
	address 192.168.1.50
	netmask 255.255.255.0
	network 192.168.1.0 
	broadcast 192.168.1.255
	gateway 192.168.1.1
	dns-nameservers 8.8.8.8
```

### ✏️ Example configuration for DHCP:
```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug enp0s3
iface enp0s3 inet dhcp 

iface enp0s3 inet6 auto


# This is an autoconfigured IPv6 interface
#iface enp0s3 inet6 auto

```
--- 

## On Debain  Configure `Multiple IPv4 Address`

### Edit interface file:

```bash
vim /etc/network/interfaces
```

```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug enp0s3
#iface enp0s3 inet dhcp

iface enp0s3 inet static
   address 192.168.1.50
   netmask 255.255.255.0
   network 192.168.1.0 
   broadcast 192.168.1.255
   gateway 192.168.1.1
   dns-nameservers 8.8.8.8
   up ip addr add 192.168.1.51/24 dev enp0s3 # Second Ipv4 addesss
   up ip addr add 192.168.1.52/24 dev enp0s3 # Third Ipv4 addesss

iface enp0s3 inet6 auto


# This is an autoconfigured IPv6 interface
#iface enp0s3 inet6 auto
```

### Check Multiple IPv4 Address
```bash
ip a
```
---