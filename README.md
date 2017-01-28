# SSL-Telegram

mkdir /etc/apache2/ssl

openssl req -newkey rsa:2048 -sha256 -nodes -keyout /etc/apache2/ssl/YOURPRIVATE.key -x509 -days 365 -out /etc/apache2/ssl/YOURPUBLIC.pem -subj "/C=ES/ST=&#60;CIUDAD&#62;/L=&#60;PAIS&#62;/O=&#60;NOMBREBOT&#62;/CN=&#60;DIRRECIONWEB/IP&#62;"

a2enmod ssl

cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/&#60;TUBOT&#62;.conf

nano /etc/apache2/sites-available/&#60;TUBOT&#62;.conf 

Buscas esta parte y añades
**********


		#   A self-signed (snakeoil) certificate can be created by installing
		#   the ssl-cert package. See
		#   /usr/share/doc/apache2/README.Debian.gz for more info.
		#   If both key and certificate are stored in the same file, only the
		#   SSLCertificateFile directive is needed.
		SSLCertificateFile	/etc/apache2/ssl/YOURPUBLIC.pem
		SSLCertificateKeyFile /etc/apache2/ssl/YOURPRIVATE.key
**********

a2ensite &#60;TUBOT&#62;.conf

service apache2 restart

curl -F "url=https://&#60;DIRECCIONWEB/IP&#62;" -F "certificate=@/etc/apache2/ssl/YOURPUBLIC.pem" https://api.telegram.org/bot&#60;TOKENBOT&#62;/setWebhook

Te devolvera un ok y para comprobarlo

Aquí veras las llamadas de TELEGRAM
tail -f /var/log/apache2/access.log 
