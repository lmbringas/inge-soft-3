# MICROSERVICE 

### Describa los contenedores creados, indicando cuales son los puntos de ingreso del sistema
El punto de entrada al sistema es el contenedor de edge-route que es un traefik que redirecciona las request al servicio de fronted, el cual es nuestra API gateway.

### Por qué cree usted que se está utilizando repositorios separados para el código y/o la configuración del sistema? Explique puntos a favor y en contra.

Este es un sistema pensando como microservicios donde cada uno tiene sos responsabilidad.

```Puntos a favor de usar Multi-Repo```

* El codigo, sus configuraciones, y su contenedores se mantienen un mismo lugar lo que facilita el deploy y no genera conflicto con configuraciones de algun otro servicio.
* Un cambio al codigo fuente solo haria un build de ese servicio y no de la aplicacion entera.
* Se genera independencia entre los servicios y una mejor forma de estructurar la comunicacion entre los mismos. Osea mejora la forma de pensar cada microservicio.
* Mejora de CI se puede ajustar mejor a cada repositorio.
* Los grupos de trabajos son independientes.


```Puntos en contra de usar Multi-Repo```

* Duplicacion de codigo, para resolver un problema en comun diferentes grupos generan la misma solucion.
* En un escenario de monorepo se tiene una vision general del proyecto de manera mas directa.
* En un escenario de monorepo se simplifican las dependencias ya que la dependencias de un microservice puede ser la misma que otro.
* Se puede complicar para la gente nueva poder entender el sistema.
* Es mas dificil seguir las mejores practicas si cada uno hace lo que quiere.

### ¿Cuál contenedor hace las veces de API Gateway?
El contenedor de ```front-end```

### ¿Cuál de todos los servicios está procesando la operación? ```curl http://localhost/customers curl http://localhost/catalogue curl http://localhost/tags``` 

Los siguientes request entran por nuestra API Gateway que es el encargado de redireccionar a los respectivos servicios.
El servicio de ```user``` procesa la request ```/customers```
El servicio de ```catalogue``` procesa las requests ```/catalogue``` y ```/tags```

### ¿Como perisisten los datos los servicios?
Cada servicio tiene su propia capa de persistencia.

### ¿Cuál es el componente encargado del procesamiento de la cola de mensajes?
El servicio ```queue-master``` es el encargado de procesar la cola de mensajes.

### ¿Qué tipo de interfaz utilizan estos microservicios para comunicarse?
Utilizan todos APiREST.