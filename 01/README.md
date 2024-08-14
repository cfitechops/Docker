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