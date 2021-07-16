---
title: "Core Constructs"
weight: "1"
---

Cloud Foundry is a multi-tenant platform designed to support multiple teams or customers with the necessary segregation of tenants. In this section, we take a quick look at the constructs that make this possible.

## Organizations and Spaces

Organizations (orgs) and spaces are logical separations within a Cloud Foundry instance. Spaces live within orgs, and a single org can contain one or more spaces.

Typically orgs separate tenants or projects. So, for example, an org could exist for each project or each tenant. Each org might then have separate spaces for different lifecycle stages, like development, staging, and production.

## Role-Based Access Control

Cloud Foundry leverages Role-Based Access Control (RBAC) to restrict user actions within the platform. Users can be assigned roles globally (Cloud Foundry-wide) or assigned roles in specific orgs and spaces. 

### Global Roles
Users can be assigned global roles and capabilities that span an entire Cloud Foundry deployment. 

* **Admin**: Operational actions on all orgs and spaces.
* **Admin Read-Only**: Visibility of all orgs and spaces without the ability to modify resources.
* **Global Auditor**: Similar to the `Admin Read-Only` role, except that this role cannot see secrets such as environment variable content.

### Org Roles
Org roles grant users access at the org level.

* **OrgManager**: Administer the org and spaces in that org.
* **OrgAuditor**: Read-only access to the org.
* **BillingManager**: Manage billing account and payment information associated with an org in Cloud Foundry instances that have deployed the billing engine.

### Space Roles
Space roles grant user access at the space level.

* **SpaceManager**: Administer users and roles in the space.
* **SpaceDeveloper**: Manage apps, services, and routes in a space. A user must have the `SpaceDeveloper` role to deploy apps.
* **SpaceAuditor**: Read-only access to the space.

Org roles do not cascade into spaces. Therefore, an `OrgManager` *cannot* deploy apps to spaces in their org. However, they can grant the `SpaceDeveloper` role to a user (including themselves) for a particular space.

## Quotas
Quotas are named sets of memory, service, and instance usage limits. Quotas apply to orgs and spaces. Org quotas are mandatory, while space quotas are optional. Org-level quotas are shared across all spaces in that org.

Typically, Cloud Foundry operators will establish quotas for orgs, and OrgManagers will establish quotas for spaces. After all, resource planning is a collaborative effort.

## Impact

The constructs in this chapter lay the foundation for a secure, multi-tenant platform that enables users to be self-sufficient. Security, compliance, and operations teams work together to define the guard rails in the platform while freeing developers to work unimpeded within those confines.

Compliance requirements often dictate regular audits of environments. The above constructs are easily managed using [Terraform](https://www.terraform.io/) and the [Cloud Foundry Terraform Provider](https://registry.terraform.io/providers/cloudfoundry-community/cloudfoundry/latest), enabling automated auditing and enforcement across all organizations. 



