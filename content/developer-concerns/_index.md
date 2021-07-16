---
title: Developer Concerns
weight: 20
geekdocAlign: left
---

In this section, we look at the capabilities of Cloud Foundry from the perspective of a typical developer.

- **Core Constructs**: Cloud Foundry is a multi-tenant platform designed to support multiple teams or customers with the necessary segregation of tenants. We take a quick look at the constructs that make this possible.

- **Workloads**: Cloud Foundry is not a generalized compute platform. It is therefore important to understand the types of workloads Cloud Foundry can support. 

- **Containerization**: Cloud Foundry runs workloads in containers. The platform supports bringing your containers or, more commonly, simply bringing your applications and allowing the platform to containerize them. In this chapter, we look at these two approaches. 

- **Routing**: Cloud Foundry’s routing model provides developers with flexible capabilities to securely and efficiently route traffic to applications. In this chapter, we cover the platform capabilities for managing routes and domains.

- **Services**: Most applications will need at least one backing service like a database, message queue, or identity service. Cloud Foundry makes it easy for a developer to get access to the services they need. Cloud Foundry also makes it easy to integrate existing services, like an existing database, with apps in the platform.

- **Visibility**: Cloud Foundry provides visibility into the lifecycle of applications. This visibility is quite helpful for development, troubleshooting, and security. 

- **Lifecycle Management**: Applications are not static resources as they will inevitably undergo changes and evolutions over their lifetime. In this chapter, we take a look at what can happen in the lifecycle of an application.
