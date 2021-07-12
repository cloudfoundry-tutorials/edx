---
title: "Cloud Foundry and Kubernetes"
weight: "2"
---

Cloud Foundry and Kubernetes are both open-source projects with many similarities. However, there are also significant differences.

- **Cloud Foundry** (CF) is a Platform-as-a-Service that makes it easy to deploy and operate applications. The [Cloud Foundry Foundation](https://cloudfoundry.org) oversees the project.
- **Kubernetes** (k8s) is an infrastructure platform for automating deployment, scaling, and management of containerized workloads. The [Cloud Native Computing Foundation](https://cncf.io) oversees the project.

The difference is in the abstraction. Constructs and features in Cloud Foundry are centered around applications, while Kubernetes constructs and features are centered around generalized containerize workloads. Both are quite valuable but differ significantly in complexity. By having an opinion, Cloud Foundry can significantly reduce the complexity of deploying and managing applications.

## Focus

Cloud Foundry focuses on bringing a world-class, cloud-native developer experience to its users. Deploying applications (and keeping them running) on Cloud Foundry is simple as the out-of-the-box functionality makes this nearly effortless. However, Cloud Foundry is not designed to run _any_ workload. For example, one will not run databases _on_ Cloud Foundry; instead, apps running on Cloud Foundry connect to databases running elsewhere. Similarly, most commercial off-the-shelf products (COTS), like Microsoft Exchange, are not ideal candidates for Cloud Foundry (though many COTS vendors have ported their platforms to run on Cloud Foundry).

Kubernetes (k8s) is an infrastructure platform for automating deployment, scaling, and managing _containerized workloads_. Kubernetes is a general-purpose, containerized compute platform. It is a modern infrastructure abstraction designed to be as generic as possible, able to run any workload defined in an [OCI (Open Container Initiative)](https://www.opencontainers.org/) container image. As a result, Kubernetes can run the same applications as Cloud Foundry, with significant extra work and configuration, while also suitable for running other containerized workloads like databases or COTS solutions.


> _"First, equating the Cloud Foundry experience to a Kubernetes experience is like equating apples and walnuts. They're not at all related. Kubernetes is all about being an infrastructure abstraction, but that's not optimized for developers, and it has a more broadly applicable set of use cases -- you can take a legacy app, slap it into a container and run it on Kubernetes; you could craft your own containers that are for a more modern architecture and run that on Kubernetes; and a whole bunch of things in between. The Cloud Foundry experience is focused on optimizing for the developer that's writing custom software for business or, in many cases, government applications. It's all about custom code."_ - Chip Childers, Executive Director of the Cloud Foundry Foundation

In short, Cloud Foundry is an opinionated application-focused platform, while [Kubernetes is a platform for building platforms](https://twitter.com/kelseyhightower/status/935252923721793536?s=20). 


## So Cloud Foundry or Kubernetes?

Because Cloud Foundry focuses on the developer experience, it is often the best platform for custom software organizations. However, Cloud Foundry stands to gain from the efficiencies and capabilities of Kubernetes. Furthermore, because Cloud Foundry is not suitable for every workload, organizations need _something else_. For most organizations, this is, or will soon be, Kubernetes. Therefore, it is not a question of "Cloud Foundry or Kubernetes" but rather "Cloud Foundry _and_ Kubernetes." 

The CF community is aligned in the mission to bring the world-class developer experience of Cloud Foundry to Kubernetes. Traditionally, Cloud Foundry has been deployed on virtual machines (VMs) using a tool called [BOSH](https://bosh.io). However, Cloud Foundry can now be easily deployed on Kubernetes, gaining efficiencies in the underlying infrastructure and capitalizing on existing Kubernetes skills.

In this model, most developers can work at the Cloud Foundry layer, gaining efficiency, security, and speed. For the use cases currently outside of Cloud Foundry's purview, smaller groups of developers can fall back to the lower level primitives in Kubernetes as workloads dictate. Over time, more of the Kubernetes primitives will continue to be integrated and encapsulated in the developer-centric approach of Cloud Foundry, allowing ever more workloads to run via Cloud Foundry on Kubernetes.

There are two converging deployment models for Cloud Foundry on Kubernetes:

- [cf-for-k8s](https://github.com/cloudfoundry/cf-for-k8s): The cf-for-k8s project re-implements Cloud Foundry APIs and functionality using Kubernetes native components. 
- [kubecf](https://github.com/cloudfoundry-incubator/kubecf): KubeCF containerizes the existing Cloud Foundry components for deployment on Kubernetes via a Helm chart. 

Deployment on Kubernetes is not the only option. Many of the largest Cloud Foundry installations are deployed on virtual machines using [BOSH](https://bosh.io). The core deployment artifact is called [cf-deployment](https://github.com/cloudfoundry/cf-deployment).
