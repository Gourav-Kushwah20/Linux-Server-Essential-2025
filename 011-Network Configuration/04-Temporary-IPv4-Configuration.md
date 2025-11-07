# 🌐 **Ifconfig** vs **Ip** Commands
## Linux Network Configuration: `ifconfig` and `ip`

The **`ifconfig`** command is a **legacy tool** used to configure network interfaces.  
The modern replacement is the **`ip`** command, which is faster and more powerful.  

---

## 🕰️ Using `ifconfig` (Legacy System)

### 🔹 Display Interfaces
- Show all **active** network interfaces:
```bash
ifconfig

```
- Show all interfaces, including inactive ones:
```bash
ifconfig -a
```

### 🔹 View a Specific Interface
```bash
ifconfig enp0s3
```

--- 
## Assign IPv4 Address using `ifconfig`

The `ifconfig` command (legacy tool) can be used to assign **temporary IPv4 addresses** to a network interface.  
⚠️ These changes are **not persistent** and will reset after reboot.

## 🔹 Assign IP without Netmask
```bash
ifconfig enp0s3 192.168.1.22
```
### 🔹 Assign IP with Netmask
```bash
ifconfig enp0s3 192.168.1.23 netmask 255.255.255.0
```
➡️ Assigns IP `192.168.1.23 `with subnet mask 2`55.255.255.0.`

### 🔹 Assign IP with CIDR Notation
```bash
ifconfig enp0s3 192.168.1.26/20
```
➡️ Assigns IP `192.168.1.26` with subnet mask `/20 `(255.255.240.0).

---

## 🔌 **Enable** and **Disable** Interfaces

You can enable (bring **up**) or disable (bring **down**) network interfaces using `ifconfig` or legacy scripts.

---

## 🕰️ Using `ifconfig`

### 🔹 Disable Interface
```bash
ifconfig enp0s3 down
```
### 🔹 Enable Interface
```bash
ifconfig enp0s3 up # enable
```
---

## 📜 Using Legacy Scripts (**ifup** / **ifdown**)
### 🔹 Disable Interface
```bash
ifdown enp0s3 #Disable
```
### 🔹 Enable Interface
```bash
ifup enp0s3  # enable
```
---

## 🌍 Assign **Multiple** IPv4 Addresses on One Interface

You can assign multiple IPv4 addresses to a **single interface** using **aliasing** in `ifconfig`.

## 🔹 Add Multiple IPs

```bash
ifconfig enp0s3:1 192.168.1.42
ifconfig enp0s3:2 192.168.1.43
```
➡️ Here, `enp0s3:1 `and `enp0s3:2` are interface aliases for `enp0s3.`

### 🔍 Verify Assigned Interfaces
```bash
ifconfig -a
```
---

##  Using `ip` Command (Recommended in CentOS 9)

## 🔹 Assign IP Address (Temporary)

- 👉 Add an IP address to an interface (`enp0s3`):

```bash
ip addr add 192.168.1.40/24 dev enp0s3
```
- 👉 Verify assigned addresses:
```bash
ip addr 
```
- 👉 Add multiple IP addresses:
```bash
ip addr add 192.168.1.41/24 dev enp0s3
```

```bash
ip addr add 192.168.1.42/20 dev enp0s3
```

### 🔍 Testing Connectivity

- 👉 Ping the assigned IP:
```bash
ping 192.168.1.42
```
- 👉 Show IP addresses for specific interface:
```bash
ip addr show dev enp0s3
```
--- 

## 🗑️ Deleting IPv4 Addresses with `ip` Command

### 🔹 Delete IPv4 Address `192.168.1.22`

```bash
ip addr del 192.168.1.22/24 dev enp0s3

```
### 🔹 Delete IPv4 Address 192.168.1.23
```bash
ip addr del 192.168.1.23/24 dev enp0s3
```

#### 🔍 Verify Changes
👉 Show the current IP configuration of the interface:
```bash
ip addr show
```
---

## Add IPv4 addresss (presistent with nmcli)

```bash
nmcli con mod enp0s3 +ipv4.addresses 192.168.1.40/24
```
```bash
nmcli con mod enp0s3 +ipv4.addresses 192.168.1.41/24
```
```bash
nmcli con mod enp0s3 +ipv4.addresses 192.168.1.42/20
```
```bash
nmcli con mod enp0s3 ipv4.method manual
```
```bash
nmcli con mod enp0s3 +ipv4.gateway 192.168.1.1
```
```bash
nmcli con mod enp0s3 +ipv4.dns "8.8.8.8 8.8.4.4"
```
```bash
nmcli con up enp0s3
```
```bash
#Output
# Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/3)
```

```bash
ip addr show
```

### Enable and Disable:

```bash
ip link set enp0s3 down
```

```bash
ip link set enp0s3 up
```

---

## Routing with `IP` (temporary)


-  Show routing table:
```bash
ip route
```
- Add a Route:
```bash
ip route add 192.168.2.0/24 via 192.168.1.1 dev enp0s3
```
- check route:
```bash
ip route
```
-  Delete a route:
```bash
ip route del 192.168.2.0/24
```
- again check route:
```bash
ip route
```
- set Default Gateway:
```bash
ip route add default via 192.168.1.1 dev enp0s3 
```

- Show route for specific interfaces:
```bash
ip route show dev enp0s3
```
