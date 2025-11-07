# 🛠️ Configure DHCP Exclusion Range

You can define an exclusion range in the DHCP configuration file to prevent certain IP addresses within the subnet from being assigned to clients. 🚫💻

## 📂 Edit DHCP Configuration File

```bash
vim /etc/dhcp/dhcpd.conf
```

## ✏️ Example Configuration for Exclusion Range

Use exclusion `range` statements to exclude specific IP addresses within the subnet. 🧱

### ❌ Example 1 - Exclude `192.168.1.51` to `192.168.1.70`


This example configure an exclusion range 192.168.1.51 192.168.1.70:

**🧾 Full Configuration Example:**

```bash
authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {

    # Specify the range of lease IP address
    range 192.168.1.50 192.168.1.50;
    range 192.168.1.71 192.168.1.150;

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
```
- **Reload the `dhcpd.service`**
```bash
systemctl restart dhcpd.service
```
- **Check lease IP-Address** 
```bash
cat /var/lib/dhcpd/dhcpd.leases
```