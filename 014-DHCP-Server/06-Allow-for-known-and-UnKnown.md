# 🔄 Allow for Known and Unknown Clients in DHCP

This configuration enables a DHCP server to assign IP addresses from **separate pools** based on whether a client is known (whitelisted) or unknown.

---

### 🛠️ Edit DHCP Configuration File

Open the DHCP configuration file:

```bash
vi /etc/dhcp/dhcpd.conf
```

---

### 📌 Example Configuration

```bash
authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {

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

    # Pool for Known Client
    pool {
        range 192.168.1.150 192.168.1.200;
        allow known-clients;
    }

    # Pool for Unknown Client
    pool {
        range 192.168.1.201 192.168.1.250;
        allow unknown-clients;
    }

}

# Define Known clients
host win11 {
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

- `allow known-clients`: Assigns IPs from a specific range to clients defined in `host` blocks.
- `allow unknown-clients`: Assigns IPs from a separate range to clients not explicitly defined.
- `pool`: Used to segment IP allocation based on client identity.
- This setup helps manage trusted devices separately from guests or new devices.

---

Let me know if you'd like to add logging, monitoring, or automation to this DHCP setup!