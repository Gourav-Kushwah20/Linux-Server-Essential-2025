# Configure Static IP in Linux VM - Mobile hotspot

Your VM is currently receiving an IP from **DHCP**:

```
inet 10.106.11.30
netmask 255.255.255.0
broadcast 10.106.11.255
```

If you want to configure a **static IP**, it must be in the **same network range**.

## 🌐 Network Information

- **Network:** `10.106.11.0/24`
- **Gateway:** `10.106.11.1`
- **Current DHCP IP:** `10.106.11.30`

Example static IPs you can use:

- `10.106.11.190`
- `10.106.11.50`

---

# ⚙️ Static IP Configuration (Debian / Kali / Ubuntu)

## 1️⃣ Open the network interfaces file

```bash
vim /etc/network/interfaces
```

---

## 2️⃣ Replace DHCP with Static Configuration

If the file contains: comment with `#`

```bash
auto enp0s3
iface enp0s3 inet dhcp
```

Replace it with:

```bash
auto enp0s3
iface enp0s3 inet static
    address 10.106.11.190
    netmask 255.255.255.0
    network 10.106.11.0
    broadcast 10.106.11.255
    gateway 10.106.11.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

---

## 3️⃣ Restart Networking Service

```bash
sudo systemctl restart networking
```

or

```bash
sudo service networking restart
```

---

## 4️⃣ Verify Configuration

### Check IP address

```bash
ip a
```

### Test Internet Connectivity

```bash
ping 8.8.8.8
ping google.com
```

---

## ⚠️ Important Notes

✔ Static IP must be in the same subnet:

```
10.106.11.X
```

❌ Incorrect subnet example:

```
192.168.1.X
```

✔ Gateway is usually the router or hotspot IP:

```
10.106.11.1
```

---
