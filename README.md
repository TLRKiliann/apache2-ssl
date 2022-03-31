# apache2-ssl

## How to protect apache2 with HTTPS protocol (raspberry pi 3) ?

`apt-get install openssl`

`openssl genrsa -aes256 -out cert.key 4096`

`mv cert.key cert.key.lock`

`openssl rsa -in cert.key.lock -out cert.key`

`openssl req -new -key cert.key.lock -out cert.csr`

`openssl x509 -req -days 365 -in cert.csr -signkey cert.key.lock -out cert.crt`

---

`sudo a2enmod ssl`

---

/etc/apache2/site-availables/tuto.conf

```
<VirtualHost *:80>
    ServerName      tuto.name_site.com
    # The HTTP port is redirected to the HTTPS port.
    Redirect        / https://www.name_site.com
</VirtualHost>

<VirtualHost *:443>
    ServerName      tuto.name_site.com
    DocumentRoot    /var/www/html
    SSLEngine on
    SSLCertificateFile    /etc/ssl/www/cert.crt
    SSLCertificateKeyFile /etc/ssl/www/cert.key
    
    # Optional, here we say that we accept all SSL protocols except
      SSLv2 and SSLv3 (of which we accept TLS here)
    SSLProtocol all -SSLv2 -SSLv3
    # Optional, it is said that it is the server that gives the order
      of the encryption algorithms during the negotiation with the client.
    SSLHonorCipherOrder on
    # Optional, encryption algorithm available (don't be too nasty or
      many older browsers won't be able to connect anymore)
    SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES
</VirtualHost>
```
---

`sudo a2ensite tuto.conf`

---

`service apache2 restart`
