---
layout: post
title:  "Well Architected Kubernetes"
date:   2021-05-24 10:50:01 +0530
categories: Kubernetes Architecture
tags: ["Kubernetes", "Architecture"]
author: coolsvap
permalink: /well-architected-kubernetes-home
---
# Well Architected Kubernetes

No doubt, [Kubernetes][0]  is one of the best container orchestration platform present today. With excellent features that support scaling, zero-downtime deployments, service discovery, automatic rollout and rollback capabilities, it is perhaps the defacto go to platform. 

There is often a big knowledge transition needed for using Open-source Kubernetes for development and its production implementation. With number of different flavors of Kubernetes implementation it becomes a challenge. With this blog series I am planning to go through one step at a time for a [Well Architected Kubernetes](/well-architected-kubernetes-home). This is an attempt to apply my understanding of [AWS Well Architected framework][1] to the Kubenetes. This is the primary blog post with links to all smaller topis and will grow with time.

I will try to organize the posts in this series in the pillars as defined within [AWS Well Architected framework][1] and will add additional groups as needed.

<details> 
<summary> Operational excellence</summary>
<p> Operational Excellence include ability to ease overall operations for any workload. It can include but not limited to depoyment automation, upgrade/rollbacks, observability, insights etc. Following are some of the recommendations for operational excellence with Kubernetes. 

Two factors are primarily important in any containerization initiative.

1. How the container images are managed?
2. How the operations on the container images are managed?

The operational excellence with Kubernetes can be categorized in three major categories
1. Excellence with operating Kubernetes Infrastructure
2. Excellence with operating the Kubernets platform
3. Excellence with Operating the applications deployed on Kubernetes
</p>

</details>
<details> 
<summary> Security</summary>
</details>
<details> 
<summary> Reliability</summary>
</details>
<details> 
<summary> Performance Efficiency</summary>
</details>
<details> 
<summary> Cost Optimization</summary>
</details>


Any source code, examples, tips will be available at [GitHub][2]. Feel free to suggest if you have any suggestions.

[0]: https://kubernetes.io/
[1]: https://aws.amazon.com/architecture/well-architected/
[2]: https://github.com/cloudnative-tech
