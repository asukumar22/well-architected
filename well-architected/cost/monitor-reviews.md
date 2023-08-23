---
title: Conduct cost reviews
description: Learn about cost monitoring and cost review to apply proactive and reactive approaches to help create cost controls.
author: PageWriter-MSFT
ms.author: martinek
ms.date: 04/18/2023
ms.topic: conceptual
ms.custom:
  - article
  - internal-intro
---

# Conduct cost reviews

Cost monitoring tracks and reviews cloud costs to help establish budget controls and prevent misuse. It's important to adopt proactive and reactive review approaches for monitoring cost. Stakeholders should conduct cost reviews regularly and include reactive cost reviews. For example, when a budget limit causes an alert.

## Who should be included in a cost review?

Cost monitoring is a complex process that involves planning, cost estimation, budgeting, and cost control, which can include several different teams.

- **Cloud Architect/Administrator​**. Monitor and back-up cloud systems.
- **Product Owner**. Needs awareness of the decisions.
- **Finance Owner and Financial Operations Practitioner**. Need to understand cloud billing to derive business benefits using financial metrics to make effective decisions.

You can identify owners of systems or applications through resource tags.

## Plan and schedule cost reviews

We recommend scheduling cost reviews as part of the regular business reviews.

- Schedule a review during the billing period. This review creates awareness of the estimated pending billing. You can use [Azure Advisor](/azure/advisor/advisor-cost-recommendations), [Advisor Score](/azure/advisor/azure-advisor-score/), and [Azure Cost Management – cost analysis](/azure/cost-management-billing/costs/) to help with the reports.
- Schedule a review after the billing period. This review shows the actual cost with activity for that month. Use [TimeframeType](/rest/api/cost-management/query/usage?tabs=HTTP#timeframetype) to generate monthly reports. The APIs can query data that gets information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges. For more information, see [Cost Management automation overview](../devops/automation-overview.md).
- Schedule a review when you receive a [budget alert](/azure/cost-management/cost-mgt-alerts-monitor-usage-spending) or Azure Advisor recommendation.

Web Direct (pay-as-you-go) and Cloud Solution Provider (CSP) billing occurs monthly, while Enterprise Agreement (EA) billing occurs annually. But ensure you do a cost review regardless of the billing cycle.
