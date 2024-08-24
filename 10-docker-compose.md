# Example 1

vi docker-compose.yaml

```sh
version: "3"
services:
  webserver1:
    image: nginx
    ports:
      - 8000:80
  webserver2:
    image: nginx
    ports:
      - 8001:80
```

```sh
docker-compose ls
docker-compose up -d
docker container ls
docker container inspect <CONTAINER ID> | grep -i ip
curl <IPAddress>
curl localhost:8001
docker container ps
docker container stop
docker container ps
docker container ls
docker container ls -a
docker container restart
docker container pause
curl localhost:8001
docker container unpause
docker container stop
docker container rm
```

# Example 2

vi Dockerfile

```sh
FROM centos:7.9.2009

LABEL Description="This Dockerfile is to create custom docker images by following best pratices CFITECH."

ARG DESIGNATION="Cfitech.OPS"
ARG NAME Cfitech
ARG PASS

ENV USERNAME="cfitech" \
    COMPANY_NAME="IT" \
    COMPANY_EMAIL="info@cfietch.be" \
    CONTACT="$COMPANY_EMAIL" \
    PASSWORD $PASS

RUN echo "Hello $NAME, are you a $DESIGNATION, YES I AM" > /tmp/args-calling.txt \
    && pwd > /tmp/pwd.txt \
    && mkdir /tmp/cfitech

COPY file /opt/files

WORKDIR /opt

ADD https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.47/bin/apache-tomcat-7.0.47.tar.gz /opt
ADD apache-tomcat-7.0.47.tar.gz /opt/
ADD add-file.txt /tmp
ADD files /tmp/myfiles

VOLUME /data
ENTRYPOINT ["bash"]
```

vi docker-compose.yaml

```sh
version: "3"
services:
  db:
    image: mysql
    container_name: mysql_db
    environment:
      - MYSQL_ROOT_PASSWORD="secret"
    volumes:
      - mysqldata:/var/lib/mysql
    networks:
      - cfitechwork1
  webserver:
    image: nginx
    depends_on:
      - db
    container_name: mywebserver
    ports:
      - 8000:80
    networks:
      - cfitechwork1
  centos:
    image: centos:7.9.2009
    container_name: mycentos
    environment:
      - OS_TYPE= 'centos'
    volume:
      - /tmp/compose-vol-local:/tmp/vol-mount-path #Bind Mount
    networks:
      - cfitechwork2
  ubuntu:
    build:
      container: .
      dockerfile: Dockerfile
      args:
        - PASS=secret
    networks:
      - cfitechwork2
    image: compose-ubuntu-image:v1

networks:
  cfitechwork1
  cfitechwork2

volumes:
  mysqldata:
```

```sh
docker-compose up -d
docker container ls
docker network ls
docker volume ls
curl localhost:8000
docker-compose down
```
