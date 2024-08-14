#How to check the images downloaded?
docker image ls

#Difference between Create and RUN command?

docker container create --name container1 -i ubuntu
docker container ls -a | grep ubuntu
docker container start container1
docker container ls -a | grep ubuntu

docker container attach container1
df -h
exit
docker container ls -a | grep ubuntu
docker container rm container1

docker container run -it --name container1 ubuntu bash
df -h
exit

# here one can observed that with the help of "run" and "iT" option, we have not only created the container1 but also start and login into the container.

docker container ls
docker container ls -a | grep container1
docker container rm container1

# Once we exit from the container, this container shutdown, why?

docker container run -it --name container1 ubuntu bash

#Now, this time, I am going to exit with CLT+P and CLT+Q button.

docker container ls

# With ls option, it will show only the running container names. and with "-a" option for all.

docker container rm -f container1

docker image ls
date ; docker container run ubuntu:20.04 ; date ---> very less time

# Notice that we haven't given the name of our containter and Docker engine give us the fancy name.

docker container ls -a

docker container run --name container1 ubuntu
docker image ls
docker container ls -a

docker container run ubuntu:20.04
docker container run ubuntu:20.04
docker container run ubuntu:20.04

docker container rm
docker container run ubuntu:20.04
docker container run ubuntu:20.04
docker container rm $(docker container ls -aq)
docker container run -it --name container1 ubuntu
docker container ls -a
docker container attach container1
exit
docker container ls -a
docker container start container1
docker container ls -a

docker container run -d -it --name container2 ubuntu
docker container ls -a

docker container top container1
docker container ps

docker container run -it --rm --name container3 ubuntu cat /etc/resolv.conf
docker container ls -a
docker container ls --help
docker container ls -aq
