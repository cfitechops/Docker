###### Dokcer Lab for Bind Mount

```sh
mkdir /data-bind

docker run -d --name Bmount-container1 --mount type=bind,source=/data-bind,target=/app nginx:latest

docker inspect Bmount-container1 | grep -A 9 "Mounts" | tail

docker inspect Vol-mount-container2 | grep -A 9 "Mounts" | tail

docker container exec -it Bmount-container1 /bin/bash

cd /app/

ls -ltr

cat > file1.txt
Hi 
This is my tes file from container Bmount-container1
^C

cat file1.txt

exit

ls -l /data-bind/

cat /data-bind/file1.txt

cat >> /data-bind/file1.txt
this update is done from hostmachine.
^C

cat /data-bind/file1.txt

docker container exec -it Bmount-container1 cat /app/file1.txt

docker container rm -f Bmount-container1

cat /data-bind/file1.txt

[root@docker1 ~]# cat /data-bind/file1.txt
Hi
This is my tes file from container Bmount-container1
this update is done from hostmachine.
[root@docker1 ~]#

docker run -d --name Bmount-container2 --mount type=bind,source=/tmp,target=/usr/ nginx:latest

docker container exec -it Bmount-container2 /bin/bash

docker container rm -f Bmount-container2

docker run -d -it --name Bmount-v-container1 -v /data-bind:/app nginx:latest
```

