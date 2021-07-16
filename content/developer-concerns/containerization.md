---
title: "Containerization"
weight: "3"
---

Cloud Foundry runs workloads in containers. The platform supports bringing your containers or, more commonly, simply bringing your applications and allowing the platform to containerize them. In this chapter, we look at these two approaches. 

## What goes into an app container image?

To compare the two approaches, we look at what goes into an app container image:

- **Base image**: The base image is a starting point for the complete image. For example, if you have a Java application, you may start with the OpenJDK base image. Even if you are building a custom container, you likely start with a base operating system like `ubuntu:bionic` as the base image contains system libraries. In Docker, a base image is specified via the `FROM` directive. In Cloud Foundry, the base image is called a `stack` and is configured by operators. A base image in Docker can contain a large number of dependencies. A stack in Cloud Foundry is purposefully kept small.

- **Runtime dependencies**: The container image needs a runtime to run your app code. The runtime could be an interpreter for a ruby app, the JRE (or JRE + Tomcat) for a Java app, or something like NGINX to serve static content. In Docker, these elements could be included in the base image or declared in the Dockerfile directives. In Cloud Foundry, runtime dependencies are provided by one or more [buildpacks](https://buildpacks.io/) during the staging process. Buildpacks gather everything your app needs to run, and it often does this job quickly and quietly.

- **App Code**: As a developer, you are responsible for providing your application code to the platform. 

## Impact

Cloud Foundry removes the responsibility for containerizing applications from developers. Developers can therefore focus on writing code. They do not need to monitor base images and runtime dependencies for updates. When a buildpack or stack (base image) is updated, they can re-containerize their application without downtime using a single command (`cf restage`).

From an operator, security, and compliance perspective, the contrast in approaches is stark. Consider the following questions:
- How many versions of Java (or Ruby, Python, etc.) are you running in production, and what are they?
- What kernel and versions are running in production?
- What known vulnerabilities currently exist in running applications?
- How long will it take you to ensure all apps are updated to the latest base image?

In an environment where developers bring their containers, these are difficult questions to answer (without a significant amount of additional software to track and manage containers). Can you implement all that Cloud Foundry does? Of course, but it would be a lot of work or require additional products to do so.

In Cloud Foundry, these questions are not difficult to answer as the stack and buildpacks are platform features. In addition, stacks and buildpacks are both versioned. Therefore, it is easy to query the platform for the usage of specific versions. The maintainers publish CVEs and release regular updates. 

In Cloud Foundry, operators deploy updates to the entire platform. Runtime dependencies are updated by uploading a new buildpack and alerting developers to execute a zero downtime `cf restage` (this is easily automated if desired). Similarly, the base image for all applications can be easily updated by uploading a new stack and again directing developers to restage. 

To highlight the power of this approach, let's look at an example: [Heartbleed](https://heartbleed.com/). The Heartbleed bug is a serious vulnerability in the popular OpenSSL cryptographic software library. The stack and most buildpacks were affected by this. When discovered, the maintainers released updates to the buildpack and stacks (as well as platform components). Cloud Foundry operators deployed these updates and alerted developers to restage applications. Restaging each app required a single `cf restage`, which completes in a few minutes. That was it. 
