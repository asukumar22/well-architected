---
title: Performance data integration
description: Analyze performance data holistically to detect fault types, bottlenecks regressions, and health states.
author: PageWriter-MSFT
ms.author: martinek
ms.date: 05/04/2023
ms.topic: conceptual
categories:
  - management-and-governance
ms.custom:
  - fasttrack-edit
---

# Performance data integration

Performance testing and investigation should be based on data captured from repeatable processes. To understand how an application's performance is affected by code and infrastructure changes, retain data for analysis. Also, it's important to measure how performance has changed *over time*, not just compared to the last measurement taken.

This article describes some considerations and tools you can use to aggregate data for troubleshooting and analyzing performance trends.

## Key points

> [!div class="checklist"]

> - Analyze performance data holistically to detect fault types, bottleneck regressions, and health states.
> - Use log aggregation technologies to consolidate data into a single workspace and analyze using a sophisticated query language.
> - Retain data in a time-series database to predict performance issues before they occur.
> - Balance the retention policy and service pricing plans with the cost expectation of the organization.

## Data interpretation

The overall performance can be impacted by both application-level issues and resource-level failures. It's vital that all data is correlated and evaluated together to optimize the detection of issues and troubleshooting of detected issues. This approach helps distinguish between transient and nontransient faults.

Use a holistic approach to quantify what *healthy* and *unhealthy* states represent across all application components. We recommend that a *traffic light* model is used to indicate a healthy state. For example, use a green light to show that key nonfunctional requirements and targets are fully satisfied and resources are optimally used. For example, a healthy state can mean that 95% of requests are processed in <= 500 ms with AKS node utilization at x%, and so on. Also, an [Application Map](/azure/azure-monitor/app/app-map?tabs=net) can help spot performance bottlenecks or failure hotspots across components of a distributed application.

Analyze long-term operational data to get historical context and detect if there have been any regressions. For example, check the average response times to see if they've been slowly increasing over time and getting closer to the maximum target.

## Aggregated view

Log aggregation technologies should be used to collate logs and metrics across all application components, including infrastructural components for later evaluation.

Resources can include Azure infrastructure as a service (IaaS) and platform as a service (PaaS) services and third-party appliances, such as firewalls or antimalware solutions used in the application. For example, if Azure Event Hubs is used, the diagnostic settings should be configured to push logs and metrics to the data sink.

[Azure Monitor](/azure/azure-monitor/data-platform) can collect and organize log and performance data from monitored resources. Data is consolidated into an [Azure Log Analytics workspace](/azure/azure-monitor/logs/log-analytics-workspace-overview) so they can be analyzed together using a sophisticated query language that can quickly analyze millions of records. Splunk is another popular choice.

**How is aggregated monitoring enforced**?
***

All application resources should be configured to route diagnostic logs and metrics to the chosen log aggregation technology. Use [Azure Policy](/azure/governance/policy/overview) to ensure the consistent use of diagnostic settings across the application and to enforce the desired configuration for each Azure service.

## Long-term data

Store long-term operational data to understand the history of application performance. This data store is important for analyzing performance trends and regressions.

**Are long-term trends analyzed to predict performance issues before they occur**?
***
It's often helpful to store such data in a time-series database (TSDB) and then view the data from an operational dashboard. An [Azure Data Explorer cluster](https://azure.microsoft.com/services/data-explorer/) is a powerful TSDB that can store any schema of data, including performance test metrics. [Grafana](https://grafana.com/), an open-source platform for observability dashboards, can then be used to query your Azure Data Explorer cluster to view performance trends in your application.

**Have retention times been defined for logs and metrics, with housekeeping mechanisms configured**?
***

Clear retention times should be defined to allow for suitable historic analysis and to control storage costs. Suitable housekeeping tasks should also be used to archive data to cheaper storage or aggregate data for long-term trend analysis.

## Cost optimization

While correlating data is recommended, there are cost implications to storing long-term data. For example, Azure Monitor is capable of collecting, indexing, and storing massive amounts of data in a Log Analytics workspace. The cost of a Log Analytics workspace is based on amount of storage, retention period, and the plan. For more information about how you can balance cost, see [Manage usage and costs with Azure Monitor Logs](/azure/azure-monitor/logs/manage-cost-storage).

If you're using Application Insights to collect instrumentation data, there are cost considerations. For more information, see [Manage usage and costs for Application Insights](/azure/azure-monitor/app/pricing).

## Next

> [!div class="nextstepaction"]
> [Scalability and reliability considerations](monitor-analyze.md)

## Related links

- [Application Map](/azure/azure-monitor/app/app-map?tabs=net)
- [Azure Policy](/azure/governance/policy/overview)
- [Azure Data Explorer cluster](https://azure.microsoft.com/services/data-explorer/)
- [Manage usage and costs with Azure Monitor Logs](/azure/azure-monitor//logs/manage-cost-storage)
- [Manage usage and costs for Application Insights](/azure/azure-monitor//app/pricing)

> [Back to the main article](checklist.md)
