# 🔐 White-or-Allow-list Clients in DHCP

This guide explains how to configure a DHCP server to **allow only specific clients** by whitelisting their MAC addresses.

---

### 📝 Whitelist/Allow List Clients

To restrict DHCP access to known devices only, use the `deny unknown-clients` directive. This ensures that only explicitly defined clients receive IP addresses.

---

### 🛠️ Edit DHCP Configuration File

Open the DHCP configuration file for editing:

```bash
vim /etc/dhcp/dhcpd.conf
```

---

### 📌 Example Configuration for Whitelisting Clients

This example allows only clients with specific MAC addresses to receive IP addresses.

```bash
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

    #deny unknown-clients from getting an IP Address
    deny unknown-clients;
}

# Allow CentOS to recieve an IP address
host CentOS {
  # MAC address of client 
  hardware ethernet 08:00:27:13:C6:8C;
 
}
# Allow win11-1 to recieve an IP address
host win11-1 {
  # MAC address of client 
  hardware ethernet 08:00:27:C2:01:09;
}
```

### 🔄 Restart DHCP Service

After updating the configuration file, restart the DHCP service to apply changes:

```bash
systemctl restart dhcpd.service
```
---
### 📘 Explanation

- `deny unknown-clients`: Blocks devices not explicitly defined in the config.
- `hardware ethernet`: Specifies the MAC address of the allowed client.
- `fixed-address`: Assigns a static IP to the whitelisted client.
- Reserved IPs should be **outside** the dynamic DHCP pool to prevent conflicts.

---

Let me know if you'd like to build a full DHCP security guide or automate this setup with scripts!