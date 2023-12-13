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

### Despliegue manual desde GUI de administrador de Tomcat

Descargamos el archivo WAR : (aqui)[https://github.com/iamabhijeet2003/DEAW/blob/main/Archivos/EjemploPruebaCarga.war]

Hacemos el login

Buscamos la sesion de despliegue manual

Subimos el archivo y se desplegara.

---
# Instalacion de maven

Instalamos maven con APT:
```
sudo apt update
sudo apt install maven
```
y compruebamos la version:
```
mvn --v
```
Ahora tenemos que añadir el rol de manager-script para permitir a que maven se autentique contra tomcat

modificamos :
```
/etc/tomcat9/tomcat-users.xml
```
y le añadimos:
```
<role rolename="admin"/>
<role rolename="admin-gui"/>
<role rolename="manager"/>
<role rolename="manager-gui"/>

<user username="debian11abhiexam" password="abhijeet" roles="admin,admin-gui,manager,manager-gui"/>
<user username="abhijeet-deploy" password="abhijeet" roles="manager-scripts"/>
```
editamos el archivo : 
```
/etc/maven/settings.xml
```
y añadimos.
```
<servers>
    <server>
        <id>Tomcat.P.3.1</id>
        <username>abhijeet-deploy</username>
        <password>abhijeet</password>
    </server>
</servers>
```

ahora creamos la aplicacion de prueba:
```
mvn archetype:generate -DgroupId=guillermo -DartifactId=war-deploy -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

ahora modificamos de nuevo el pom.xml:
```
<build>
    <finalName>war-deploy</finalName>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
                <url>http://localhost:8080/manager/text</url>
                <server>Tomcat.P.3.1</server>
                <path>/myapp</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```
