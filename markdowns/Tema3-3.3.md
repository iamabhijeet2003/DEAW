# Tema 3
## Despliegue de una aplicacion clusterizada con Node Express

### Primero sin Cluster

- creamos una aplicacion de prueba , por ejemplo `pruebaSinCluster.js`:
```
const express = require("express");
const app = express();
const port = 3000;
app.get("/", (req, res) => {
    res.send("Hello World!");
});
app.get("/api/:n", function (req, res) {
        let n = parseInt(req.params.n);
        let count = 0;

        if (n > 5000000000) n = 5000000000;

        for (let i = 0; i <= n; i++) {
        count += i;
    }

    res.send(`Final count is ${count}`);
});

 app.listen(port, () => {
 console.log(`App listening on port ${port}`);
 });

```

- Creamos un directorio para este proyecto.
- ejecutamos dentro del directorio:
```
npm init
``` 
- instalamos Express :
```
npm install express
```

iniciamos la apicacion:
```
node pruebaSinCluster.js
```
para comprobar accedemos a `http://IP-maq-virtual:3000`

ahora compruebamos con 2 valores diferentes en 2 pestañas de navegador abiertas:
- primero con n=50
`http://IP-maq-virtual:3000/50`

- despues con n=50000000
`http://IP-maq-virtual:3000/50000000`

podemos comprobar que con el n=5000000 tarda mucho mas, porque la aplicacion no esta clusterizada.

## Con cluster

- creamos otra aplicacion  , por ejemplo `pruebaConCluster.js`:
```
const express = require("express");
const port = 3000;
const cluster = require("cluster");
const totalCPUs = require("os").cpus().length;
if (cluster.isMaster) {
console.log(`Number of CPUs is ${totalCPUs}`);
console.log(`Master ${process.pid} is running`);
 // Fork workers.
 for (let i = 0; i < totalCPUs; i++) {
 cluster.fork();
 }

 cluster.on("exit", (worker, code, signal) => {
 console.log(`worker ${worker.process.pid} died`);
 console.log("Let's fork another worker!");
 cluster.fork();
 });
 } else {
  const app = express();
 console.log(`Worker ${process.pid} started`);

 app.get("/", (req, res) => {
 res.send("Hello World!");
 });

 app.get("/api/:n", function (req, res) {
 let n = parseInt(req.params.n);
 let count = 0;

 if (n > 5000000000) n = 5000000000;

 for (let i = 0; i <= n; i++) {
 count += i;
 }

 res.send(`Final count is ${count}`);
 });

 app.listen(port, () => {
 console.log(`App listening on port ${port}`);
 });
 }
```
Ahora hacemos el mismo procedimiento que la aplicacion sin cluster y compruebamos la velocidad de carga de esta aplicacion Clusterizada.


## Metricas de rendimiento

Instalamos `loadtest` que nos permite simular una gran cantidad de conexiones simultáneas a la nuestra API para que podamos medir su rendimiento.

```
npm install -g loadtest
```
Mientras ejecutamos la aplicación, en otro terminal realizamos la siguiente prueba de carga:
```
loadtest http://localhost:3000/api/500000 -n 1000 -c 100
```
El comando anterior enviará 1000 solicitudes a la URL dada, de las que 100 son concurrentes.


Ahora detenemos la aplicación sin clusters y ejecutamos:
``` 
node nombre_aplicacio_cluster.js 
``` 
Ejecutaremos exactamente las mismas pruebas con el objetivo de realizar una comparación

