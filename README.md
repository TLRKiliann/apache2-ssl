# apache2-ssl
How to protect apache2 with HTTPS protocol (raspberry pi 3)

`apt-get install openssl`

`openssl genrsa -aes256 -out certificat.key 4096`

`mv certificat.key certificat.key.lock`

`openssl rsa -in certificat.key.lock -out certificat.key`

`openssl req -new -key certificat.key.lock -out certificat.csr`

`openssl x509 -req -days 365 -in certificat.csr -signkey certificat.key.lock -out certificat.crt`

`sudo a2enmod ssl`



/etc/apache2/site-availables/tuto.conf



`<VirtualHost *:80> \
    ServerName      tuto.name_site.com \
    # The HTTP port is redirected to the HTTPS port. \
    Redirect        / https://www.name_site.com \
</VirtualHost>

<VirtualHost *:443> \
    ServerName      tuto.name_site.com \
    DocumentRoot    /var/www/html \        
    SSLEngine on \
    SSLCertificateFile    /etc/ssl/www/certificat.crt \
    SSLCertificateKeyFile /etc/ssl/www/certificat.key \
    # Optional, here we say that we accept all SSL protocols except \
      SSLv2 and SSLv3 (of which we accept TLS here) \
    SSLProtocol all -SSLv2 -SSLv3 \
    # Optional, it is said that it is the server that gives the order \
      of the encryption algorithms during the negotiation with the client. \
    SSLHonorCipherOrder on \
    # Optional, encryption algorithm available (don't be too nasty or \
      many older browsers won't be able to connect anymore) \
    SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES \
</VirtualHost>`


`sudo a2ensite tuto.conf`

`service apache2 restart`
