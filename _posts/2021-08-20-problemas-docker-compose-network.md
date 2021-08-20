---
layout: post
title: Problemas con Docker-compose network zombie
---

Si tienes problemas con puertos ocupados en docker esto te puede interesar.

Si al intentar levantar tu container no te lo permite porque los puertos están ocupados, verifica que servicio está levantado en el puerto mencionado.

```sh
sudo netstat -tulpn | grep :8000
```

Seguramente la respuesta será parecida a esta, donde el último valor es el número de proceso y el servicio que lo está ocupando.

```sh
tcp6       0      0 :::8000                 :::*                    ESCUCHAR    16635/docker-proxy  
```

Podemos intentar cerrar un proceso con el comando kill -9 y el número de proceso.

```sh
sudo kill -9 16635
```

Si esto no funciona, podemos intentar un docker-compose down, tener cuidado porque puedes perder información, debido a que este comando no solo hace un stop del container, sino que además elimina los stopped containers como las networks que se crearon.

```sh
docker-compose down
```

Y por último otra posibilidad puede ser eliminar el archivo local-kv.db

```sh
 sudo service docker stop
 sudo rm /var/lib/docker/network/files/local-kv.db
 sudo service docker start
```