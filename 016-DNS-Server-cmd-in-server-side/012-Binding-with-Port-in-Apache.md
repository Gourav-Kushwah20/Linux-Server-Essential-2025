# 🔧 Binding with Port Number in Apache

Apache can bind to specific ports, allowing you to run multiple websites on different ports using **Virtual Hosts**. This is useful when you want to test different sites on the same IP but using unique ports. 🌐

## 1. ⚙️ Modify Apache Configuration

Open the main Apache configuration file:

```bash
vim /etc/httpd/conf/httpd.conf
````

### 📡 Add Listen Directives for New Ports

The `Listen` directive tells Apache which ports to bind to. Add the following lines to allow Apache to listen on ports `80`,`81`, `8080`, `8081`, and `8082`: 
`Line no 49`

```bash
Listen 80
Listen 81
Listen 8080
Listen 8081
Listen 8082
```

💾 Save and exit.

## 2. 📝 Create Virtual Host Files

Create separate virtual host files for each port under `/etc/httpd/conf.d/`.

### 2.1 🌍 Default HTTP Port (80)

Create a virtual host for port `80`:

```bash
vim /etc/httpd/conf.d/site1.conf
```

```bash
IncludeOptional conf.d/*.conf

<VirtualHost 192.168.1.21:80>
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:8080>
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:8081>
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:8082>
    DocumentRoot /var/www/html/site4/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:81>
    DocumentRoot /var/www/html/site5/
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.21:82>
    DocumentRoot /var/www/html/site6/
    DirectoryIndex index.html
</VirtualHost>
```

--- 
## 3.Configure Firewall

```bash
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --permanent --add-port=8081/tcp
firewall-cmd --permanent --add-port=8082/tcp
firewall-cmd --reload
```
- Restart `Apache Service`:
```bash
systemctl restart  httpd.service
```
---
 
## Another Way to bind IP with Port

## Create Virtual Host File
- Create separate virtual host configuration `port` under `/etc/httpd/conf.d/`.

### Default HTTP Port (80)
### Site1 : 

- Create a virtual host for port `80`:
```bash
vim /etc/httpd/conf.d/site1.conf
```
Example:
```bash
<VirtualHost 192.168.1.21:80>
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
http://192.168.1.21:80


### Site2 : Virtual Host for Port 8080

- Create a virtual host for port `8080`:
```bash
vim /etc/httpd/conf.d/site2.conf
```
Example:
```bash
<VirtualHost 192.168.1.21:8080>
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
http://192.168.1.21:8080

### Site3 :  Virtual Host for Port 8081
- Create a virtual host for port `8081`:
```bash
vim /etc/httpd/conf.d/site3.conf
```
Example:
```bash
<VirtualHost 192.168.1.21:8081>
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
http://192.168.1.23:8081

---

## Virtual Host for Alternate IP and Port
- If you want to bind to another IP (192.168.1.22)

### Port 81: 

- Create a virtual host for port `81`:
```bash
vim /etc/httpd/conf.d/site5.conf
```
Example:
```bash
<VirtualHost 192.168.1.22:81>
    DocumentRoot /var/www/html/site5/
    DirectoryIndex index.html
</VirtualHost>
```
### Port 82: 

- Create a virtual host for port `82`:
```bash
vim /etc/httpd/conf.d/site6.conf
```
Example:
```bash
<VirtualHost 192.168.1.22:82>
    DocumentRoot /var/www/html/site6/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```

http://192.168.1.21:80

---
## Configure Firewall
```bash
firewall-cmd --permanent --add-port=81/tcp
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
```
---
## Validate Configuration
- check if the apache configuration is valid:
```bash
httpd -t
```
**Output :**
Syntax OK

---
### Restart the `httpd.service` 
```bash
systemctl restart httpd.service
```
---
### Verify Listening Ports
use `netstat` to confirm apache is listening on the correct ports:

```bash
netstat -nltup | grep httpd
```

```bash
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      4136/httpd          
tcp        0      0 0.0.0.0:81              0.0.0.0:*               LISTEN      4136/httpd          
tcp        0      0 0.0.0.0:8082            0.0.0.0:*               LISTEN      4136/httpd          
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      4136/httpd          
tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      4136/httpd 
```

