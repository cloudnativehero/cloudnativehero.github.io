---
layout: post
title:  "CKA Tips - 001 Enable Kubectl Completion"
date:   2020-09-09 17:50:01 +0530
categories: Kubernetes CKA Tips
author: coolsvap
permalink: /cka-tip-001-kubectl-completion
---

[Kubectl][1] is the most common command line client used for working with [Kubernetes][2]. It provides a range of features. One of the most important feature is to enable the completion so we can easily get options for different CRUD operations on Kubernetes resources. I think this is the first thing you should do when using Kubernetes. During both [CKA][3] and [CKAD][4], time is the essense and we need to use all the help we can get from the tools we have available. Please have a look at the recording below for setting up kubectl completion.

[![asciicast](https://asciinema.org/a/358692.svg)](https://asciinema.org/a/358692)

Once you have kubectl completion enabled you can easily use it. Since asciinema does not really work well with completion, I will figure it out later, following is the output you can expect.

```
root@k8s-master:~# kubectl
alpha          cluster-info   diff           logs           scale
annotate       completion     drain          options        set
api-resources  config         edit           patch          taint
api-versions   convert        exec           plugin         top
apply          cordon         explain        port-forward   uncordon
attach         cp             expose         proxy          version
auth           create         get            replace        wait
autoscale      delete         kustomize      rollout
certificate    describe       label          run
```
[1]: https://kubernetes.io/docs/reference/kubectl/overview/
[2]: https://kubernetes.io/
[3]: https://www.cncf.io/certification/cka/
[4]: https://www.cncf.io/certification/ckad/
