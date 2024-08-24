###### Docker lab for tmpfs

```sh
for i in $(docker container ls -aq) ; do docker container rm -f $i ;done

docker volume ls

docker run -d -it --name tmpt-container1 --mount type=tmpfs,destination=/app nginx:latest

docker container exec -it tmpt-container1 /bin/bash

cd /app/

ls -ltr

echo "This is a test file in tmpt-container1 file." > tmpt-container1.txt

cat tmpt-container1.txt

exit

docker container inspect tmpt-container1

docker cp /etc/resolv.conf tmpt-container1:/tmp

docker container exec -it tmpt-container1 cat /tmp/resolv.conf

docker run -d -it --name tmpt-container2 --mount type=tmpfs,target=/app nginx:latest

docker run -d -it --name tmpt-container3 --mount type=tmpfs,target=/app,tmpfs-size=500 nginx:latest

docker run -d -it --name tmpt-container4 --mount type=tmpfs,target=/app,tmpfs-size=500,tmpfs-mode=1777 nginx:latest
``` 