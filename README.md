##generating password for the key
openssl genrsa -des3 -out server.key 2048

#generating csr certificate.
openssl req -new -key server.key -out server.csr

cp server.key server.key.org

openssl rsa -in server.key.org -out server.key
----------------------------------------------------
cat server.csr (copy this to name.com and generate the certificate)

sudo nano mydomain.crt (paste the certificate generated from name.com)
sudo cp mydomain.crt /etc/ssl/certs/mydomain.crt
sudo chmod 777 /etc/ssl/certs/mydomain.crt

sudo cp server.key /etc/ssl/private/server.key
sudo chmod 777 /etc/ssl/private/server.key

sudo nano digicertca.crt (paste the ca authority certificate in it)
sudo cp digicertca.crt /etc/ssl/certs/digicertca.crt
sudo chmod 777 /etc/ssl/certs/digicertca.crt
----------------------------------------------------
sudo a2enmod ssl
sudo a2enmod headers
sudo a2ensite default-ssl
sudo apache2ctl configtest
sudo systemctl restart apache2
----------------------------------------------------------
cd /etc/apache2/sites-enabled/
sudo nano 000-default.conf
        ServerAdmin engr_saqib.naeem@hotmail.com
        DocumentRoot /var/www/html/
        Redirect  "/" "https://www.jatoi.engineer/" (add this line)
_________________________________________________________________
sudo nano default-ssl.conf
                SSLCertificateFile      /etc/ssl/certs/mydomain.crt
                SSLCertificateKeyFile /etc/ssl/private/server.key

		SSLCertificateChainFile /etc/ssl/certs/digicertca.crt
--------------------------------------------------------------------
sudo apache2ctl configtest
sudo systemctl restart apache2

------------------------------------------------------------------------------
cd /var/www/html
sudo mv index.html index_old.html
sudo nano index.html

<html>
 <head>
     <title>SN Khawaja</title>
     <style>
        h1 {text-align: center;}
        </style>
 </head>   
 <body>
     <h1>We just finished our short series and now own a server & domain ;)</h1>
 </body>
</html>
-------------------------------------------------------------
#for certificate validation
https://www.sslshopper.com/certificate-decoder.html
