# Cloud Foundry and Kubernetes

Cloud Foundry and Kubernetes are both open-source projects with many similarities. However, there are also significant differences.

- **Cloud Foundry** (CF) is an opinionated Platform-as-a-Service that makes it easy to deploy and operate applications. The [Cloud Foundry Foundation](https://cloudfoundry.org) oversees the project.
- **Kubernetes** (k8s) is a generalized infrastructure platform for automating deployment, scaling, and managing containerized workloads. The [Cloud Native Computing Foundation](https://cncf.io) oversees the project.

![CF with K8s logo](/images/eirini-logo.png) The difference is in the focus and approach. Constructs and features in Cloud Foundry are centered around applications and developer workflows, while Kubernetes constructs and features are centered around generalized containerized workloads. Both are quite valuable but differ significantly in complexity. By having an opinion, Cloud Foundry can substantially reduce the complexity of deploying and managing applications.

In short, Cloud Foundry is an opinionated application-focused platform, while [Kubernetes is a platform for building platforms](https://twitter.com/kelseyhightower/status/935252923721793536?s=20). 

## Focus and Approach

Cloud Foundry focuses on bringing a world-class, cloud-native developer experience to its users. Deploying applications (and keeping them running) on Cloud Foundry is simple; the out-of-the-box functionality makes this nearly effortless. However, Cloud Foundry is not designed to run _any_ workload. For example, one will not run databases _on_ Cloud Foundry; instead, apps running on Cloud Foundry connect to databases running elsewhere. Similarly, most commercial off-the-shelf products (COTS), like Microsoft Exchange, are not ideal candidates for Cloud Foundry (though many COTS vendors have ported their platforms to run on Cloud Foundry).

Kubernetes (k8s) is an infrastructure platform for automating deployment, scaling, and managing _containerized workloads_. Kubernetes is a general-purpose, containerized compute platform. It is a modern infrastructure abstraction designed to be as generic as possible, able to run any workload defined in an [OCI (Open Container Initiative)](https://www.opencontainers.org/) container image. As a result, Kubernetes can run the same applications as Cloud Foundry, with significant extra work and configuration, while also potentially running other containerized workloads like databases or COTS solutions.

### Cognitive Load

Cloud Foundry and Kubernetes are based on the same (or similar) infrastructure primitives. However, Cloud Foundry has chosen to implement automation that encapsulates these primitives in higher-level abstractions. For example, updating the runtime components in the container image for an application without downtime is trivial in Cloud Foundry. This is not the case in Kubernetes. 
The very nature of being a generalist platform means K8s must keep these primitives exposed as first-class citizens. This has some profound implications, the largest of which is cognitive load. 

It is no secret that technology is challenging. There are many considerations for developers have to account for. By establishing common patterns based on best practices and encapsulating complex but low-level tasks in simple commands, Cloud Foundry significantly reduces the cognitive load on users. 

As a comparison, let's consider three representative scenarios: onboarding and learning, day-to-day actions, and compliance.

- Consistency of automation is easier to rationalize, onboard, and troubleshoot. Cloud Foundry exposes simple concepts through standardized patterns. Onboarding new developers with Cloud Foundry takes a few days. Getting to the same level of proficiency in Kubernetes takes weeks or months. With Cloud Foundry, there are fewer primitives and fewer patterns to learn.

- With higher-level capabilities, deploying, updating, and managing applications is fast, repeatable, and safe. Cloud Foundry makes it easy to operate safely with speed by having an opinion and implementing best practices. Consistency reduces the likelihood of errors. If an error occurs,  patterns make it easier to track down issues. With Cloud Foundry, there is less to configure, less to implement, and less to maintain.

- By having an opinion, CF can automate the repetition. Standardization significantly impacts compliance and security without impeding progress. Potential attack vectors are reduced considerably, and closing vulnerabilities is far easier.

In summary, when you expose lower-level platforms and primitives to all developers, you pay a significant organizational penalty in development, operations, security, and compliance. So the core question becomes, "do you want developers focused on capabilities or building platforms?"


## So Cloud Foundry or Kubernetes?

Because Cloud Foundry focuses on the developer experience, it is often the best platform for custom software organizations. However, Cloud Foundry stands to gain from the efficiencies and capabilities of Kubernetes. Furthermore, because Cloud Foundry is not suitable for every workload, organizations need _something else_. For most organizations, this is, or will soon be, Kubernetes. Therefore, it is not a question of "Cloud Foundry or Kubernetes" but rather "Cloud Foundry _and_ Kubernetes." 

The CF community is aligned in the mission to bring the world-class developer experience of Cloud Foundry to Kubernetes. Traditionally, Cloud Foundry has been deployed on virtual machines (VMs) using a tool called [BOSH](https://bosh.io).  Many of the largest Cloud Foundry installations are deployed on virtual machines using BOSH. However, Cloud Foundry can now be easily deployed on Kubernetes, gaining efficiencies in the underlying infrastructure and capitalizing on existing Kubernetes skills.

The following [diagram from EngineerBetter](https://github.com/EngineerBetter/k8s-is-not-a-paas) summarises the out of the box functionality provided by a traditional IaaS, Cloud Foundry deployed to virtual machines with BOSH, Kubernetes, hosted Kubernetes services like Google’s GKE, and Cloud Foundry deployed on top of Kubernetes:

![Cloud Foundry and Kubernetes Concerns Image](/images/iaas-kubes-paas.png)

The diagram shows that:

- Cloud Foundry can compliment Kubernetes by providing a single distribution of additional functionality that is pre-configured to work together
- Kubernetes can be extended to offer the same functionality as Cloud Foundry
- Cloud Foundry can still be deployed to virtual machines with BOSH. This is still the most common deployment methodology today, though the focus is on integrating Cloud Foundry and Kubernetes more robustly.

Most developers can work at the Cloud Foundry layer in this model, gaining efficiency, security, and speed. However, developers can fall back to the lower level primitives in Kubernetes as workloads dictate for the use cases outside Cloud Foundry's purview. Over time, more of the Kubernetes primitives will continue to be integrated and encapsulated in the developer-centric approach of Cloud Foundry, allowing ever more workloads to run via Cloud Foundry on Kubernetes.
