---
layout: post
title:  "Building non root container images"
date:   2020-09-20 17:50:01 +0530
categories: docker tips
author: askashuk
permalink: /docker-001-building-non-root-container-images
---
With ever growing increase in using containerized workloads for deployment, the security of the workload has to come under similar purview and release managers are looking at fine grained security controls for their latest type of build and runnable artifacts, the container images. By default, most container images built earlier had root as the default user and the root user in a containr is same as root use in host machine, there by creating a privileged container or a privileged escalation. If not monitored the application running as root can gain access to the host with the same root user. The same applies to your network access, storage access and hence forth.

Lets have a look at our very popular [nginx][2] image on [Docker Hub][1].

```
# docker run -it nginx bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
d121f8d1c412: Pull complete
ebd81fc8c071: Pull complete
655316c160af: Pull complete
d15953c0e0f8: Pull complete
2ee525c5c3cc: Pull complete
Digest: sha256:c628b67d21744fce822d22fdcc0389f6bd763daac23a6b77147d0712ea7102d0
Status: Downloaded newer image for nginx:latest

# id
uid=0(root) gid=0(root) groups=0(root)
```
Let's check the available users in the current nginx image
```
# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
nginx:x:101:101:nginx user,,,:/nonexistent:/bin/false

# cat /etc/passwd | grep nginx
nginx:x:101:101:nginx user,,,:/nonexistent:/bin/false

# apt-get update
Get:1 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:2 http://deb.debian.org/debian buster InRelease [122 kB]
Get:3 http://deb.debian.org/debian buster-updates InRelease [51.9 kB]
Get:4 http://security.debian.org/debian-security buster/updates/main amd64 Packages [231 kB]
Get:5 http://deb.debian.org/debian buster/main amd64 Packages [7906 kB]
Get:6 http://deb.debian.org/debian buster-updates/main amd64 Packages [7868 B]
Fetched 8385 kB in 7s (1199 kB/s)
Reading package lists... Done
```

Let's come out of the container console and check the nginx image for configured user
```
# docker inspect nginx:latest | grep -i User
            "User": "",
            "User": "",
```


It is recommended practice, running a container is to launch the process with a non-root user. Dockerfile has a very simple instruction USER which can help you do that. Though as simple as it looks, it only starts with using this instruction but a lot of things need to go in design of the application,  dependency management, access levels, etc. This is where you application starts getting container-native.

Let's try creating our nginx image non-root. Here's the Dockerfile,

```
# cat Dockerfile
FROM nginx
USER nginx
```
Simple Dockerfile, lets build it
```
# docker build . -t nginx-non-root
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM nginx
 ---> 7e4d58f0e5f3
Step 2/2 : USER nginx
 ---> Running in 9956c06b7be4
Removing intermediate container 9956c06b7be4
 ---> 80f4160c9c60
Successfully built 80f4160c9c60
Successfully tagged nginx-non-root:latest
```
Checking the user in new Docker image

```
# docker inspect nginx-non-root:latest | grep -i User
            "User": "nginx",
                "USER nginx"
            "User": "nginx",
```
Running the new image and checking and its effect
```
# docker run -it nginx-non-root:latest bash
$ id
uid=101(nginx) gid=101(nginx) groups=101(nginx)

$ apt-get update
Reading package lists... Done
E: List directory /var/lib/apt/lists/partial is missing. - Acquire (13: Permission denied)
```

`nginx` usr doesn't have privileges as `root`, simple isn't it? Try it for your Container images and let us know your feedback.

[1]: https://hub.docker.com/
[2]: https://hub.docker.com/_/nginx