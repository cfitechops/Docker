###### LAB for Bridge / custom bridge Network Driver

```sh
for i in $(docker container ls -aq ) ; do docker container rm -f $i ;done
```

## Default Bridge driver

```sh
docker container run -it -d --name cut1-container1 -p 3901:80 mywebserver1

docker container run -it -d --name cut1-container2 -p 3902:80 mywebserver1

docker container inspect cut1-container2 | grep -i ipaddress

docker container run -it -d --name cut2-container1 -p 3903:80 mywebserver1

docker container inspect cut2-container1 | grep -i ipaddress

docker network inspect bridge

docker container exec cut1-container1 ping -c 2 172.17.0.4

docker container exec -it cust1-container1 /bin/bash

curl http://172.17.0.4
```

### How to create custom network bridge

```sh
docker network create testbridge1
docker network inspect testbridge1
docker network inspect bridge | grep Subnet
docker container run -it -d --network=testbridge1 --name testbridge1-container1 -p 3904:80 mywebserver1
docker container run -it -d --network=testbridge1 --name testbridge1-container2 -p 3905:80 mywebserver1

docker network create testbridge2
docker network inspect testbridge2
docker container run -it -d --network=testbridge2 --name testbridge2-container1 -p 3906:80 mywebserver1
docker container run -it -d --network=testbridge2 --name testbridge2-container2 -p 3907:80 mywebserver1
docker container run -it -d --net testbridge2 --cap-add=NAT_ADMIN --name testbridge2-container2 -p 3907:80 mywebserver1
```

### This option "--cap-add=NAT_ADMIN" is use to get the super user privileges on container

```sh
docker container exec testbridge1-container1 ifconfig eth0 | grep Mask
docker container exec testbridge2-container1 ifconfig eth0 | grep Mask
```

### Ping test within custom bridge network

```sh
docker container inspect testbridge1-container2 | grep -i IPaddress
docker container exec testbridge1-container1 ping -c 2 172.18.0.3
docker container exec testbridge1-container1 ping -c 2 testbridge1-container2
```

### Ping test within custom bridge network

```sh
docker container inspect testbridge1-container2 | grep -i IPaddress

"SecondaryIPAddresses": null,
"IPAddress": "",
"IPAddress": "172.18.0.3",

[root@ ~]# docker container exec testbridge1-container1 ping -c 2 172.18.0.3
[root@ ~]# docker container exec testbridge1-container1 ping -c 2 testbridge1-container2
```

##### Inbuild DNS in container

```sh
[root@ ~]# docker container exec testbridge1-container1 cat /etc/resolv.conf
nameserver 127.0.0.11
options ndots:0
[root@ ~]#
```

#### Ping test between two custom bridge network####

```sh
[root@ ~]# docker container inspect testbridge2-container1 | grep -i IPaddress

"SecondaryIPAddresses": null,
"IPAddress": "",
"IPAddress": "172.19.0.2",

[root@ ~]# docker container exec testbridge1-container1 ping -c 2 -w 3 172.19.0.2
[root@ ~]# docker container exec testbridge1-container1 ping -c 2 -w 3 testbridge2-container1
```

### How to make a connectivity between two different networks?

```sh
docker network connect testbridge1 testbridge2-container1
docker container exec testbridge1-container1 ping -c 2 -w 3 testbridge2-container1
docker container inspect testbridge2-container1 | grep -i IPaddress
```

###### How to remove the connectivity between two different networks?

```sh
docker network disconnect testbridge1 testbridge2-container1

[root@ ~]# docker container exec testbridge1-container1 ping -c 2 -w 3 testbridge2-container1
```

###### END

###### LAB for Overlay Network Driver

###### On Master Node

```sh
sudo apt update
sudo apt install firewalld
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

```sh
[root@docker1 ~]# ip a

[root@docker1 ~]# systemctl stop firewalld
[root@docker1 ~]# systemctl disable firewalld

[root@docker1 ~]# docker container ls -a

[root@docker1 ~]# for i in $(docker container ls -aq ) ; do docker container rm -f $i ;done

[root@docker1 ~]# docker network ls

[root@docker1 ~]# docker swarm init --advertise-addr 192.168.129.102
```

### NO need to execute below commands as we have already disabled the firewall services ... This is only for your information

```sh
[root@docker1 ~]# firewall-cmd --add-port=2377/tcp --permanent
success
[root@docker1 ~]# firewall-cmd --add-port=7946/tcp --permanent
success
[root@docker1 ~]# firewall-cmd --add-port=7946/udp --permanent
success
[root@docker1 ~]# firewall-cmd --add-port=4789/udp --permanent
success
[root@docker1 ~]# firewall-cmd --reload
success
#########################################
```

###### Execute the below commands on Salve server

```sh
[root@docker1 ~]# systemctl stop firewalld
[root@docker1 ~]# systemctl disable firewalld

[root@docker2 ~]# docker swarm join --token SWMTKN-1-52hdoybuvxjl86x7col5v9hin7un0bazil5odk5ixxroznmhz6-8qvgeg3v0pi8lsl5nk5zdvabq 192.168.1.19:2377
This node joined a swarm as a worker.

[root@docker2 ~]# docker network ls
```

###### On Master Node

```sh
[root@docker1 ~]# docker node ls

[root@docker1 ~]# docker network ls
NETWORK ID NAME DRIVER SCOPE
6bdf1cc916b8 bridge bridge local
d43beb83a586 docker_gwbridge bridge local
8d66995c344a host host local
lcqle0bghsoh ingress overlay swarm
c4864127ea6b none null local

[root@docker1 ~]# docker network create --opt encrypted -d overlay --attachable my_overlay
iaovhvbnf74gd43nu93a46qpi
[root@docker1 ~]#

[root@docker1 ~]# docker network ls

[root@docker1 ~]# docker run -itd --name overlay-container1 --network my_overlay mywebserver1
```

###### Execute the below commands on Salve server

```sh
[root@docker2 ~]# docker network ls

[root@docker2 ~]# docker run -itd --name overlay-container2 --network my_overlay mywebserver1
```

###### On Master Node

```sh
[root@docker1 ~]# docker container exec -it overlay-container1 ping overlay-container2
```

###### END

###### LAB for IPVLAN Network Driver

```sh
Host 1
for i in $(docker container ls -aq ) ; do docker container rm -f $i ;done

ip a
route -n
docker network create -d ipvlan --subnet=192.168.129.0/24 --gateway=192.168.129.1 --ip-range=192.168.129.40/30 -o ipvlan_mode=l2 -o parent=enp0s3 ipvlan1
docker network inspect ipvlan1

docker run --net=ipvlan1 -itd --name ipvlan1-H1-C1 alpine /bin/sh
docker run --net=ipvlan1 -itd --name ipvlan1-H1-C2 alpine /bin/sh

docker network inspect ipvlan1

ifconfig enp0s3
docker container exec -it ipvlan1-H1-C1 ping 192.168.129.41 => C2 machine
docker container exec -it ipvlan1-H1-C1 ping 192.168.129.19
docker container exec -it ipvlan1-H1-C1 ping 192.168.129.10 ==> My VM IP
docker container exec -it ipvlan1-H1-C1 ping 192.168.129.1
docker container exec -it ipvlan1-H1-C1 ping host2 IP ==> This will also work.

Windows IP = 192.168.129.10
Gateway = 192.168.129.1
VM IP = 192.168.129.19

Host 1

for i in $(docker container ls -aq ) ; do docker container rm -f $i ;done

ip a
docker network create -d ipvlan --subnet=192.168.129.0/24 --gateway=192.168.129.1 --ip-range=192.168.129.50/30 -o ipvlan_mode=l2 -o parent=enp0s3 ipvlan1
docker network inspect ipvlan1

docker run --net=ipvlan1 -itd --name ipvlan1-H2-C1 alpine /bin/sh
docker run --net=ipvlan1 -itd --name ipvlan1-H2-C2 alpine /bin/sh

docker network inspect ipvlan1

ifconfig enp0s3
docker container exec -it ipvlan1-H2-C1 ping 192.168.129.49 => C2 machine IP
docker container exec -it ipvlan1-H2-C1 ping 192.168.129.19
docker container exec -it ipvlan1-H2-C1 ping 192.168.129.10 ==> My VM IP
docker container exec -it ipvlan1-H2-C1 ping 192.168.129.1
docker container exec -it ipvlan1-H2-C1 ping host2 IP ==> This will also work.
docker container exec -it ipvlan1-H1-C1 ping 192.168.129.40
```
