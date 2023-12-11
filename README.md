# DAW (Despliegue de Aplicaciones Web)

---
## Tema 1

### Instalacion y configuracion de maquina virtual.

conectar  
```
ssh -l abhijeet ip-maquina
```

Canviant del nostre usuari a l’usuari root:  
```
su root
```

Entramos en visudo:  
```
/usr/sbin/visudo
```

y editamos el archivo debajo de
``` 
# User privilegdes
root ALL=(ALL:ALL) ALL
abhijeet ALL=(ALL:ALL) ALL
```
compruebamos si somos root : 
```
sudo -v
```
o de otra forma : 
```
timeout 2 sudo id && echo Access granted || echo Access denied
```

si hay permisos correctos devolvera: `uid=0(root) gid=0(root) grupos=0(root) Access granted`

### Configuración
Creamos la llave de accesso:
```
ssh-keygen -b 4096
```
Creamos la copia en la MV:
```
ssh-copy-id abhijeet@IP-maquina
```
para evitar problemas damos permisos en debian:
```
chmod 700 .ssh/
chmod 600 .shh/authorized_keys
```
---
## Tema 2

### Instalación y Configuración de servidor web Nginx

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

### Autenticación en Nginx
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