---
title: "Application Lifecycle"
weight: "7"
---

Applications are not static resources as they will inevitably undergo changes and evolutions over their lifetime. In this chapter, we take a look at what can happen in the lifecycle of an application.

## Deploying 

An application's life in Cloud Foundry begins upon deployment. Deploying an application is called a 'push'. A `cf push` is highly automated, encapsulating a holistic process of uploading, containerizing, configuring, and scheduling application instances. For this reason, `cf push` has become the gold standard for application deployment. 

### Application Configuration

In the 'Containerization' section, we covered what goes into a containerized application. In addition, a running application needs:

- **Application Configuration**: This could include credentials to access a database or feature flags used in code.  

- **Deployment Configuration**: The deployment config tells Cloud Foundry how to run your application, including the number of instances of the app, and the amount of memory and disk to allocate per container instance. The app name used to refer to the app in Cloud Foundry is part of the deployment config. App routes, if applicable, are also part of the deployment config. 

Configuration for an application can be declared in a manifest file and checked into source control (provided secrets are in a secret store or the Cloud Foundry created environment!). 

### Updating

The `cf push` command is also used to update an application when code changes. The `--strategy rolling` flag can be used with `cf push` (and the commands below) to ensure no downtime during the update.
 
If the app code remains the same, but the base image (stack) or runtime dependencies (from the buildpack) need to be updated, a developer can simply run a `cf restage`. This command uses the same app code but creates a new container image.

If an app container needs to be rescheduled, a developer can perform a `cf restart` (or `cf restart-app-instance`). This command results in new running container instance(s) replacing the old.

### Rollback

Should the need arise, Cloud Foundry provides robust rollback support. If an error occurs during a deployment, an immediate halt and rollback process can be triggered using `cf cancel-deployment`. If an error is discovered after a deployment completes, developers can rollback to a previous containerized application version or re-containerize using a previous application version. Thus, the rollback system is both robust and flexible.

### Finite control

A lot happens during a `cf push` (that is the beauty of it!). But there are use cases where a developer may need to automate individual elements of this process. Cloud Foundry enables this as well via [sub-step commands](https://docs.cloudfoundry.org/devguide/push-sub-commands.html).


## Scaling

In Cloud Foundry, it is easy to scale applications up and down as load variations dictate. Resource allocations (vertical scaling) and the number of instances (horizontal scaling) are easily modified. The autoscaler service can be used to automatically scale the number of application instances within a range, based on configured metrics.

## Health checks, resiliency, and routing

If you completed the Katacoda scenario, you experienced the power of heatlh checks when you killed an app instance. A health check is a monitoring process that continually checks the status of a running Cloud Foundry app. 

If you deploy an app and ask Cloud Foundry to ensure five instances are running, the platform will maintain the desired state of five instances. If an instance crashes, a new one will be created in its place. The route tables, used to route http traffic, are automatically updated as the state of application instances changes. Automated route table updates based on application health minimize the risk of a consumer being routed to a defunct instance.

Additionally, health checks are used to determine when an app is ready to receive http traffic. As an example, when you scale the number of instances up, Cloud Foundry waits for health checks on the new instances to pass before updating route tables.

Health checks are performed for every application instance automatically. Developers can leverage the default health check types or implement their own health check endpoints.


## Impacts

Developers have the tools they need to efficiently manage applications without sacrificing capability. For this reason, the `cf push` experience is a career-altering event for most developers. Cloud Foundry makes it easy to scale, rollback, repair, repave, and evolve applications.

The extremely high levels of automation dramatically reduce errors and mistakes that lead to downtime. Cloud Foundry empowers software teams to operate quickly, efficiently, and securely.
