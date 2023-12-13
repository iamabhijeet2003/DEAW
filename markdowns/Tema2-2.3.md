# Proxy Inverso con Nginx

## Servidor web de Nginx
Paso 1 es Clonar el debian donde tenemos configurado el servidor nginx

El nuevo debian servira para el proxy inverso

Realizaremos las peticiones desde nuestra maquina anfitriona contra el servidor de nginx del proxy
> Hay que tener en cuenta que al clonar hay que crear nuevas direcciones MAC para evitar problemas posteriormente.


Para diferenciar entre las maquinas realizamos una serie de pasos:
- Cambiar el nombre de web por el nombre `webserver` y eso implica:
    - cambiar el nombre del archivo de configuracion de `sites-available` para nginx.
    - cambiar nomvres dentro del archivo donde haga falta.
    - hacer un `unlink` dentro de directorio `/etc/nginx/sites-enabled`
    - crear el nuevo enlace simbolico 
- Ahora cambiar el puerto en el archivo de configuracion de 80 a 8080 `listen 8080`
- reinicar nginx `sudo systemctl restart nginx`


## Proxy inverso Nginx
- creamos nuevo archivo de configuracion en `sites-available` con el nombre por ejemplo : `exemple-proxy`
- la configuracion seria:
    ```
        listen 80;
        server_name ______; #el nombre del sitio web
        location / {
            proxy_pass http://ip_maquina_del_webserver:8080;
        }
    ```
- crear el link simbolico pertinente.