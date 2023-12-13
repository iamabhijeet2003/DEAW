# 3.1 Instalación de Tomcat y Maven para el despliegue de aplicaciones java

> Para esta practica usaremos la version 9 de tomcat

## Instalación Tomcat

Primero que todo instalamos ufw para cambiar el puerto a Tomcat
```
sudo apt update
sudo apt install ufw
```
ahora queremos que el puerto por defecto del Tomcat sea 8080.
```
sudo ufw allow 8080
```
ahora actualizamos los paquetes de nuevo : 
```
sudo apt update
```
instalamos Java:
```
sudo apt install openjdk-11-jdk -y
```

## Instalación Apache Tomcat
Instalamos apache tomcat:
```
sudo apt install tomcat9 -y
```
añadimos el grupo tomcat9 :
```
sudo groupadd tomcat9
```
añadimos el grupo: 
```
sudo useradd -s /bin/false -g tomcat9 -d /etc/tomcat9 tomcat9
```

tomcat esta instalada iniciamos y compruebamos:
```
sudo systemctl start tomcat9
sudo systemctl status tomcat9
```

ahora definimos el usuario:
```
sudo nano /etc/tomcat9/tomcat-users.xml
```
y ponemos:
```
<role rolename="admin"/>
<role rolename="admin-gui"/>
<role rolename="manager"/>
<role rolename="manager-gui"/>

<user username="debian11abhiexam" password="abhijeet" roles="admin,admin-gui,manager,manager-gui"/>
```

ahora compruebamos el accesso a Tomcat:
```
http://localhost:8080
```

Ahora instalamos el tomcat admin :
```
sudo apt install tomcat9-admin
```

y ahora entramos en :
```
http://localhost:8080/manager/html
```
y tambien en.
```
http://localhost:8080/host-manager/html
```

## Despliegue manual desde GUI de administrador de Tomcat

Descargamos el archivo WAR : (aqui)[https://github.com/iamabhijeet2003/DEAW/blob/main/Archivos/EjemploPruebaCarga.war]

Hacemos el login

Buscamos la sesion de despliegue manual

Subimos el archivo y se desplegara.

