# SSL-Telegram

- Crea una carpeta para certificados SSL (si no tienes una ya creada)
```
mkdir /etc/apache2/ssl
```
- Genera una nueva clave pública y privada.
```
openssl req -newkey rsa:2048 -sha256 -nodes -keyout /etc/apache2/ssl/YOURPRIVATE.key -x509 -days 365 -out /etc/apache2/ssl/YOURPUBLIC.pem -subj "/C=ES/ST=#CIUDAD#/L=#PAIS#/O=#NOMBREBOT#/CN=#DIRRECIONWEB/IP#"
```
- Activa el módulo SSL para Apache si no lo tienes ya activo.
```
a2enmod ssl
```
- Genera un nuevo Virtual Host en base a la configuración por defecto.
```
cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/#TUBOT#.conf
```
- Abre un editor de texto para editar la configuración del sitio.
```
nano /etc/apache2/sites-available/#TUBOT#.conf 
```

Buscas esta parte y añades:
**********


		#   A self-signed (snakeoil) certificate can be created by installing
		#   the ssl-cert package. See
		#   /usr/share/doc/apache2/README.Debian.gz for more info.
		#   If both key and certificate are stored in the same file, only the
		#   SSLCertificateFile directive is needed.
		SSLCertificateFile	/etc/apache2/ssl/YOURPUBLIC.pem
		SSLCertificateKeyFile /etc/apache2/ssl/YOURPRIVATE.key
**********

- Activa el sitio creado para que pueda ser llamado.
```
a2ensite #TUBOT#.conf
```
- Reinicia el servicio Apache para cargar el nuevo sitio.
```
service apache2 restart
```
- Configura el Webhook en la API de Telegram para poder recibir los mensajes en el sitio que acabas de crear.
```
curl -F "url=https://#DIRECCIONWEB/IP#" -F "certificate=@/etc/apache2/ssl/YOURPUBLIC.pem" https://api.telegram.org/bot#TOKENBOT#/setWebhook
```

Te devolvera un ok y para comprobarlo

Aquí verás las llamadas de TELEGRAM
```
tail -f /var/log/apache2/access.log 
```
