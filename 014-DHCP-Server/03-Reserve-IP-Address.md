# 📌 Reserve IP Address

You can reserve IP addresses for clients based on their MAC addresses in the DHCP configuration file. This ensures that specific devices always receive the same IP address from the DHCP server. 🖧🔒

## 📝 Edit DHCP Configuration File

```bash
vim /etc/dhcp/dhcpd.conf
```

## ⚙️ Step 2 - Example Configuration for IP Reservation

Use the `host` directive to bind a MAC address to a specific IP address. This is useful for printers, servers, or any device that needs a consistent IP. 🖨️🖥️

- **Reserve IP Address for a Single Client**

```bash
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {

    # Specify the range of lease IP address
    range 192.168.1.50 192.168.1.50;
    range 192.168.1.71 192.168.1.210;
    range 192.168.1.231 192.168.1.250;


    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time (in seconds)
    default-lease-time 600;

    # Max lease time (in seconds)
    max-lease-time 7200;
}

# Reserve IP address for specific client
host win11 {
  # MAC address of client for fixed IP
  hardware ethernet 08:00:27:13:C6:8C;
  # Reserved IP address
  fixed-address 192.168.1.202;
}
```

---
## 🧩 Example 2: Reserve Multiple IP Addresses in DHCP

This example demonstrates how to reserve IP addresses for multiple clients using their MAC addresses.

### 🔧 Configuration Snippet

```bash
host win11-1 { hardware ethernet 08:00:27:13:C6:8C; fixed-address 192.168.1.101;}
host win11-2 { hardware ethernet 00:0C:29:96:81:67; fixed-address 192.168.1.102;}
```

### 🔄 Restart DHCP Service

After updating the configuration file, restart the DHCP service to apply changes:

```bash
systemctl restart dhcpd.service
```

### 📘 Explanation

- `hardware ethernet`: Specifies the MAC address of the client device.
- `fixed-address`: Assigns a static IP address to the client with the matching MAC.
- **Effect**: When a client with the specified MAC address requests an IP, the DHCP server assigns the reserved IP.
- **Best Practice**: Reserved IPs should be **outside** the dynamic DHCP pool to avoid conflicts.
- Ensure reserved IPs are **not part of the dynamic range** defined in your DHCP pool.
