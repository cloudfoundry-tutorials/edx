---
title: "Containerization"
weight: "4"
---

Cloud Foundry runs workloads (applications and tasks) in containers. The platform supports bringing your container images in the form of Docker or, more commonly, simply bringing your applications and allowing the platform to containerize them. In this chapter, we look at these two approaches. 

## What goes into an app container image?

To compare the two approaches, we look at what goes into an app container image:

### Base image 

The base image is a starting point for the complete container image and contains the common system libraries required to run the container. For example, if you have a Java application, you may start with the OpenJDK base image, which contains the base operating system (like Alpine or Debian) and the OpenJDK runtime. Even if you build a custom container, you likely start with a base operating system like `ubuntu:bionic` as the base image contains system libraries. 

In Docker, a base image is specified via the `FROM` directive in a Dockerfile. In Cloud Foundry, the base image is called a `stack` and is configured and managed by platform operators. A base image in Docker can contain many dependencies and libraries, or it may be minimal. A stack in Cloud Foundry is purposefully kept as small as possible. 

Developers do not typically need to be concerned with a base image in Cloud Foundry. The platform will default to the same Linux base image for every container automatically. With Docker, this is not the case, and developers are responsible for monitoring when their base images change and updating application images accordingly. In Cloud Foundry, operators notify developers when they need to perform a simple `cf restage --strategy rolling` for their apps to pick up changes to the base image.

### Runtime dependencies

The container image needs a runtime to run your app code. The runtime could be an interpreter for a Ruby app, the JRE (or JRE + Tomcat) for a Java app, or something like NGINX to serve static content. The runtime dependencies could also include things like a New Relic or App Dynamics agent that adds capabilities for application performance monitoring (APM).

In Docker, these elements could be included in the base image or installed in other Dockerfile directives (typically `COPY` or `RUN` directives). A security team would have to inspect the container image to determine what is inside. In Cloud Foundry, runtime dependencies are provided by one or more [buildpacks](https://buildpacks.io/) during the staging process. Buildpacks gather everything your app needs to run, and it often does this job quickly and quietly.

Again, developers are not responsible for directly managing runtime dependencies in Cloud Foundry. As buildpacks are updated, they can simply use `cf restage` to pick up the latest changes.

### App code

As a developer, you are responsible for providing your application code to the platform. This is the same in Docker and Cloud Foundry. However, it should be noted that Cloud Foundry will store app code on every push. This is what enables developers to simply restage applications or roll back to previous versions.

## Impact

Cloud Foundry removes the responsibility for containerizing applications from developers. Developers can therefore focus on writing code. They do not need to monitor base images and runtime dependencies for updates. When a buildpack or stack (base image) is updated, they can re-containerize their application without downtime using a single command (`cf restage`).

From an operations, security, and compliance perspective, the contrast in approaches is stark. There is tremendous power and protection in standardizing both base images and runtime dependencies. Consider the following questions:
- How many versions of Java (or Ruby, Python, etc.) are you running in production? What are they? Where are they running?
- What operating system kernel and versions are running in production?
- What known vulnerabilities currently exist in running applications?
- How long will it take you to ensure all apps are updated to the latest base image and runtime dependencies?

In an environment where developers bring their containers, these are difficult questions to answer without a significant amount of additional software to track, scan, and manage containers. Can you implement all that Cloud Foundry does? Of course, but it would be a lot of work or require additional products, which also require operations and maintenance, to do so.

In Cloud Foundry, these questions are not difficult to answer as the stack and buildpacks are standardized platform features. In addition, stacks and buildpacks are both versioned. Therefore, it is easy to query the platform for the usage of specific versions. The maintainers publish [CVE (Common Vulnerability and Exposure)](https://en.wikipedia.org/wiki/Common_Vulnerabilities_and_Exposures) reports and release regular updates that are efficiently and reliably rolled out without downtime.

To highlight the power of the Cloud Foundry approach, let's look at an example: [Heartbleed](https://heartbleed.com/). The Heartbleed Bug is a serious vulnerability in the popular OpenSSL cryptographic software library. Most base images and many runtime dependencies were vulnerable to this, including the stack and buildpacks. When discovered, the maintainers released updates to the buildpack and stacks (as well as platform components). Cloud Foundry operators deployed these updates and alerted developers to restage applications. Restaging each app required a single `cf restage`, which was completed in a few minutes. That was it. Now ask yourself what this would have looked like in a buildpack-less world?

## Container Execution

As we have stated, Cloud Foundry runs all workloads in [containers](https://azure.microsoft.com/en-us/overview/what-is-a-container/). In addition, cloud Foundry runs each application instance in its own container instance. Therefore, if you ask Cloud Foundry to run five instances of an application, Cloud Foundry will run 5 container instances. You saw this in the scaling portion of the Katacoda tutorial.

Depending on the type of Cloud Foundry deployment you are using, these containers may be run in Kubernetes or in Cloud Foundry's container execution engine called Diego. Either way, the developer experience is the same.

### Multi-process Applications

Cloud Foundry supports multiple process applications as well. The most common scenario is an application that consists of a front-end web interface and back-end data service. The front end and back end can be deployed as a single unit, and each front end and back end instance will run in its own container. 

### Sidecar Applications

Cloud Foundry also supports running sidecars. A sidecar is an executable that runs _in the same container_ as your application. For example, logging or auditing agents are a common use case for sidecars. When using this feature, a sidecar is run inside each application instance container.

### Tasks

A task is an app or script whose code is included as part of a deployed app, but runs independently in its own container. For example, database migration or maintenance scripts are a common use case for tasks.
