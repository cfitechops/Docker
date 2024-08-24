###### Lab for Docker Volume:

```sh
docker container ls -a
for i in $(docker container ls -aq) ; do docker container rm -f $i ;done

docker run -d --name container1 nginx:latest
docker container exec -it container1 /bin/bash

cat > file.txt

Hi
This is my test file.

ctl+c

cat file.txt

exit

docker container inspect container1  #--> Check the mount option
container exec -it container1 /bin/bash
cat file.txt
exit

docker container rm -f container1

docker run -d --name container1 nginx:latest
docker container exec -it container1 /bin/bash
cat file.txt
exit

docker volume ls
docker volume create my-voloume1
docker volume ls
docker volume inspect my-voloume1
docker volume rm my-voloume1
docker volume ls
```

###### If the volume is not yet created then Docker will create for you.

```sh
docker run -d --name Vol-mount-container1 --mount source=my-voloume1,target=/app nginx:latest
docker container inspect Vol-mount-container1
docker inspect Vol-mount-container1 >> Check the Month options #--> "Mode=z The 'z' flag tells Docker that the volume content will be shared between containers.

docker container exec -it Vol-mount-container1 /bin/bash

cd /app/

cat > Vol-mount-container1.txt
Hello
This is the raw file of Vol-mount-container1

ctl+c

cat Vol-mount-container1.txt

docker volume inspect my-voloume1

cat /var/lib/docker/volumes/my-voloume1/\_data/Vol-mount-container1.txt

docker run -d \
 --name Vol-v-container1 \
 -v my-voloume1:/app \
 nginx:latest

docker container inspect Vol-v-container1
docker container exec -it Vol-v-container1 /bin/bash

cd /app/

cat > Vol-v-container1
Hello
this is the raw file of Vol-v-container1

ctl+c

cat Vol-v-container1

docker container ls
docker container exec -it Vol-mount-container1 ls /app/  # ---> You will see two files, here.
docker container exec -it Vol-mount-container1 cat /app/Vol-v-container1

for i in $(docker container ls -aq) ; do docker container rm -f $i ;done

docker container ls -a

docker run -d --name Vol-mount-container2 --mount source=my-voloume1,target=/app nginx:latest
docker container exec -it Vol-mount-container2 ls /app/
docker container exec -it Vol-mount-container2 cat /app/Vol-v-container1
```
