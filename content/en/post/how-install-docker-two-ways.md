---
date: 2017-04-09T10:58:08-04:00
description: "How to install docker. Two ways. Installing package or by hand."
featured_image: "/images/Pope-Edouard-de-Beaumont-1844.jpg"
tags: ["docker","DevOps","Linux"]
title: "How to install docker. Two ways."
---

Long time ago we had simple instalation procedure for docker. It can be installed by root or by unpriveleged user. There was some differences.

For now, installing Docker is even simplier than couple years ago. There are 2 main ways:

- 1. Setup a repository for easy update in future (recomended)
- 2. By hand for difficult update in future

## Recomended way

Update index of package database:

```
sudo apt update

```

Install required packages:


```
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

```

Add Docker’s official GPG key:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Set up the **stable** repository:

```
echo \
  deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install Docker engine and dependencies:

```
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## If Docker can\'t be installed from repository.

There is a way how install it from package. Download .deb package and install it. You must go to [`https://download.docker.com/linux/ubuntu/dists/`](https://download.docker.com/linux/ubuntu/dists/), choose your Ubuntu version, then browse to `pool/stable/`, choose `amd64`, `armhf`, `arm64`, or `s390x`, and download the `.deb` file for the Docker Engine version you want to install.

```
sudo dpkg -i /path/to/package.deb
```

## Verify installation by running simple container.

This container can be easily installed and run immediately:

```
sudo docker run hello-world
```

## Who can start Docker

In a described way there will be only one user who can configure, start and stop Docker containers. The Root. If you in need to run Docker containers by unpriveleged user you must apply post-installation steps for linux you can find [here](https://docs.docker.com/engine/install/linux-postinstall/).

```
Good luck and be Docker with containers with you as long as you wish!
```