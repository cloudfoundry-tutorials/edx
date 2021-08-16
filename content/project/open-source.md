# The Open Source Model

Cloud Foundry is a large and complex ecosystem of components and add-on capabilities. This section reviews the model to ensure compatibility across distributions, the variations you are likely to see in the commercial market, and links the source code.

## Open-core for Compatibility

In large, open-source projects, it is challenging to prevent divergence, particularly when very large commercial vendors are involved. Each wants to ensure the features they need are prominent and prioritized. Compatibility between distributions becomes a challenge for the end-user community. To prevent these types of issues, Cloud Foundry follows an open-core model whereby core components must be supported in order to use the name "Cloud Foundry". The open-core model helps to ensure a basic level of interoperability between all Cloud Foundry distributions.

The two most crucial open core elements in Cloud Foundry are the [command line interface (CLI)](https://docs.cloudfoundry.org/cf-cli/) and the [Cloud Foundry API (CAPI)](https://v3-apidocs.cloudfoundry.org/version/3.102.0/index.html). 

The CLI is the tool used to interact with any Cloud Foundry instance. Therefore, the CLI must be supported unchanged. However, providers may decide to disable certain features. The disabling of features is perfectly acceptable, provided the existing commands remain unmodified.

The Cloud Foundry API is the standardized interface into a Cloud Foundry instance. The CLI is making calls to the API when you use it to execute commands. The API defines the capabilities of the Cloud Foundry platform. Providers are free to reimplement this API by any means they see fit. The cf-for-k8s project is doing this now, reimplementing Cloud Foundry features using Kubernetes native constructs whenever possible. Just like with the CLI, providers may choose to disable features. And again, this is acceptable provided the API endpoints are not modified.

### Optional Features

As we have mentioned before, Cloud Foundry is a huge project consisting of many individual components. Many of the components and features are designed to be optional, allowing operators and commercial providers the opportunity to select the features and capabilities they wish to use. For example, a provider may choose to disable support for Docker images due to security concerns. Or, a provider may choose to leverage a custom user interface while another may choose to make the open-source Stratos interface available. Therefore, you must check with your provider for a list of available features.

### Marketplace

As we mentioned earlier, the marketplace will vary by provider. While most marketplaces contain similar offerings like databases, they may vary in other capabilities and differentiation. For example, some providers may offer 3rd party logging services, volume services, or the ability to provision or manage certificates for custom domains. Each provider will publish a list of services available in their marketplace.

### Commercial Offerings

The open-source distributions of Cloud Foundry can be run standalone or packaged inside commercial offerings. Your company, or even your government, may be running Cloud Foundry, and you don’t even know it. Cloud Foundry is running production workloads in over half of the Fortune 500 companies.

Some offerings denote it in the name, like IBM Cloud Foundry or Atos Cloud Foundry. But Cloud Foundry is also at the core of other platforms, including cloud.gov, SAP Cloud Platform, and VMWare’s Tanzu Application Service. Moreover, this list is by no means exhaustive. There are additional as-a-service offerings widely available and installations of Cloud Foundry in many enterprises and governments worldwide.

Commercial providers are responsible for selecting the features they support, providing marketplace services, and providing value-added, potentially differentiating capabilities around their offerings (if desired). Everyone is encouraged to bring new capabilities to the platform, provided the open-core model is maintained.

## Code on Github

Cloud Foundry is a large, open-source project. All of the source code is available on GitHub. There are a few different organizations:

- https://github.com/cloudfoundry: Core Cloud Foundry projects and components include all source code and documentation.
- https://github.com/cloudfoundry-incubator: Projects under development that are not yet part of the core Cloud Foundry project. The incubator serves as a proving ground. Projects typically graduate to the core Cloud Foundry project or are shuttered.
- https://github.com/cloudfoundry-community: Community-contributed and maintained projects outside of, but related to, the core Cloud Foundry project.
- https://github.com/cloudfoundry-tutorials: The organization that houses this and other tutorials.
- https://github.com/paketo-buildpacks: A collection of Cloud Native Buildpacks designed for Cloud Foundry and/or Kubernetes.

### Industry Standards

Cloud Foundry has fostered projects that have become industry standards. While most Cloud Foundry components are designed to standalone, these projects are specifically aimed at being widely applicable outside of Cloud Foundry:

- [Cloud Native Buildpacks](https://buildpacks.io/): Cloud Native Buildpacks transform your application source code into images that can run on any cloud. Cloud-Native Buildpacks embrace modern container standards, such as the [Open Containers Initiative](https://opencontainers.org/) image format.

- [Paketo Buildpacks](https://paketo.io/): Paketo Buildpacks use the Cloud Native Buildpack specification to provide modular language runtime support for applications. Paketo Buildpacks can run on any platform that supports container images, including Cloud Foundry, Docker, and Kubernetes!

- [Open Service Broker API](https://www.openservicebrokerapi.org/): The Open Service Broker API project allows independent software vendors, SaaS providers, and developers to easily provide backing services to workloads running on cloud-native platforms such as Cloud Foundry and Kubernetes. 

- [BOSH](https://bosh.io): BOSH is a project that unifies release engineering, deployment, and lifecycle management of small and large-scale cloud software. BOSH can provision and deploy software over hundreds of VMs. It also performs monitoring, failure recovery, and software updates with zero-to-minimal downtime.
