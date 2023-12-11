# Tema 1

## Instalacion y configuracion de maquina virtual.

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