# Docker Containers

```sh
docker ps
docker ps -a #(Contenedores detenidos)
```

## List all containers (only IDs)

docker container ls

docker container ls --all

```sh
docker ps -aq
```

## Stop all running containers

```sh
docker stop $(docker ps -aq)
```

## Remove all containers

```sh
docker rm $(docker ps -aq)
```

## Run

Con -d se ejecuta en segundo plano
Con -e se le pueden pasar variables de entorno

```sh
docker run -d -e "variable=valor" {image_name} 
```

Se pueden mapear los puertos 80:80

```sh
docker run -d -p 80:80 --name apache apache-centos:apache-cmd 
docker run --rm -ti centos bash
```

## Stop

```sh
docker container stop {c_name} Stop a running container through SIGTERM
```

## KILL

```sh
docker container kill {c_name} Stop a running container through SIGKILL
```

## Shell en contenedores

Para entrar a la consola bash del contenedor docker

```sh
docker exe -ti {docker_name} bash (Si esque acaso tiene bach)
```

Para entrar al bash con un usuario especifico

```sh
docker exe -u root -ti {docker_name} bash // -u {user_name} 
```

## Otros comandos

```sh
docker logs {container_id} // devuelve el ultimo log
docker logs -f apache // Se queda escuchando el log
docker rm -f apache # BORRAR CONTENEDOR
docker rename {old_name} {new_name} // use docker ps -a to view docker names #RENAME DOCKER CONTAINER
docker restart {docker_name}
docker inspect {docker_name}
```