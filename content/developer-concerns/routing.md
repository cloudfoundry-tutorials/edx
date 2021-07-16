---
title: "Routing"
weight: "4"
---

Cloud Foundry’s routing model provides developers with flexible capabilities to securely and efficiently route traffic to applications. In this chapter, we cover the platform capabilities for managing routes and domains.


## Apps and routes

In Cloud Foundry, a route maps client traffic to one or more applications. Apps and routes have a many-to-many relationship. While the default behavior on an initial application deployment is to create and assign a route, apps can have zero or more routes. Likewise, a single route can be mapped to zero or more apps.

The commands to manage routes are comprehensive and straightforward. There is no need for developers to manipulate DNS or load balancers as Cloud Foundry routers maintain route tables with up-to-date information. 

## External domains

External domains are routable from outside of Cloud Foundry. The default app domain used in the Katacoda scenario is an example of such a domain. Apps accessible from outside of Cloud Foundry are assigned one or more external domains.

While every Cloud Foundry instance has a default domain for applications, it is trivially easy to add custom domains to a Cloud Foundry instance. Domains are enabled at the org level. Note, adding a new domain typically requires coordination with operators to ensure valid certificates are configured correctly.

### Internal domains

For security purposes, it is desirable to have some applications be inaccessible from outside of Cloud Foundry. A common example is a RESTful data service that should not be routable from the internet. Routes  on internal domains are only resolvable by other apps in that same Cloud Foundry deployment.

Cloud Foundry instances have a default internal domain of 'apps.internal'. You can create routes on this domain and map them to your applications. An overlay network controls traffic on this network, adding a level of security. Developers declaratively control communication between apps.

## Impact

For developers, simplicity is again the name of the game. Simple-to-use commands encapsulate traditionally error-prone functionality. Additionally, it is easy for developers to support capabilities like [A/B testing](https://en.wikipedia.org/wiki/A/B_testing) or custom branding based on URL.

Internal domains add a significant level of security to the applications that use them. They have the added benefit of being very efficient as traffic never leaves the overlay network. 

Cloud Foundry enables the secure, self-service use of domains and routes. These constructs are easily auditable via the Cloud Foundry API.
