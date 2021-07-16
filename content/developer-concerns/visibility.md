---
title: "Visibility"
weight: "6"
---

Cloud Foundry provides visibility into the lifecycle of applications. This visibility is quite helpful for development, troubleshooting, and security. 

## Logs

Cloud Foundry aggregates logs from all app instances into a single stream. No additional logging libraries are required. Developers simply direct logs to standard output and standard error. The log stream for an application includes logs from Cloud Foundry components involved with the execution of the app (routers, container metrics, etc.).

Cloud Foundry provides the ability to stream logs in near real-time or view the most recent logs. For long-term storage, logs are drained to 3rd party logging services like Splunk and Papertrail.

## Events

There are two main types of Cloud Foundry events: audit events and usage events. Audit events depict actions taken against resources, such as app restarts, stops, scaling, route mapping, and many more. Usage events represent consumption.

## Container Metrics

Metrics for Cloud Foundry application containers can be viewed in the log stream or extracted via CLI plugins. Additionally, the [Stratos](https://github.com/cloudfoundry/stratos) user interface graphs container metrics for applications over time. 


## Impact

Developers get access to logs regardless of the number of application instances. They can continue to use logging libraries in code without the need for another 3rd party service. Developers get the information they need, but only the information they need. 

Integration with 3rd party log aggregation services is reliable and straightforward. Logs, metrics, and events are powerful resources in identifying patterns and potential incidents.
