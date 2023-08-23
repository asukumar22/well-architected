---
title: Applications and services
description: Secure your applications and services in Azure. Applications and the data associated with them act as the primary store of business value on a cloud platform.
author: martinekuan
ms.author: martinek
ms.date: 12/08/2021
ms.topic: conceptual
ms.custom:
  - article
---

# Applications and services

Applications and the data associated with them act as the primary store of business value on a cloud platform. Applications can play a role in risks to the business because:

- **Business processes** are encapsulated and executed by applications and services need to be available and provided with high integrity.
- **Business data** is stored and processed by application workloads and requires high assurances of confidentiality, integrity, and availability.

## Identify and classify business critical applications

Enterprise organizations typically have a large application portfolio, but not all applications have equal importance. Applications can be classified based on a criticality scale. For example, business-critical applications are designed to prevent financial losses, safety-critical are focused on costs associated with loss of human life. [**Mission-critical applications**](/azure/architecture/framework/mission-critical/mission-critical-overview) cover both aspects that can be impacted by unavailability or underperformance. 

Criticality should be identified and classified, to direct investment of monitoring, time, and resources appropriately. You should also identify applications or systems with significant access — those which might grant control over other critical systems or data.

For more information, see [Criticality scale](/azure/cloud-adoption-framework/manage/considerations/criticality#criticality-scale) in Cloud Adoption Framework.

### Suggested actions

Identify and classify key organizational applications according to organizational impact.

## Next steps

See these best practices related to PaaS applications.

> [!div class="nextstepaction"]
> [Securing PaaS deployments](/azure/security/fundamentals/paas-deployments)

Secure communication paths between applications and the services. Make sure that there's a distinction between the endpoints exposed to the public internet and private ones. Also, the public endpoints are protected with web application firewall.

> [!div class="nextstepaction"]
> [Network security](./design-network.md)
