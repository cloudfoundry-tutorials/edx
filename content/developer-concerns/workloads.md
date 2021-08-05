---
title: "Workload Basics"
weight: "2"
---

As we mentioned previously, Cloud Foundry is not a generalized computing platform but rather a platform for running custom applications. It is therefore essential to understand the types of workloads Cloud Foundry is designed to support.

## Stateless Applications

Cloud Foundry is optimized to run stateless applications. "Stateless" does not mean your application cannot store data or have any state. It simply means that the state should be stored in an external service like a database or attached [volume](https://docs.cloudfoundry.org/devguide/services/using-vol-services.html) rather than on a local disk. 

This approach may sound familiar if you are aware of [12 Factor Application](https://12factor.net) best practices. 12 Factor App best practices were authored by experienced developers and practitioners to address systemic problems in modern application development. They attempt to define a common language to discuss the issues and offer a broad set of conceptual solutions. When evaluating platforms like Cloud Foundry, it is important to remember 12-factor app principles are recommendations, not requirements. 

The [sixth factor](https://12factor.net/processes) regarding processes states: "Execute the app as one or more stateless processes." There are many inherent benefits to a short-lived, ephemeral file system. Application instances can safely and quickly be added, removed, or replaced. As you saw in the Katacoda tutorial, this allows Cloud Foundry to quickly detect when instances have failed or crashed and replace them. It also ensures scaling up or down is simple and fast. Cloud Foundry can simply add or remove application instances at your direction or using the [autoscaler](https://github.com/cloudfoundry/app-autoscaler) service. This form of scaling is related to the [eighth factor](https://12factor.net/concurrency) regarding concurrency: "Scale out via the process model".

## Language Support

Cloud Foundry allows developers to simply bring application code to the platform or to bring existing Docker images.

### Application Code

When you bring your application code to Cloud Foundry, the platform will generate a container image for the application before executing instances in containers. Buildpacks are used to generate container images. We will discuss this process more in the next section. For now, it suffices to understand that Cloud Foundry can support languages for which a buildpack exists. This includes, but is not limited to, the following:

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

There are additional community-created buildpacks as well. For example, the Katacoda tutorial utilized the binary-buildpack to run the compiled application written in go-lang.

### Docker Images

Cloud Foundry also supports running Docker images (provided operators enable this feature in your instance). In this case, Cloud Foundry fetches the Docker image from a registry, like Dockerhub, before running instances in containers. The `cf push` command has flags for specifying the details of your registry and image. For more information on deploying Docker applications in Cloud Foundry, refer to the [documentation](https://docs.cloudfoundry.org/devguide/deploy-apps/push-docker.html).


## Inbound Traffic

Cloud Foundry applications can serve inbound web traffic over HTTP or HTTPS if desired. Applications that do not serve web traffic are referred to as "worker" applications. Note that applications are not limited to HTTP or HTTPS when connecting out. Provided operators have allowed it, applications can connect out on any number of additional protocols.


## Impact

Cloud Foundry supports a wide range of languages without putting the responsibility of managing container images on developers. By using buildpacks, developers can focus on application code. If necessary, developers can fall back to docker images for applications. We compare and contrast the use of buildpacks and docker images in more detail in a future section.

Cloud Foundry supports a wide range of workloads, provided they are designed to house state in an external service. In enforcing this, Cloud Foundry can provide significant development and operational capabilities. For example, by insisting that applications are stateless, Cloud Foundry can provide unparalleled performance in application scaling and recovery. In most cases, this approach dramatically simplifies the design of applications with respect to future scalability.

We will look at the impact of how Cloud Foundry handles applications as we progress through this course.
