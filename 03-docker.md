# LAB

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
