---
layout: post
title:  "Building Non-root images"
date:   2020-09-20 17:50:01 +0530
categories: Docker Tips
author: askashuk
permalink: /docker-001-building-non-root-images
---
With ever growing increase in using containerized workloads for deployment, the security of the workload has to come under similar purview and release managers are looking at fine grained security controls for their latest type of build and runnable artifacts, the container images. By default, most container images built earlier had root as the default user and the root user in a containr is same as root use in host machine, there by creating a privileged container or a privileged escalation. If not monitored the application running as root can gain access to the host with the same root user. The same applies to your network access, storage access and hence forth.

Lets have a look at our very popular nginx image on Docker Hub.



It is recommended practice, running a container is to launch the process with a non-root user. Dockerfile has a very simple instruction USER which can help you do that. Though as simple as it looks, it only starts with using this instruction but a lot of things need to go in design of the application,  dependency management, access levels, etc. This is where you application starts getting container-native.

Let's try creating our nginx image non-root. Here's the Dockerfile,

Dockerfile

Let's see how it runs with non-root user

