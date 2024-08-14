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

**Containerd** is available as a daemon for Linux and Windows. It manages the complete container lifecycle of its host system, from image transfer from one place to another and storage to container execution (fetch the file from the storage if container required) and supervision to low-level storage to network attachments (data will flow from where?) and beyond.
**Containerd** uses the OS kernel feature such as Namespace and Cgroups.
**cgroups** (abbreviated from control groups) is a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes.
**A namespace** wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource.
