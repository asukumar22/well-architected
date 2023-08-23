---
title: Checklist - Monitor cost
description: Learn the tasks you need to complete to monitor your Azure workload cost. Checklist items include getting cost data from diverse sources, using resource tag policies, and more.
author: martinekuan
ms.author: martinek
ms.date: 04/06/2023
ms.topic: conceptual
ms.custom:
  - article
  - internal-intro
---

# Checklist - Monitor cost

Use this checklist to monitor the cost of the workload. 

- **Gather cost data from diverse sources to create reports**. Start with tools like [Azure Advisor](/azure/advisor/advisor-cost-recommendations), [Advisor Score](/azure/advisor/azure-advisor-score), and [Azure Cost Management](/azure/cost-management-billing/costs/). Build custom reports relevant for the business by using [Consumption APIs](/rest/api/consumption/).
  - [Cost reports](./monitor-reports.md)
  - [Review costs in Cost analysis](/azure/cost-management-billing/costs/quick-acm-cost-analysis#review-costs-in-cost-analysis)

- **Use resource tag policies to build reports**. Use tags to identify the owners of systems or applications and create custom reports.
  - [Follow a consistent tagging standard](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#metadata-tags)
  - [Video: How to review tag policies with Azure Cost Management](https://www.youtube.com/watch?v=nHQYcYGKuyw)

- **Use Azure built-in roles for cost**. Give access only to users who you want to view and analyze cost reports. You can define roles by their scope. For example, use the **Cost Management Reader role** to enable users to view costs for their resources in subscriptions or resource groups.
  - [Provide the right level of cost access](/azure/cloud-adoption-framework/ready/azure-best-practices/track-costs#provide-the-right-level-of-cost-access)
  - [Azure RBAC scopes](/azure/cost-management-billing/costs/understand-work-scopes#azure-rbac-scopes)

- **Respond to alerts and have a response plan according to the constraints**. Respond to alerts quickly and identify possible causes and any required action.
  - [Budget and alerts](monitor-alert.md)
  - [Use cost alerts to monitor usage and spending](/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending)

- **Adopt both proactive and reactive approaches for cost reviews**. Conduct cost reviews at a regular cadence to determine the cost trend. Also, review reports that are created because of alerts.
  - [Conduct cost reviews](./monitor-reviews.md)
  - [Participate in central governance cost reviews](/azure/cloud-adoption-framework/govern/cost-management/compliance-processes)

- **Analyze the cost at all scopes by using Cost analysis**. Identify services that drive the cost through different dimensions, such as location, usage meters, and so on. Review whether certain optimizations bring results. For example, analyze costs associated with reserved instances, savings plans, and Spot virtual machines (VMs) against business goals.
  - [Quickstart: Explore and analyze costs with Cost analysis](/azure/cost-management-billing/costs/quick-acm-cost-analysis)

- **Detect anomalies and identify changes in business or applications that might have contributed changes in cost**. Focus on these factors:
  - Traffic pattern as the application scales
  - Budget for the usage meters on resources
  - Performance bottle necks
  - CPU utilization and network throughput
  - Storage footprint for blobs, backups, and archiving

- **Use Visualization tools to analyze cost information**.
  - [Create visuals and reports with in Power BI Desktop](/power-bi/desktop-connect-azure-cost-management)
  - [Cost Management App](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp)
