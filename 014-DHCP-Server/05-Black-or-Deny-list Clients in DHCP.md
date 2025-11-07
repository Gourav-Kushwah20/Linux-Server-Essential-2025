# 🚫 Allow-or-Deny-list Clients in DHCP

You can prevent specific clients from receiving an IP address from the DHCP server by using the `deny booting` directive. This is useful for blocking unauthorized devices from accessing the network.

---

### 🛠️ Edit DHCP Configuration File

Open the DHCP configuration file for editing:

```bash
vi /etc/dhcp/dhcpd.conf
```

---

### 📌 Example Configuration for Denying Clients

This example denies a client with a specific MAC address from obtaining an IP address.

#### ❌ Example 1: Deny a Single Client


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

}

# Deny Specific client from obtaning ip
host win11 {
    hardware ethernet 08:00:27:C2:01:09;
    deny booting;
}

```
### 🔄 Restart DHCP Service

After updating the configuration file, restart the DHCP service to apply changes:

```bash
systemctl restart dhcpd.service
```
---

### 📘 Explanation

- `hardware ethernet`: Specifies the MAC address of the client to be denied.
- `deny booting`: Prevents the client from receiving an IP address.
- `subnet`: Defines the IP range and network options for each subnet.
- This setup ensures that the specified client is blocked while other clients in the subnet can still receive IPs.

---

Let me know if you'd like to expand this into a full DHCP access control guide or automate deny-listing with scripts!