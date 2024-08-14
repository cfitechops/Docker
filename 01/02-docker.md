**How to create an Image from Container or from DockerFile**

**How to configure web server**

```sh
docker run -it --name container1 ubuntu
```

**Below commands need to te executed on container.**

```sh
apt-get update

apt-get -y install apache2 vim curl elinks

sed -i '$a\ServerName 127.0.0.1' /etc/apache2/apache2.conf

mkdir /var/www/container1/

cd /var/www/container1/

cat > index.html <<EOF
<html>
<head>
  <title> Docker Container1 of Ubuntu! </title>
</head>
<body>
  <p> I'm running this website on Docker Container1 on Ubuntu OS
</body>
</html>
EOF

cd /etc/apache2

service apache2 start

cd /etc/apache2/sites-available/

cp 000-default.conf container1.conf

cat container1.conf

sed -i 's=DocumentRoot /var/www/html=DocumentRoot /var/www/container1/=' container1.conf

sed -i '/container1/ a \\tServerName localhost' container1.conf

a2ensite container1.conf

a2dissite 000-default.conf

service apache2 reload

service apache2 start

elinks http://localhost

CLT+P+Q
```

**How to create Image from running Container**

```sh
docker commit container1 apache1-ubuntu
docker image ls

> We can delete the container1 and re-build the container from new image i.e. apache1-ubuntu.
> Remove the container and check if we get all the information from the image that we build from previous container.

docker container rm -f container1

docker run -it --name container1 apache1-ubuntu

root@e7d0ab7090d3:/# service apache2 start
root@e7d0ab7090d3:/# elinks http://localhost
Ubuntu rocks!
I'm running this website on Docker Container1 on Ubuntu OS , Cheers!

CLT P + CLT Q

docker run -itd --name container2 apache1-ubuntu
docker exec -it container2 sed -i 's/Container1/Container2/' /var/www/container1/index.html
docker exec -it container2 service apache2 start
docker exec -it container2 service apache2 reload
docker exec -it container2 service apache2 status

docker container inspect container1 | grep -i ipaddress
docker container inspect container2 | grep -i ipaddress
docker exec -it container1 /bin/bash

ping 172.17.0.3 > This should not work because PING command not available
apt-get install iputils-ping -y
ping 172.17.0.3

> From the base machine
> curl http://172.17.0.3
> curl http://172.17.0.2
```

###### LAB

**How to create Image from DockerFile**

```sh
mkdir /data

cd /data/

touch hostfile{1..5}.txt

ls -ltr

cat > index.html <<EOF
<html>
<head>
  <title> Docker Container1 of Ubuntu! </title>
</head>
<body>
  <p> I'm running this website on Docker Container1 on Ubuntu OS
</body>
</html>
EOF

tar cvfz myfile.tar*

cat > file1.txt <<EOF
This is my first file.
EOF

ls -ltr

rm -f hostfile*

rm -f index.html

ls -ltr
```

vi Dockerfile

**Usage: FROM [image_name]**

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

docker image ls

docker container run -itd --name mynewcontainer1 mywebserver1 /bin/bash

docker container exec -it mynewcontainer1 /bin/bash # > check the working Directory

cd /var/www/container1/

ls -ltr

cat file1.txt

curl http://localhost

exit

docker container inspect mynewcontainer1 | grep volume

docker volume ls

docker container ls -a
```

**How to push the Image to Dockerhub**

```sh
docker login

docker image ls

docker image tag mywebserver1 cfitech/learning-docker:mywebserver1

docker image ls

docker push cfitech/learning-docker:mywebserver1

docker image rm -f cfitech/learning-docker:mywebserver1

docker container run -itd --name mynewcontainer2 cfitech/learning-docker:mywebserver1 /bin/bash

docker image ls
```
