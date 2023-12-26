# Tema 3
## Despliegue de una aplicacion clusterizada con Node Express

### Primero sin Cluster

creamos una aplicacion de prueba , por ejemplo `pruebaSinCluster.js`:
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

