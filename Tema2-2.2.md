## Autenticación en Nginx
Vamos a utilizar openssl y compruebamos si esta instalada:
```
dpkg -l | grep openssl
```
Ahora creamos un archivo oculto .htpasswd en `/etc/nginx`
```
sudo sh -c  "echo -n 'abhijeet:' >> /etc/nginx/.htpasswd"
```
Crea dos usuarios, uno con tu nombre y otro con tu apellido.
Comprueba que el usuario y la contraseña aparezcan cifrados en el fichero:
```
cat /etc/nginx/.htpasswd
```

#### Configurando el servidor Nginx para usar autenticación básica
editamos el archivo de server block: 
```
sudo nano /etc/nginx/sites-available/nom_web
```
y editamos dentro de `location / {...}`:
```
server {
    listen 80;
    listen [::]:80;

    root /var/www/abhijeet/html/perfect-learn;
    index index.html index.htm index.nginx-debian.html;
    server_name tasca;
    location / {
      auth_basic "Àrea restringida";
      auth_basic_user_file /etc/nginx/.htpasswd;
      try_files $uri $uri/ =404;
    }
}
```
y reiniciamos el servidor.
```
sudo systemctl restart nginx
```

#### Combinacion de la autenticación basica con restricciones de IP
dentro de server block añadiremos:

Restricciones y accesso a IP's:
```
server {
    ...
    location / {
      deny 198.192.1.10;
      allow 192.168.1.1/24,
      allow 127.0.0.1;
      deny all;
    }
}
```
Combinar IP y la autenticación HTTP:
```
server {
    ...
    location / {
      satisfy all;

      deny 198.192.1.10;
      allow 192.168.1.1/24,
      allow 127.0.0.1;
      deny all;

      auth_basic              "Administrator's area"
      auth_basic_user_file    conf/htpasswd;
    }
}
```