---
title: Monitor application health for reliability
description: Use application health monitoring to improve application reliability in Azure. Learn how to work with alerts, subscriptions, service limits, and instrumentation.
author: martinekuan
ms.author: martinek
ms.date: 04/27/2023
ms.topic: conceptual
categories:
  - management-and-governance
---

# Monitoring application health for reliability

Monitoring and diagnostics are crucial for availability and resiliency. If something fails, you need to know *that* it failed, *when* it failed, and *why*.

*Monitoring* isn't the same as *failure detection*. For example, your application might detect a transient error and retry, avoiding downtime. It should also log the retry operation so that you can monitor the error rate to get an overall picture of application health.

## Key points

- Define alerts that are actionable and effectively prioritized.
- Create alerts that poll for services nearing their limits and quotas.
- Use application instrumentation to detect and resolve performance anomalies.
- Track the progress of long-running processes.
- Troubleshoot issues to gain an overall view of application health.

## Alerting

Alerts are notifications of system health issues that are found during monitoring. Alerts only deliver value if they're actionable and effectively prioritized by on-call engineers through defined operational procedures. Present data in a dashboard or email alert format that makes it easy for an operator to notice problems or trends quickly.

### Service-level alerts

Use Azure Service Health to respond to *service level* events. Azure Service Health provides a view into the health of Azure services and regions. It issues communications that affect the following services:

- Outages
- Planned maintenance activities
- Other health advisories

Azure Service Health alerts should be configured to operationalize Service Health events. However, Service Health alerts shouldn't be used to detect issues because of associated latencies. There's a five-minute service-level objective (SLO) for automated issues. Many issues require manual interpretation to define a root cause analysis (RCA). Instead, alerts should be used to provide useful information to help interpret issues that have been detected and surfaced through the health model to inform an operational response.

For more information, see [Azure Service Health](/azure/service-health/service-health-overview).

### Resource-level alerts

Use Azure Resource Health to respond to *resource level* events. Azure Resource Health provides information about the health of individual resources such as a specific virtual machine. It's useful to diagnose unavailable resources.

Azure Resource Health alerts should be configured for specific resource groups and resource types. These alerts should be adjusted to maximize signal to noise ratios. For example, only distribute a notification when a resource becomes unhealthy according to the application health model or due to an Azure platform initiated event.

It's important to consider transient issues when setting an appropriate threshold for resource unavailability. For example, configure an alert for a virtual machine with a threshold of one minute for unavailability before an alert is triggered.

For more information, see [Azure Resource Health](/azure/service-health/resource-health-overview).

### Dashboards

You can also get a full-stack view of application state by using [Azure dashboards](/azure/azure-portal/azure-portal-dashboards) to create a combined view of monitoring graphs from the following sources:

- Application Insights
- Azure Monitor Logs
- Azure Monitor metrics
- Service Health

### Samples

:::image type="icon" source="../_images/github.png" border="false"::: Here are some samples about creating and querying alerts:

- [HealthAlerts](https://github.com/mspnp/samples/tree/master/Reliability/HealthAlerts). A sample about creating resource-level health activity log alerts. The sample uses Azure Resource Manager to create alerts.
- [GraphAlertsPsSample](https://github.com/mspnp/samples/tree/master/Reliability/GraphAlertsPsSample). Azure PowerShell and Azure commands that query for alerts generated against your subscription.

## Azure subscription and service limits

Azure subscriptions have limits on certain resource types, such as number of resource groups, cores, and storage accounts. To ensure that your application doesn't run up against Azure subscription limits, create alerts that poll for services nearing their limits and quotas.

Address the following subscription limits with alerts.

### Individual services

Individual Azure services have consumption limits on:

- Storage
- Throughput
- Number of connections
- Requests per second

Your application fails if it attempts to use resources beyond these limits. This situation results in service throttling and possible downtime.

Depending on the specific service and your application requirements, you can often stay under these limits by *scaling up* or *scaling out*. Scaling up could involve choosing another pricing tier. Scaling out involves adding new instances.

### Azure storage scalability and performance targets

Azure allows a maximum number of storage accounts per subscription. If your application requires more storage accounts than are currently available in your subscription, create a new subscription with extra storage accounts. For more information, see [Azure subscription and service limits, quotas, and constraints](/azure/azure-resource-manager/management/azure-subscription-service-limits).

### Scalability targets for virtual machine disks

An Azure infrastructure as a service (IaaS) virtual machine supports attaching many data disks, depending on several factors. The factors include the virtual machine size and the type of storage account. If your application exceeds the scalability targets for virtual machine disks, provision more storage accounts and create the virtual machine disks there. For more information, see [Scalability and performance targets for VM disks](/azure/virtual-machines/disks-scalability-targets).

### Virtual machine size

If the actual CPU, memory, disk, and I/O of your virtual machines approach the limits of the virtual machine size, your application might experience capacity issues. To correct the issues, increase the virtual machine size.

If your workload fluctuates over time, consider using Azure Virtual Machine Scale Sets to automatically scale the number of virtual instances. Otherwise, you need to manually increase or decrease the number of virtual machines.

### Azure SQL Database

If your Azure SQL Database tier isn't adequate to handle your application's Database Transaction Unit (DTU) requirements, your data use is throttled. For more information on selecting the correct service plan, see [Compare vCore and DTU-based purchasing models](/azure/azure-sql/database/purchasing-models).

## Instrumentation

Use instrumentation to measure the customer experience. Effective instrumentation is vital for detecting and resolving performance anomalies that can affect customer experience and application availability. To build a robust application health model, it's vital that you achieve visibility into the operational state of critical internal dependencies, such as a shared Network Virtual Appliance (NVA) or Azure ExpressRoute connection.

Automated failover and failback systems depend on the correct functioning of monitoring and instrumentation. Dashboards that visualize system health and operator alerts also depend on having accurate monitoring and instrumentation. These elements might fail, miss critical information, or report inaccurate data. If so, an operator might not realize that the system is unhealthy or failing. Make sure that you include monitoring systems in your test plan.

Use applications to track calls to dependent services. Dependency tracking and measuring the duration or status of dependency calls is also vital to measuring overall application health. It should be used to inform a health model for the application.

We recommend collecting and storing logs and key metrics of critical components.

Provide rich instrumentation:

- For failures that are likely, but haven't yet occurred, provide enough data to determine the cause, mitigate the situation, and ensure that the system remains available.
- For failures that have already occurred, the application should return an appropriate error message to the user. The application should attempt to continue running despite reduced functionality.

Monitoring systems should capture comprehensive details so that applications can be restored efficiently. Designers and developers can modify the system to prevent the situation from recurring.

## Long-running workflow failures

Long-running workflows often include multiple steps, each of which should be independent.

Track the progress of long-running processes to minimize the likelihood that the entire workflow needs to be rolled back or that multiple compensating transactions need to be run.

> [!TIP]
> Monitor and manage the progress of long-running workflows by implementing a pattern such as [Scheduler Agent Supervisor](/azure/architecture/patterns/scheduler-agent-supervisor).

## Analysis and diagnosis

Analyze data combined in the data stores to troubleshoot issues and gain an overall view of application health. Generally, you can search for and analyze the data in [Application Insights](/azure/azure-monitor/app/app-insights-overview) and [Azure Monitor Logs](/azure/azure-monitor/logs/log-analytics-overview) using Kusto queries. View preconfigured graphs using management solutions. Use [Azure Advisor](/azure/advisor/advisor-overview) to view recommendations with a focus on resiliency and performance.

## Related links

- For information on dashboards, see [Azure dashboards](/azure/azure-portal/azure-portal-dashboards).
- For information on virtual machine sizes, see [Sizes for virtual machines in Azure](/azure/virtual-machines/sizes).
- For information on scale sets, see [Virtual Machine Scale Sets overview](/azure/virtual-machine-scale-sets/overview).

## Next steps

> [!div class="nextstepaction"]
> [Health modeling for reliability](./monitor-model.md)

Go back to the main article: [Monitoring for reliability](monitor-checklist.md)
