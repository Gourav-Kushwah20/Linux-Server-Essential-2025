# Binding-with-Domain-Name

## Binding with Domain Names in Apache

Apache can bind to specific **domain names** using the **ServerName** and **ServerAlias** directives in Virtual Hosts.  
This allows you to host multiple websites on the same IP address, each accessible by a unique domain name.

---

## 🛠️ Configure Hostnames (DNS or /etc/hosts)

- To simulate DNS resolution, define the domain names in the `/etc/hosts` file:

### ✏️ Edit /etc/hosts:
```bash
vim /etc/hosts
````

Add the following entries:

```bash
127.0.0.1   localhost ns1 ns1.armour.local
::1         localhost localhost6 ns1 ns1.armour.local
192.168.1.21 ns1 ns1.armour.local armour.local www.armour.local ai.local www.ai.local infosec.local www.infosec.local
```

This will resolve the domain names to the local IP (`192.168.1.21`).

---

## 🌐 2. Test DNS Resolution

Use **dig** to confirm that the domain names resolve to the correct IP:

```bash
dig www.armour.local +short
```

**Example output:**

```bash
armour.local.
192.168.1.21
```

Repeat for other domains:

```bash
dig www.infosec.local +short
```
```bash
infosec.local.
192.168.1.21
```

Repeat for other domains:

```bash
dig www.armour.local +short
```
```bash
ai.local.
192.168.1.21
```
---

# Create Apache Virtual Host Files

- Define virtual hosts for each domain.  
  Apache will route incoming requests based on the **ServerName** and **ServerAlias**.

---

## 🏠 Main Virtual Host

Create a default virtual host file:

```bash
vim /etc/httpd/conf/httpd.conf
````

**Example:**

```apache
<VirtualHost 192.168.1.21:80>
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>
```

---

## 🌐 Virtual Host for `armour.local`

Create a virtual host file for **armour.local**:

```bash
vim /etc/httpd/conf.d/armour.conf
```

**Example:**

```apache
<VirtualHost 192.168.1.21:80>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>
```

---

## 🌐  Virtual Host for `infosec.local`

Create a virtual host file for **infosec.local**:

```bash
vim /etc/httpd/conf.d/infosec.conf
```

**Example:**

```apache
<VirtualHost 192.168.1.21:80>
    ServerName infosec.local
    ServerAlias www.infosec.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
```
## 🌐 Virtual Host for `infosec.local`

Create a virtual host file for **infosec.local**:

```bash
vim /etc/httpd/conf.d/infosec.conf
```

**Example:**

```apache
<VirtualHost 192.168.1.21:80>
    ServerName infosec.local
    ServerAlias www.infosec.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
```
## 🌐Virtual Host for `ai.local`

Create a virtual host file for **ai.local**:

```bash
vim /etc/httpd/conf.d/ai.conf
```

**Example:**

```apache
<VirtualHost 192.168.1.21:80>
    ServerName ai.local
    ServerAlias www.ai.local
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>
```

- Check after Configure `site1`,`site2`,`site3` are placed or not this location `/var/www/html`

## Validate Configuration
- check if the apache configuration is valid:
```bash
httpd -t
```
**Output :**
Syntax OK

---
## Restart the `httpd.service` 
```bash
systemctl restart httpd.service
```

## Verify Listening Ports
use `netstat` to confirm apache is listening on the correct ports:

```bash
netstat -nltup | grep httpd
```

## Configure Firewall
```bash
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --permanent --add-port=8081/tcp
firewall-cmd --permanent --add-port=8082/tcp
firewall-cmd --reload
```

## You can call the website with domain name

- Open Windows Client and configure static ip `192.168.1.something`and DNS `192.168.1.serverIP`  Ex: `192.168.1.21`

- open Browser 

    : 192.168.1.21:80

    : armour.local

    : wwww.armour.local

    : infosec.local

    : www.infosec.local

    : ai.local

    : www.ai.local

---
# Binding Domains with Different Ports

> You can also bind the same domains to different ports using `Listen`.

---

## 4.1. Bind `armour.local` on Port 8080

Edit Apache config:

```bash
vim /etc/httpd/conf.d/armour-8080.conf
````

**Example:**

```apache
Listen 8080

<VirtualHost 192.168.1.21:8080>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/site4
    DirectoryIndex index.html
</VirtualHost>
```

---

## 4.2. Bind `infosec.local` on Port 8080

Create a virtual host file for `infosec.local` on port 8080:

```bash
vim /etc/httpd/conf.d/infosec-8080.conf
```

**Example:**

```apache
Listen 8080

<VirtualHost 192.168.1.21:8080>
    ServerName infosec.local
    ServerAlias www.infosec.local
    DocumentRoot /var/www/html/site5
    DirectoryIndex index.html
</VirtualHost>
```

## 4.3. Bind `ai.local` on Port 8081

Create a virtual host file for `ai.local` on port 8080:

```bash
vim /etc/httpd/conf.d/ai-8080.conf
```

**Example:**

```apache
Listen 8080

<VirtualHost 192.168.1.21:8081>
    ServerName ai.local
    ServerAlias www.ai.local
    DocumentRoot /var/www/html/site6
    DirectoryIndex index.html
</VirtualHost>
```
## Validate Configuration
- check if the apache configuration is valid:
```bash
httpd -t
```
**Output :**
Syntax OK

---
## Restart the `httpd.service` 
```bash
systemctl restart httpd.service
```

## Verify Listening Ports
use `netstat` to confirm apache is listening on the correct ports:

```bash
netstat -nltup | grep httpd 
```
---
## 8. Test in Browser

You should be able to access the sites by their domain names:

| Domain                                | Port |
|---------------------------------------|------|
| http://armour.local                   | 80   |
| http://www.armour.local               | 80   |
| http://infosec.local                  | 80   |
| http://www.infosec.local              | 80   |
| http://ai.local                       | 80   |
| http://www.ai.local                   | 80   |
| http://armour.local:8080              | 8080 |
| http://infosec.local:8080             | 8080 |
| http://ai.local:8081                  | 8081 |

---

## 🏆 Example: Complete Apache Virtual Host Configuration

Example of a complete Apache configuration using different domain names and ports:

```apache
Listen 80
Listen 8080
Listen 8081

<VirtualHost 192.168.1.21:80>
    DocumentRoot /var/www/html/site1
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:80>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/site2
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:80>
    ServerName infosec.local
    ServerAlias www.infosec.local
    DocumentRoot /var/www/html/site3
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:8080>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/site2
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:80>
    ServerName ai.local
    ServerAlias www.ai.local
    DocumentRoot /var/www/html/site4
    DirectoryIndex index.html
</VirtualHost>
```

