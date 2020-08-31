# DOCKER 

Al ejecutar el siguiente comando no se ejecuta nada ya que el entrypoint del container espera argumentos para ser ejecutados.

``` 
docker run busybox

```


El siguiente comando nos muestra lista de container junto con su id, y su estado entre otros datos.
``` 
docker ps -a

```

![docker ps](assets/docker_ps.png)

```
docker run -it busybox sh
```

![Interactive](assets/interactive.png)

