**Docker Container Port mirroring**

vi Dockerfile

```sh
FROM ubuntu:20.04
LABEL maintainer="Thierno Barry"
ENV TZ=Europe/Brussels
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get -y install apache2 vim curl elinks && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
ADD myfile.tar /var/www/container1/
COPY index.html /var/www/container1/
RUN cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/container1.conf && \
    sed -i 's=DocumentRoot /var/www/html=DocumentRoot /var/www/container1/=' /etc/apache2/sites-available/container1.conf && \
    sed -i '/container1/ a \\tServerName localhost' /etc/apache2/sites-available/container1.conf && \
    a2ensite container1 && \
    a2dissite 000-default.conf
ENV MY_VAR="This is my server"
ENV PASSWD="HismyPass"
WORKDIR /data/
EXPOSE 8080
VOLUME /myvol
ENTRYPOINT ["bash", "-c", "service apache2 restart && exec bash"]
```

```sh
docker image build -t mywebserver1 .

docker container run -it -d --name container1 -p 3901:80 mywebserver1
docker container run -it -d --name container2 -p 3902:80 mywebserver1
```


```sh
docker exec -it container2 /bin/bash

cd /var/www/container1/

cat index.html

sed -i 's/Container1/container2/' index.html

cat index.html

service apache2 reload

docker container inspect container1 | grep -i ipaddress

docker container inspect container2 | grep -i ipaddress

ifconfig docker0

docker container exec -it container1 ping 172.17.0.3 -c 4

docker container exec -it container2 /bin/bash

ping 172.17.0.3

exit

docker container exec -it container2 ping 172.17.0.2
```

**LAB for None Network Driver**
 
```sh
docker network ls

docker container ls -a

for i in $(docker container ls -aq) ; do docker container rm -f $i ;done

docker container run -itd --net none --name none-container1 mywebserver1

docker container run -it -d --network none --name none-container2 mywebserver1

docker network inspect none

docker container exec -it none-container1 /bin/bash

#ifconfig >>> You will notice no eth0 interface
#ip route -> No route will show.
```

