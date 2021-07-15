---
title: "Services"
weight: "5"
---

Most applications will need at least one backing service like a database, message queue, or identity service. Cloud Foundry makes it easy for a developer to get access to the services they need. Cloud Foundry also makes it easy to integrate existing services, like an existing database, with apps in the platform.

## Managed Services

Managed services allow Cloud Foundry users to provision instances of databases, message queues, logging services, and others, as well as credentials on-demand, using standard Cloud Foundry commands. Platform operators manage service offerings. The 'marketplace' lists available services. 

Managed services integrate with Cloud Foundry through an API called the [Open Service Broker API](https://www.openservicebrokerapi.org/) (OSBAPI). The OSBAPI is an industry-standard API supported by many cloud platforms and Kubernetes. Provisioning, deprovisioning, and credential management is implemented in a service broker. Each broker advertises a catalog of one or more services, each with a tiered offering called a 'plan'. Platform operators control the available services and plans for each organization.

By standardizing on the OSBAPI, Cloud Foundry can make available any service offering with a broker. Developers only need to know a few simple commands:

- `cf marketplace`: View the marketplace to discover available services.
- `cf create-service`: Provision a new service instance (deprovisioned with `cf delete-service`).
- `cf bind-service`: Generate unique credentials for your application (revoked with `cf unbind-service`).

The OSBAPI is an industry-standard API supported by many platforms and cloud providers, including Kubernetes.

## User-Provided Services

Cloud Foundry also makes it easy to work with existing services provisioned outside the marketplace. These services are referred to as 'user-provided service instances' (upsi). User-provided services are effectively a set of key-value pairs representing credentials for the existing service. The same bind and unbind commands are to tell applications about them.

## Impact

Developers can quickly get access to the services they need. Ticketing systems or detailed knowledge on how to deploy or configure services like databases are not needed. All services, self-provisioned or user-provided, can be used consistently.

Manual credential management is also a thing of the past. For example, in a dev space, the app is bound to a dev database. In production, the production app instances are bound to a production database. Therefore, there is no need to manage sensitive config files.

Managed services create a self-service system with guard rails. Granular control over plan availability coupled with quotas creates a safe environment where developers can work unimpeded without worry.

By standardizing on the OSBAPI, services become easily auditable and verifiable. Functions for provisioning and credential management are encapsulated in code and therefore can be tested accordingly. The wild-west of services is over.
