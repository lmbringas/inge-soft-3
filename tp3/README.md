# SISTEMA DISTUIBUIDOS

¿Cuáles puertos están abiertos?
```
➤ docker ps          
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                    NAMES
457946d407da        alexisfr/flask-app:latest   "python /app.py"         3 minutes ago       Up 3 minutes        0.0.0.0:5000->5000/tcp   web
2d66965cc8e9        redis:alpine                "docker-entrypoint.s…"   3 minutes ago       Up 3 minutes        6379/tcp                 db

```

Mostrar detalles de la red mybridge con Docker.
```
➤ docker network inspect mybridge
[
    {
        "Name": "mybridge",
        "Id": "e21606ced3f8b281dc0379b7b93fd8123099d9e18275797d47ccbf17b1e34a58",
        "Created": "2020-09-07T15:00:15.20840709-03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.25.0.0/16",
                    "Gateway": "172.25.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "2d66965cc8e9851411d78b59126c1f570c3631b87b23e9179ac8b3041c10ce3f": {
                "Name": "db",
                "EndpointID": "ebb4e39954932f26547e905cdc0ee6382bdce9dc80115d27e3848d27acef1125",
                "MacAddress": "02:42:ac:19:00:02",
                "IPv4Address": "172.25.0.2/16",
                "IPv6Address": ""
            },
            "457946d407dac1e25a636ef5d89abd1e0bde8405d546d22a91db4f697b239f3b": {
                "Name": "web",
                "EndpointID": "ac0559d12039e0ff7b9f3f4e768913dba0723bab16afa70b107310fedf8ebf15",
                "MacAddress": "02:42:ac:19:00:03",
                "IPv4Address": "172.25.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

## ANALIZANDO EL SISTEMA

La imagen ```alexisfr/flask-app:latest```  corre una aplicacion en flask y se conecta con una base de datos redis. 
Luego crea un endpoint en ```/``` y  por cada vez que se visita ese endpoint  el programa incrementa uno la key ```hits``` 

### ¿Para qué se sirven y porque están los parámetros -e en el segundo Docker run del ejercicio 1?

Son variables de entornos que se le pasan a la aplicacion asi puede buscar la host de redis y su puerto.

### ¿Qué pasa si ejecuta docker rm -f web y vuelve a correr docker run -d --net mybridge -e REDIS_HOST=db -e REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest ?
Se mantiene la cantidad de hits. Ya que la db en redis se mantiene.

### ¿Qué occure en la página web cuando borro el contenedor de Redis con docker rm -f db?
Se obtiene un error en la aplicacion.

### Y si lo levanto nuevamente con docker run -d --net mybridge --name db redis:alpine ?
Vuelve a cero el contador.

### ¿Qué considera usted que haría falta para no perder la cuenta de las visitas?
Se deberia generar un volumen que persista los datos de db redis.

## UTILIZANDO DOCKER COMPOSE 

### ¿Qué hizo Docker Compose por nosotros? Explicar con detalle.

Creo todos los pasos manuales que realizamos anteoriormente. Esto se puedo hacer gracias al archivos ```docker-compose.yml``` que describe cuales son los servicios que van a estar corriendo, tambien crea una red interna entre los servicios donde pueden verse y tambien genera un volumen para persistir los datos de la base de datos de redis.


## example-voting-app

La aplicacion ```example-voting-app``` cuenta con 5 servicios que trabajan en cojunto. 

![services](https://dockerlabs.collabnix.com/play-with-docker/example-voting-app/architecture.png)

La app en python se encarga de enviar votos a la base de datos de redis, el worker escrito en java se encarga de tomar los datos de la db en redis y las persiste en la base de datos postgres. La aplicacion node esta constantemente sacando datos de db en postgres (dato de color luego de cierto tiempo la aplicacion en node no va funcionar mas ya que tiene un retry de 1000 veces).  