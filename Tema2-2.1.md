## Instalación y Configuración de servidor web Nginx

#### Instalación servidor 
Instalamos nginx:
```
sudo apt update
sudp apt install nginx
```
Compruebamos si se ha instalado correctamente
```
sudo systemctl status nginx
```
Cremaos una carpeta del sitio web / dominio:
```
sudo mkdir -p /var/www/nombre_de_la_web/html
```
dentro de la carpeta creada anteriormente clonamos el repositorio:
`
https://github.com/cloudacademy/static-website-example
`

asignamos como propietario a www-data :
```
sudo chown -R www-data:www-data /var/www/nombre_de_la_web/html
```
y le damos los permisos adecuados:
```
sudo chmod -R 755 /var/www/nombre_de_la_web
```
podemos comprobar si esta funcionando el servidor entrando en:
```
http://IP-maquina-virtual
```
y deberia aparecer la pagina **default** de **nginx** 

#### Configuracion del servidor web
creamos nuevo archivo en en `/etc/nginx/sites-available/nombre_pagina_web`
```
sudo nano /etc/nginx/sites-available/nombre_pagina_web
```
y contendrá:
```
server {
    listen 80;
    listen [::]:80;
    root /ruta/absoluta/archivo/index; #como /var/www/ejemplo2/html/2016_soft_landing
    index index.html index.htm index.nginx-debian.html;
    server_name nombre_web;
    location / {
      try_files $uri $uri/ =404;
    }
}
```
y ahora creamos un enlace simbolico:
```
sudo ln -s /etc/nginx/sites-available/nombre-web /etc/nginx/sites-enabled/
```
y reinciamos el servidor:
```
sudo systemctl restart nginx
```

#### Comprobaciones 
Tenemos que introducir el nombre y el ip de la pagina web en la maquina anfitriona/real en `/etc/hosts`
y añadir la linea `ip_maquina_debian nombre_web`

ahora podemos compribar acceder a la pagina y comprobar los logs posteriormente en:
```
/var/log/nginx/access.log
/var/log/nginx/error.log
```

#### FTP 
en pdf de los apuntes
