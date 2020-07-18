# apache2-ssl
How to protect apache2 (raspberry pi 3)

> apt-get install openssl

> openssl genrsa -aes256 -out certificat.key 4096

> mv certificat.key certificat.key.lock

> openssl rsa -in certificat.key.lock -out certificat.key

> openssl req -new -key certificat.key.lock -out certificat.csr

> openssl x509 -req -days 365 -in certificat.csr -signkey certificat.key.lock -out certificat.crt

> sudo a2enmod ssl



/etc/apache2/site-availables/tuto.conf



> <VirtualHost *:80>
    ServerName      tuto.name_site.com
    # On redirige le port HTTP vers le port HTTPS
    Redirect        / https://www.name_site.com
> </VirtualHost>

> <VirtualHost *:443>
    ServerName      tuto.name_site.com
    DocumentRoot    /var/www/html
        
    SSLEngine on
    SSLCertificateFile    /etc/ssl/www/certificat.crt
    SSLCertificateKeyFile /etc/ssl/www/certificat.key
    # Facultatif, ici, on dit qu'on accepte tout les protocoles SSL sauf SSLv2 et SSLv3 (dont on accepte que le TLS ici)
    SSLProtocol all -SSLv2 -SSLv3
    # Facultatif, on dit que c'est le serveur qui donne l'ordre des algorithmes de chiffrement pendant la négociation avec le client
    SSLHonorCipherOrder on
    # Facultatif, algorithme de chiffrement disponibles (ne pas être trop méchant sinon beaucoup de navigateur un peu ancien ne pourront plus se connecter)
    SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES
> </VirtualHost>


> sudo a2ensite tuto.conf

> service apache2 restart
