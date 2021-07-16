---
title: "Types of Workloads"
weight: "2"
---

As we mentioned previously, Cloud Foundry is not a generalized compute platform. It is therefore important to understand the types of workloads Cloud Foundry can support. Cloud Foundry supports both short running and long running processes in containers. A short running process is called a 'task'. A long running process is called an 'application'.

Cloud Foundry is optimized to run stateless applications. State should be stored in an attached service like a database or [volume](https://docs.cloudfoundry.org/devguide/services/using-vol-services.html) (for applications requiring persistent disk).  Cloud Foundry applications can optionally serve web (http/https) traffic. Applications that do not serve web traffic are referred to as "worker" applications.

## Language Support

Cloud Foundry allows developers to bring application code to the platform or to bring existing Docker images.

### Application Code

When you bring your application code to Cloud Foundry, the will generate a container image for an application before executing each instance it in a container. Buildpacks are used to generate container images. We will discuss this process more in the next section. For now, it suffices to understand that Cloud Foundry can support languages for which a buildpack exists. This includes, but is not limited to, the following:

- Compiled Binaries
- Go
- HWC
- Java (including JVM languages)
- .NET Core
- NGINX
- NodeJS
- PHP
- Python
- R
- Ruby
- Staticfile

There are many additional community created buildpacks as well.

### Docker Images

Cloud Foundry also supports running Docker images (provided this feature is enabled). Cloud Foundry will fetch the image from registry, like Dockerhub, before running each instance in a container. For details on Docker support in Cloud Foundry, refer to the [documentation](https://docs.cloudfoundry.org/devguide/deploy-apps/push-docker.html).

## Container Execution

Cloud Foundry runs all workloads in [containers](https://azure.microsoft.com/en-us/overview/what-is-a-container/). Cloud Foundry runs each application instance in its own container instance. Therefore, if you ask Cloud Foundry to run five instances of an application, Cloud Foundry will run 5 container instances. However, Cloud Foundy also supports more complex scenarios.

### Multi-process Applications

Cloud Foundry supports multiple process applications as well. The most common scenario is an application that consists of a front end web interface and back end data service. The front end and back end can be deployed as a single unit and each front end and back end instance will run in its own container. 

### Sidecar Applications

Cloud Foundry also supports running sidecars. A sidecar is an executable that runs _in the same container_ as your application. Logging or auditing agents are a common use case for sidecars. When using this feature, a sidecar is run inside each application instance container.

## Tasks

A task is an app or script whose code is included as part of a deployed app, but runs independently in its own container. Database migration or maintenance scripts are a common use case for tasks.




## Impact

Developers:
- Wide range of languages supported. 
- Docker as a fallback



More about the impact of buildpacks in the next section


Operations:
Cloud Foundry can efficiently run thousands of app instances by achieving significant application density using containers.

Security
Workloads are isolated from each other