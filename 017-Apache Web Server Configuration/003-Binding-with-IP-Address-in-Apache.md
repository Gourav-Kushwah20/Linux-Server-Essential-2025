# Binding With IP Address in Apache
Apache can bind to specific IP addresses and ports,allowing you to host multiple websites on the same server using Virtual Hosts.
This guide covers how to configure Apache to bind to specific Ip addresses and set Up Virual Hosts for multiple websites.

---
## 1.Create Website Directories
- 1.Navigate to the document root:
```bash
cd /var/www/html/
```
- 2.Create directories for each websites:
```bash
mkdir site1 site2 site3
```
- 3.Set permissions:
```bash
chown -Rv apache:apache site1/ site2/ site3/
```
```bash
chown -Rv apache:apache site*
```
---

## 2.Set Multiple IP Address Configuration
- Check IP Address
```bash
ip a
```
- Now Set Multiple Ip Address:
```bash
nmtui
```
### edit: nmtui > Edit a Connection > Edit > Address in add `192.168.1.22/24` > Activate a connection > Deactivate > Activate > ok

---
## 3.Before Modify configuration file
- Download HTML template:
https://htmlcodex.com/template/

- Unzip Template like this: 
```bash
unzip Selecao-1.0.0.zip
```
- delete `zip` file:
```bash
rm -rvf Selecao-1.0.0.zip
```
- Rename file name:
```bash
mv -v Selecao-1.0.0 site3
```
- copy `site2` in this location:
```bash
cp -vr site2/* /var/www/html/site2
```
- search the browser 

http://192.168.1.21/site2/

---

## 4.Modify Apache Configuration
Open the main Apache configuration file:
```bash
vim /etc/httpd/conf/httpd.conf
```

Multiple Virtual Hosts

```bash
IncludeOptional conf.d/*.conf

<VirtualHost 192.168.1.22:80>
    DocumentRoot /var/www/html/site1
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.23:80>
    DocumentRoot /var/www/html/site2
    DirectoryIndex index.html
</VirtualHost>

<VirtualHost 192.168.1.24:80>
    DocumentRoot /var/www/html/site3
    DirectoryIndex index.html
</VirtualHost>
```

Ensure the following line is present to allow virtual hosts:
```bash
IncludeOptional conf.d/*.conf
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
- search with IP Address

    http://192.168.1.22/

    http://192.168.1.23/

    http://192.168.1.24/

---
## 5.Create Virtual Host File
- Create individual virtual host configuration file under `/etc/httpd/conf.d/`.

### 5.1 Single IP Binding

### Site1 : 

- Bind Apache to a specific IP Address `192.168.1.21` for single site:
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
http://192.168.1.21/


### Site2 : 

- Bind Apache to a specific IP Address `192.168.1.22` for single site:
```bash
vim /etc/httpd/conf.d/site2.conf
```
Example:
```bash
<VirtualHost 192.168.1.22:80>
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
http://192.168.1.22/

### Site3 : 
- Bind Apache to a specific IP Address `192.168.1.23` for single site:
```bash
vim /etc/httpd/conf.d/site3.conf
```
Example:
```bash
<VirtualHost 192.168.1.23:80>
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
http://192.168.1.23/

---
### 5.2 Binding to all IP Address
- If you want Apache to listen on all available IP Address:
```bash
vim /etc/httpd/conf.d/site1.conf
```

Example:
```bash
<VirtualHost *:80>
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>
```
- Restart the `httpd.service` 
```bash
systemctl restart  httpd.service
```
---
## Validate Configuration
- check if the apache configuration is valid:
```bash
httpd -t
```
output:
Syntax OK

---
