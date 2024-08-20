###### LAB for HOST Network Driver

```sh
docker network ls

docker network inspect host

docker container run -it -d --net host --name host-container1 mywebserver1

docker network inspect host

docker container exec -it host-container1 /bin/bash

ip a

cd /var/www/container1

sed -i 's/Docker Container1/Docker host network container1/' index.html

service apache2 reload
```

###### Exit from the container

```sh
curl http://localhost

docker container run -it -d --net host --name host-container12 mywebserver1

docker container ls -a | grep “host-”   # >>> you will notice the container will be on Exit status.

docker container ls -a | grep host

docker container logs host-container12  # --> Port already used error message.
```
