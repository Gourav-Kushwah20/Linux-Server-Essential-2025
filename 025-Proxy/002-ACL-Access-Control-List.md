# 🛂 Access-Control-List

## 📘 Access Control List (ACL) in Squid Proxy

An **Access Control List (ACL)** in Squid is a set of rules used to control and filter network connections, website access, and port numbers based on specific conditions.

🔗 **For detailed official documentation, visit:**
**[Squid ACL FAQ](http://wiki.squid-cache.org/SquidFaq/SquidAcl)**

---

## 🧩 ACL Elements (Types)

Different ACL types define specific criteria for matching requests.
Below is a table of the most commonly used ACL types:

---

### 📋 ACL Types Table

| **ACL Type**    | **Description**                                                            |
| --------------- | -------------------------------------------------------------------------- |
| `src`           | Matches source (client) IP addresses                                       |
| `dst`           | Matches destination (server) IP addresses                                  |
| `myip`          | Matches the local IP address of a client's connection                      |
| `arp`           | Matches Ethernet (MAC) address                                             |
| `srcdomain`     | Matches source (client) domain name                                        |
| `dstdomain`     | Matches destination (server) domain name                                   |
| `srcdom_regex`  | Matches source domain using regular expressions                            |
| `dstdom_regex`  | Matches destination domain using regular expressions                       |
| `src_as`        | Matches source Autonomous System number                                    |
| `dst_as`        | Matches destination Autonomous System number                               |
| `peername`      | Matches name tag assigned to the cache_peer destination                    |
| `time`          | Matches time of day and day of the week                                    |
| `url_regex`     | Matches URL patterns using regular expressions                             |
| `urlpath_regex` | Matches URL path (without protocol and hostname) using regular expressions |
| `port`          | Matches destination (server) port number                                   |
| `myport`        | Matches local port number used by the client                               |
| `myportname`    | Matches the name tag for Squid listening port                              |
| `proto`            | Matches transfer protocol (HTTP, FTP, etc.)                                 |
| `method`           | Matches HTTP request methods (GET, POST, etc.)                              |
| `http_status`      | Matches HTTP response status codes (200, 302, 404, etc.)                    |
| `browser`          | Matches user-agent header using regular expressions                         |
| `referer_regex`    | Matches HTTP referer header using regular expressions                       |
| `proxy_auth`       | Matches user authentication via external processes                          |
| `proxy_auth_regex` | Matches authentication credentials using regular expressions                |
| `user_cert`        | Matches against user SSL certificate attributes                             |
| `ca_cert`          | Matches against user CA SSL certificate attributes                          |
| `ext_user`         | Matches user field from an external ACL helper                              |
| `ext_user_regex`   | Matches user field from external ACL using regular expressions              |
| `snmp_community`   | Matches SNMP community string                                               |
| `maxconn`          | Limits number of connections from a single client IP                        |
| `max_user_ip`      | Limits the number of IPs a user can log in from                             |
| `req_mime_type`    | Matches request content-type header using regex                             |
| `req_header`       | Matches specific request headers using regex                                |
| `rep_mime_type`    | Matches downloaded content MIME type (usable in `http_reply_access`)        |
| `rep_header`       | Matches reply header content (usable in `http_reply_access`)                |
| `external`         | Performs a lookup via an external ACL helper defined by `external_acl_type` |

---

## 🧱 Basic ACL Syntax

The basic syntax for defining an ACL rule is:

```bash
acl [Name] [Type] [Data]
```

### 📌 Where:

* **Name** → A user-defined identifier for the ACL
* **Type** → Specifies the type of match (IP, port, domain, etc.)
* **Data** → The matching criteria

---

## 📝 ACL Examples


## 🏠 Allow Local Network

To allow access only from a local network (e.g., **192.168.0.0/16**):

```bash
acl localnet src 192.168.0.0/16
http_access allow localnet
http_access deny all
```

### 📌 Explanation:

* `acl localnet src 192.168.0.0/16` → Matches any IP address in the **192.168.x.x** range.
* `http_access allow localnet` → Allows access for local network users.
* `http_access deny all` → Denies access to everyone else.

---

## 🔒 Restrict To Safe Ports Only

To restrict access to only safe ports (e.g., **HTTP, HTTPS**):

```bash
acl SSL_ports port 443
http_access deny !Safe_ports
```

### 📌 Explanation:

* `acl SSL_ports port 443` → Defines the allowed SSL traffic (HTTPS).
* `http_access deny !Safe_ports` → Denies access to any traffic not using a predefined “safe” port.

---

## 🟦 Full Minimal Squid ACL Configuration

```bash
acl localnet src 0.0.0.1-0.255.255.255    # RFC 1122 "this" network (LAN)
acl localnet src 10.0.0.0/8               # RFC 1918 local private network (LAN)
acl localnet src 100.64.0.0/10            # RFC 6598 shared address space (CGN)
acl localnet src 169.254.0.0/16           # RFC 3927 link-local (directly plugged) machines
acl localnet src 172.16.0.0/12            # RFC 1918 local private network (LAN)
acl localnet src 192.168.0.0/16           # RFC 1918 local private network (LAN)
acl localnet src fc00::/7                 # RFC 4193 local private network range
acl localnet src fe80::/10                # RFC 4291 link-local (directly plugged) machines

acl SSL_ports port 443                     # https
acl Safe_ports port 80                     # http
acl Safe_ports port 21                     # ftp
acl Safe_ports port 443                    # https
acl Safe_ports port 70                     # gopher
acl Safe_ports port 210                    # wais
acl Safe_ports port 1025-65535             # unregistered ports
acl Safe_ports port 280                    # http-mgmt
acl Safe_ports port 488                    # gss-http
acl Safe_ports port 591                    # filemaker
acl Safe_ports port 777                    # multiling http

http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow localhost manager
http_access deny manager
http_access allow localnet
http_access allow localhost
http_access deny all

http_port 3128
coredump_dir /var/spool/squid

refresh_pattern ^ftp:           1440   20%   10080
refresh_pattern ^gopher:        1440   0%    1440
refresh_pattern -i (/cgi-bin/|\?) 0    0%    0
refresh_pattern .               0      20%   4320
```

## 🌐 Allow Specific Websites Using Squid Proxy

You can allow access to only specific websites using ACLs that match the destination domain.

---

## ✏️ Edit Squid Configuration File

Open the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

- 🔍 Allow Only Google

To allow access **only to Google**:

```bash
acl allow_website dstdomain .google.com
http_access allow allow_website
http_access deny all
```

### 📘 Explanation:

* `acl allow_website dstdomain .google.com`
  → Matches any subdomain of **google.com**

* `http_access allow allow_website`
  → Allows access to Google

* `http_access deny all`
  → Blocks access to all other websites

---

## 🔁 Restart Squid Service

Apply the configuration changes:

```bash
systemctl restart squid
```

---

## 🌐 Allow Multiple Websites Using a List File

For managing multiple allowed websites, use an external list.

---

## 📄 Create a Website List File

Create a new file with the allowed websites:

```bash
vim /etc/squid/allow_website_list
```

Add the domains, **one per line**:

```text
.google.com
.youtube.com
.armourinfosec.com
.facebook.com
```

---

## 🔗 Reference the List in `squid.conf`

Update the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

Add the following rule:

```bash
acl allow_website dstdomain "/etc/squid/allow_website_list"
http_access allow allow_website
http_access deny all
```

---

## 🔁 Restart Squid Service

Apply the configuration changes:

```bash
systemctl restart squid
```
---

## 🚫 Deny Access to Specific Websites

You can block specific websites by defining ACLs that match destination domains.

## ❌ Deny Specific Websites

### ✏️ Open the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

### 🚫 To block access to a specific website (e.g., **google.com**):

```bash
acl deny_website dstdomain .google.com
http_access deny deny_website
```

### 📘 Explanation:

* `acl deny_website dstdomain .google.com`
  → Blocks access to **google.com**
* `http_access deny deny_website`
  → Denies access to the matched domain

---

## 🔁 Restart Squid Service

Apply the configuration changes:

```bash
systemctl restart squid
```

---

## 🚫 Deny Multiple Websites

### ✏️ Open the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

To block multiple websites:

```bash
acl deny_website dstdomain .google.com .youtube.com
http_access deny deny_website
```

---

## 🔁 Restart Squid Service

Apply the configuration changes:

```bash
systemctl restart squid
```
---

## 📄 Use External File for Website List

To maintain a list of websites to block, use an external file.

---

## 📁 Create a Website List

Create a file for blocked websites:

```bash
vim /etc/squid/deny_website_list
```

Add websites to block (**one per line**):

```text
.google.com
.youtube.com
.armourinfosec.com
.facebook.com
```


## 🔧 Update Squid Configuration

Edit the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

Add the following rule:

```bash
acl deny_website dstdomain "/etc/squid/deny_website_list"
http_access deny deny_website
```
---

# 🚫 Deny Access Based on Keywords in Squid

---

## ✏️ Open the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

---

## 🔍 Add the rule to deny URLs containing certain keywords:

For example:

```bash
acl deny_keywords url_regex -i reports news game
http_access deny deny_keywords
```

---

## 📘 Explanation:

* `acl deny_keywords url_regex -i reports news game`
  → Matches URLs containing any of the keywords **reports**, **news**, or **game** (case-insensitive due to `-i` flag).

* `http_access deny deny_keywords`
  → Denies access if any keyword matches the URL.

---

## 🔁 Restart the Squid service to apply the changes:

```bash
systemctl restart squid
```

---

## 📝 Create a List of Keywords to Block

If you want to block many keywords or maintain a large list, store them in a separate file.

Create the file:

```bash
vim /etc/squid/deny_keywords
```
## 📝 Add Keywords to Block

Add the keywords you want to block (**one per line**):

```text
torrent
game
news
business
sports
```

---

## 🔧 Update the Squid Configuration to Use the External List

Now modify the Squid configuration to use this external list of keywords.

---

## ✏️ Edit the Squid configuration file again:

```bash
vim /etc/squid/squid.conf
```

---

## ➕ Add the following rule to load the list from the file:

```bash
acl deny_keywords url_regex -i "/etc/squid/deny_keywords"
http_access deny deny_keywords
```

---

## 📘 Explanation:

* `acl deny_keywords url_regex -i "/etc/squid/deny_keywords"`
  → Tells Squid to read keywords from the **/etc/squid/deny_keywords** file and block any URL containing those words.

* `http_access deny deny_keywords`
  → Denies access to URLs matching any keyword in the list.

---

## 🔁 Restart the Squid Service

Apply the changes by restarting the service:

```bash
systemctl restart squid
```

---

## 🚫 Deny Clients Using Squid

## ❌ Deny a Specific Client (Single IP)

To block access from a specific client IP (e.g., **192.168.2.51**), add this rule in your Squid configuration.

### ✏️ Edit the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

### ➕ Add the rule to deny the client:

```bash
acl deny_client src 192.168.2.11
http_access deny deny_client
```

### 📘 Explanation:

* `acl deny_client src 192.168.2.51`
  → Defines the IP address you want to block.

* `http_access deny deny_client`
  → Denies access from the defined client.

---

### 🔁 Restart Squid:

```bash
systemctl restart squid
```

---

## ❌ Deny Multiple Clients (Multiple IPs)

If you want to block several IP addresses, specify them all in the configuration:

### ✏️ Open the Squid configuration file:

```bash
vim /etc/squid/squid.conf
```

### ➕ Add multiple client IPs:

```bash
acl deny_client src 192.168.2.11 192.168.2.12 192.168.2.13 192.168.2.14
http_access deny deny_client
```

### 📘 Explanation:

* `acl deny_client src 192.168.2.11 192.168.2.12 ...`
  → Lists the client IPs you want to block.

* `http_access deny deny_client`
  → Denies access for all listed clients.

---
### 🔁 Restart Squid:

```bash
systemctl restart squid
```
---

## 🚫 Deny Clients Using an External File

To make it easier to manage multiple blocked clients, you can store the list of IPs in an external file.

---

## 📁 1. Create a file to hold the denied IPs:

```bash
vim /etc/squid/deny_clients
```

Add the IPs you want to block (**one per line**):

```text
192.168.2.51
192.168.2.52
192.168.2.100
192.168.2.101
192.168.2.80
192.168.2.81
192.168.2.85
192.168.2.95
192.168.2.200
```

---

## ✏️ 2. Edit the Squid configuration file to reference this external list:

```bash
vim /etc/squid/squid.conf
```

Add the following rule:

```bash
acl deny_client src "/etc/squid/deny_clients"
http_access deny deny_client
```

### 📘 Explanation:

* `acl deny_client src "/etc/squid/deny_clients"`
  → Tells Squid to load the denied IP list from the external file.

* `http_access deny deny_client`
  → Blocks access to all IPs listed in the file.

---

## 🔁 3. Restart Squid to apply the changes:

```bash
systemctl restart squid
```

---

## 🛠️ Monitoring and Testing

## 📊 Verify ACL Functionality

To check if your ACLs are working, monitor the Squid access logs:

```bash
tail -f /var/log/squid/access.log
```
## 🔍 Test Access

Test by trying to visit the websites you have **blocked** or **allowed**
to ensure the configuration is working properly.

---
