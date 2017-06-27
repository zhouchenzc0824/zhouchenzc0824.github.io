---
layout: post
title: "Docker Configuration"
date: 2017-5-5 12:53:11.000000000 -06:00
tags: Docker
---
>Docker is a powerful and, at the same time, a lightweight tool with extrem flexibility and portability for DevOps Engineer or Software Engineer to make application environment configurationa and testing easy and fast.

I am not gonna talk about the general configuration for different OS since there are so many tutorials avaliable online for learners. Follow it step by step then you can work it out.I just want to elaborate on some weird issues that I came across.

No matter what OS you use, after succsessfully complete **Docker** installation, you can try typing `docker version` that it means it is installed properlly when pop up the message on command prompt,

```
Client:
  Version: ...
  API version: ...
  Go version:
  Git commit: ...
  Built: ...
  OS/Arch: ...
Server:
  Version: ...
  API version: ...
  Git commit: ...
  Go version:
  Built: ...
  OS/Arch: ...
  Experimental: ...
```

Otherwise you need to reinstall **Docker**

Or it may seem like this without Client information

```
Server:
  Version: ...
  API version: ...
  Git commit: ...
  Go version:
  Built: ...
  OS/Arch: ...
  Experimental: ...
```

Then restart the service, `service docker restart`, it should be normal.
Otherwise go reinstalling the whole thing and try it again.

After finish installation part, then you may have to try create your first container based off of your first image.
`docker pull *` replace `*` with any image name (e.g. mysql, unbuntu etc) that can be found in **Docker Store** or **Docker Hub**.
`docker search *`replace `*` with any image name (e.g. mysql, unbuntu etc).

It is recommended that you go to these places using browser with more specifics in **Docker Hub** and **Docker Store**.

For example you have a windows machine and you want to dockerize a linux OS like alpine(a lightweight OS with only 4MB listed as one of the most popular Linux OS), then try
`docker pull apline`
or
`docker run -it alpine`
If alpine is not locally installed, it will do `docker pull alpine:latest` unless you specify the version like `docker pull alpine:3.5` behind the scenes then run a terminal on alpine OS.


The weird thing is that when I try to build and pubulish the python app, it failed with message like this

```
Sending build context to Docker daemon   7.68kB
Step 1/8 : FROM alpine
 ---> 665ffb03bfae
Step 2/8 : RUN apk add --update py2-pip
 ---> Running in 9e963eb5c93c
fetch http://dl-cdn.alpinelinux.org/alpine/v3.6/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.6/community/x86_64/APKINDEX.tar.gz
ERROR: http://dl-cdn.alpinelinux.org/alpine/v3.6/main: temporary error (try again later)
WARNING: Ignoring APKINDEX.84815163.tar.gz: No such file or directory
ERROR: http://dl-cdn.alpinelinux.org/alpine/v3.6/community: temporary error (try again later)
WARNING: Ignoring APKINDEX.24d64ab1.tar.gz: No such file or directory
ERROR: unsatisfiable constraints:
  py2-pip (missing):
    required by: world[py2-pip]
The command '/bin/sh -c apk add --update py2-pip' returned a non-zero code: 1
```

which means it cannot install pip and python on alpine.
After I test the cdn link, it is actully valid.

My troubleshooting process is that let me try the same thing in different containers with diffrent OS and it goes to failure everytime.

I start thinking it could be the internet connection that is block from inside the container as I have connection on my host OS.

Then I try the following things:

I use `apk update` to fetch the cdn address and refresh the list on alpine, failed with same log.
I use `yum update` on CentOs, failed with the same log.

Then issue comfirmed that it has something to do with connection from inside container.

I guess it is something acting strange that the DNS server the docker is using lets containers resolve onto the internet.

So I go for the docker DNS configuration file. First locate the DNS configuration in /etc/default/docker, it shows info 

```
# Docker Upstart and SysVinit configuration file

#
# THIS FILE DOES NOT APPLY TO SYSTEMD
#
#   Please see the documentation for "systemd drop-ins":
#   https://docs.docker.com/engine/admin/systemd/
#

# Customize location of Docker binary (especially for development testing).
# DOCKERD="/usr/local/bin/dockerd"

# Use DOCKER_OPTS to modify the daemon startup options.
  DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"

# If you need Docker to use an HTTP proxy, it can also be specified here.
# export http_proxy="http://127.0.0.1:3128/"

# This is also a handy place to tweak where Docker's temporary files go.
# export DOCKER_TMPDIR="/mnt/bigdrive/docker-tmp"
```


Uncomment the `DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"` then Restart the service `service docker restart`

```
Boom, it works fine now, you are wellcome.
```











