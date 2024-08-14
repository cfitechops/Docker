# Agenda

**What resources are required to run an application.**

. Traditional way of hosting an applications.

. Why we need virtualization?

. Why we need Container?

. Difference between VM and Container ?

. What is container ?

. What is Docker ?

# ---IMAGE---

## Step 1: Remove already installed container engine.

```sh
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine podman buildah -y
```

Execute below commands in order to install the Docker packages.

```sh
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine podman buildah -y
```

## Step 2: yum install -y yum-utils

**Why we need to install the yum-utils ?**

**yum-utils:** is a collection of tools and programs for managing yum repositories, installing debug packages, source packages, extended information from repositories and administration.

## Step 3: Need to add the repo file for Docker packages.

```sh
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install containerd.io docker-ce docker-ce-cli  -y
cat /etc/yum.repos.d/docker-ce.repo
systemctl enable docker.service
systemctl start docker.service
systemctl status docker.service
docker –version
```
