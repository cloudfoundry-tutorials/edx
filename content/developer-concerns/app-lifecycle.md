# Application Lifecycle

Applications are not static resources as they will inevitably undergo changes and evolutions over their lifetime. In this chapter, we take a look at what can happen in the lifecycle of an application.

## Deploying 

An application's life in Cloud Foundry begins upon deployment. Deploying an application is called a 'push'. A `cf push` is highly automated, encapsulating a holistic process of uploading, containerizing, configuring, and scheduling application instances. For this reason, `cf push` has become the gold standard for application deployment. 

You experienced some of the power of `cf push` in the Katacoda tutorial. With one simple command and a manifest file, you were able to containerize your application, run it, and start serving web traffic once the application became available. Quite a few steps go into this process, including reserving and mapping routable hostnames, allocating compute and storage resources for your containers, caching container images, and container scheduling. All of this complexity (and much more) is hidden behind the very simple `cf push` command.

This means developers focus on writing applications that drive business. Operators focus on high-value tasks like providing new services and capabilities and capacity planning. And security and compliance teams rest easy knowing that runtimes are standardized and easy to update.

### Application Configuration

In a future section, we will cover what goes into a containerized application. In addition, a running application needs:

- **Application Configuration**: This could include credentials to access a database or feature flags used in code.  

- **Deployment Configuration**: The deployment config tells Cloud Foundry how to run your application, including the number of instances of the app, and the amount of memory and disk to allocate per container instance. The app name used to refer to the app in Cloud Foundry is part of the deployment config. App routes, if applicable, are also part of the deployment config. 

In Cloud Foundry, the configuration for an application can be declared in a manifest file and checked into source control. This means deployments are repeatable, and configurations are declarative and easily verifiable. In addition, the configuration is available to all developers on the team in a centralized repository. Of course, storing a manifest in source control assumes there is no sensitive information like credentials inside. In Cloud Foundry, this is achieved by parameterizing your secrets and injecting them from a secret store like [CredHub](https://github.com/cloudfoundry-incubator/credhub) or [Vault](https://www.vaultproject.io/) during the push. You can also store secrets in environment variables. 

### Updating

The `cf push` command is also used to update an application when code changes. The `--strategy rolling` flag can be used with `cf push` (and the commands below) to ensure no downtime during the update. When using the rolling strategy, Cloud Foundry will first create a new instance based on the latest payload provided to the push command and place it in service before removing an old instance. Cloud Foundry will roll through all application instances one at a time using this approach. 

The rolling strategy assumes changes to your application are non-breaking. This is a common approach and goal in cloud-native application development. During the update, instances of the old application will run alongside instances of the new version of your application. In the case where breaking changes must occur, Cloud Foundry provides capabilities like route manipulation that can be used to minimize or eliminate downtime during these updates. Note this capability is highly dependent on the application being updated and the breaking changes required. Therefore it is outside the scope of this course.

The rolling update feature is built into the platform. The complexity of ensuring traffic is only routed to live instances is entirely handled by Cloud Foundry. The net result is applications are updated without downtime. Developers simply need to plan for non-breaking changes to leverage this feature.

#### Runtime Dependency Updates
 
If the app code remains the same, but the container or runtime dependencies (from the buildpack) need to be updated, a developer can simply run a `cf restage`. This command uses the same app code but creates a new container image. Again, the rolling strategy can be used to ensure there is no downtime during this process. 

These updates often occur when operators update the buildpacks used to containerize applications or the base image from which all containers are created. When this happens, operators can send a notification explaining the nature of the updates and instructing teams to complete a restage. 

Security and compliance teams can easily audit this process and ensure all applications are updated. Automating the notification and audit process is a common capability implemented by platform providers. This provides confidence that applications are leveraging the latest dependencies and most secure container images possible.

#### Restarting Application Instances

Restarting an application instance in Cloud Foundry involves destroying old container instances and scheduling (starting) new ones. This could be necessary if an environment variable changes or perhaps to deal with a memory leak. If an app container needs to be rescheduled, a developer can perform a `cf restart`, using the rolling strategy to ensure no downtime. Cloud Foundry will use the existing container image to create new running instances, while destroying the old ones.

### Rollback

Should the need arise, Cloud Foundry provides robust rollback support. If an error is detected during a deployment (`cf push`), an immediate halt and rollback process can be triggered using `cf cancel-deployment`. Cloud Foundry will immediately stop deploying new instances, destroy all new ones already created, and replace the old ones. It does this as quickly as possible in parallel, and therefore downtime can occur when canceling a deployment. This command intends to be used when a potentially dangerous or compromising error is discovered during deployment, and thus getting back to a good state is the highest priority. 

If an error is discovered after a deployment completes, developers can roll back to a previous containerized application version or re-containerize using a previous application version. We will not cover these features in detail in this course but do so in the Cloud Foundry for Developers course.

The rollback system in Cloud Foundry is both comprehensive and flexible. Mistakes happen. Bugs are missed. Cloud Foundry gives developers the tools to quickly address these issues and minimize the window of risk exposure. 

### Finite Control

A lot happens during a `cf push` (that is the beauty of it!). But there are use cases where advanced developers may want to automate individual elements of this process. Cloud Foundry enables this as well via [sub-step commands](https://docs.cloudfoundry.org/devguide/push-sub-commands.html). Again we leave the detailed discussion and teaching of these capabilities to the Cloud Foundry for Developers course as these are more advanced features. But rest assured, Cloud Foundry can serve the more advanced or fringe cases of application deployment with similar elegance to the `cf push` command.

## Scaling

In Cloud Foundry, it is easy to scale applications up and down as load variations dictate. This can be achieved via a manifest or using the `cf scale` command.

It is common practice to figure out the required resource allocations of memory and disk for an application container during development. This is called vertical scaling. The allocation of memory and disk applies to each container running an application instance, not directly to the application runtime. 

It is common practice to scale up or down as load dictates by changing the number of deployed instances in production. This is called horizontal scaling and is the process you used in the Katacoda exercise and implements the [eighth factor](https://12factor.net/concurrency) of 12 Factor App best practices related to concurrency: "Scale out via the process model". In addition, the [app autoscaler](https://github.com/cloudfoundry/app-autoscaler) service can be used to automatically scale the number of application instances within a range, based on configured metrics.

## Health Checks, Resiliency, and Routing

In the Katacoda tutorial, you experienced the power of health checks when you killed an app instance. A health check is a monitoring process that continually checks the status of each application instance running in Cloud Foundry.  

If you deploy an app and ask Cloud Foundry to ensure five instances are running, the platform will maintain the desired state of five instances. If an instance crashes, a new one will be created in its place. The route tables used to route HTTP traffic are automatically updated as the state of application instances changes. Automated route table updates based on application health minimize the risk of an app consumer being routed to a defunct instance.

Additionally, health checks are used to determine when an app is ready to receive HTTP traffic. For example, when you scale the number of instances up, Cloud Foundry waits for health checks on the new instances to pass before updating route tables.

Health checks are performed for every application instance automatically. Developers can leverage the default health check types or implement their own health check endpoints if needed. More information on health checks is available in the [documentation](https://docs.cloudfoundry.org/devguide/deploy-apps/healthchecks.html).


## Impacts

Developers have the tools they need to efficiently manage applications without sacrificing capability. For this reason, the `cf push` experience is a career-altering experience for most developers. Cloud Foundry makes it easy to scale, roll back, repair, repave, and evolve applications.

The extremely high levels of automation dramatically reduce errors and mistakes that lead to downtime. Cloud Foundry empowers software teams to operate quickly, efficiently, and securely.
