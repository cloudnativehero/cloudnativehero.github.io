---
layout: post
title:  "CKA Tips - 002 Using kubectl explain"
date:   2020-09-14 17:50:01 +0530
categories: Kubernetes CKA Tips
author: coolsvap
permalink: /cka-tip-002-kubectl-explain
---

What would you do when you don't know what is the structure of any [Kubernetes][2] resource, what is the specification, types of attributes, etc, most of us probably reach for Google. But there’s a better way! `kubectl explain` is used to discover static information about Kubernetes Resources. [kubectl][1] `explain` command as it seems a good starting point to understand and know the static information for. In this post lets see how `kubectl explain` works

Here it is

`kubectl explain pod`

Behind the scenes, kubectl just made an API request to my Kubernetes cluster, grabbed the current Swagger documentation of the API version running in the cluster, and output the documentation and object types.

what if I want to see all the fields in a Pod?

`kubectl explain pod --recursive`

and what does the Deploymentstrategy do again?

`kubectl explain pod.spec.containers`


So whenever in exam like [CKA][3]/[CKAD][4] you are not sure about what spec a particular resource has or what fields it has you can use `kubectl explain`

[1]: https://kubernetes.io/docs/reference/kubectl/overview/
[2]: https://kubernetes.io/
[3]: https://www.cncf.io/certification/cka/
[4]: https://www.cncf.io/certification/ckad/
