# Resources
https://github.com/ricardoandre97/docker-es-resources

# IMAGES
## Build
docker build -t {tag}:{version} -f {dockerfile_name}

### Ejemplos:
1. docker build -t apache-centos:apache-cmd . (El punto busca automaticamente el dockerfile en la carpeta)
2. docker build -t joaquin/posts:0.0.1 .

# Delete
docker rmi {id} o {nombre}:{tag}

# Remove all images
docker rmi $(docker images -q)

## Other commands
docker images -f dangling=true
docker images -f dangling=true -q
docker images -f dangling=true -q | xargs docker rmi
docker images | grep {nombre}


# Ejemplo Dockerfile
```shell
FROM centos:7 # Image

LABEL version=1.0 # Information
LABEL vendor="nadie"
LABEL description="apache centos image"

RUN yum install httpd -y # Run inside container

# COPY startbootstrap-freelancer-gh-pages /var/www/html sin usar el WORKDIR
COPY startbootstrap-freelancer-gh-pages html # Copy outside info inside containers

ENV contenido prueba # Añade variables de entorno

# Es buena practica escribir todo en menos capas.
RUN echo "${contenido}" > /var/www/html/prueba.html && \
    echo "$(whoami)" > /var/www/html/user1.html && \
    useradd joaquin

USER joaquin # Usar el user joaqiuin

RUN echo "$(whoami)" > /tmp/user2.html

USER root

RUN cp /tmp/user2.html /var/www/html/user2.html

# CMD apachectl -DFOREGROUND

COPY run.sh run.sh

CMD sh run.sh
```
