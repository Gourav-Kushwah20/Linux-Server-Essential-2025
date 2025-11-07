# Multiple Website with SSL

**Editing the Virtual host Configuration**

- Edit the Configuration file (`site1.conf` or any other configuration in `/etc/httpd/conf.d`) to include hosts for `SSL ports 8080 and 8443`

```bash
vim /etc/httpd/conf.d/site1.conf
```

- Add SSL Virtual Hosts for ports 8080 and 8443.Here is configuration you Provided:

**Virtual Hosts for port 8080 :**
```apache
Listen 8080 https

<VirtualHost 192.168.1.23:8080>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key
    ServerName ai.local
    ServerAlias www.ai.local
    DocumentRoot /var/www/html/site1/
    DirectoryIndex index.html
</VirtualHost>
```
**Virtual Hosts for port 8443 :**
```bash
vim /etc/httpd/conf.d/site2.conf
```
```apache
Listen 8443 https

<VirtualHost 192.168.1.23:8443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key
    ServerName infosec.local
    ServerAlias www.infosec.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
```
- Save and Close

---
## Firewall Configuration

- Allow Traffic on port 8080 (SSL):
```bash
firewall-cmd --permanent --add-port=8080/tcp
```
- Allow Traffic on port 8443 (SSL):
```bash
firewall-cmd --permanent --add-port=8443/tcp
```
- Reload the Firewall to apply changes
```bash
firewall-cmd --reload
```
---
## Restart Apache to Apply SSL Configuration

- Restart Apache to apply SSL configuration:

```bash
systemctl restart httpd.service
```
## Verify Ports are open
```bash
netstat -nltup | grep httpd
```
---

### Check Main Configuration File in Dublicate entry and `Listen ports`:
```bash
vim /etc/httpd/conf/httpd.conf
```
## Final Checks
- **Test Access**: You should now be able to access the sites via:

    - https://192.168.1.23:8080/

    - https://192.168.1.23:8443/
