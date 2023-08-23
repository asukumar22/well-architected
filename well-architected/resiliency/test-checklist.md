---
title: Checklist for reliability testing
description: Use a reliability checklist for app testing. Validate existing thresholds, targets, assumptions, the health and capacity models, and operational procedures.
author: martinekuan
ms.author: martinek
ms.date: 04/27/2023
ms.topic: conceptual
---

# Checklist for reliability testing

Regular testing should be performed as part of each major change and, if possible, on a regular basis to validate existing thresholds, targets, and assumptions. Testing should also ensure the validity of the health model, capacity model, and operational procedures.

## Checklist

Have you tested your applications with reliability in mind?

> [!div class="checklist"]
> - Test regularly to validate existing thresholds, targets, and assumptions.
> - Automate testing as much as possible.
> - Perform testing on both key test environments and the production environment.
> - Perform chaos testing by injecting faults.
> - Create and test a disaster recovery plan on a regular basis by using key failure scenarios.
> - Design a disaster recovery strategy to run most applications with reduced functionality.
> - Design a backup strategy that's tailored for the business requirements and circumstances of the application.
> - Test and validate the failover and failback approach successfully at least once.
> - Configure request timeouts to manage intercomponent calls.
> - Implement retry logic to handle transient application failures and transient failures with internal or external dependencies.
> - Configure and test health probes for your load balancers and traffic managers.
> - Apply chaos principles continuously.
> - Create and organize a central chaos engineering team.

## Azure services

- [Azure Site Recovery](/azure/site-recovery/site-recovery-overview)
- [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops&preserve-view=true)
- [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview)
- [Azure Load Balancer](/azure/load-balancer/load-balancer-overview)

## Reference architecture

- [Failure mode analysis for Azure applications](/azure/architecture/resiliency/failure-mode-analysis)
- [High availability and disaster recovery scenarios for IaaS apps](/azure/architecture/example-scenario/infrastructure/iaas-high-availability-disaster-recovery)
- [Back up files and applications on Azure Stack Hub](/azure/architecture/hybrid/azure-stack-backup)

## Related links

- For information on performance testing, see [Performance testing](../scalability/performance-test.md).
- For information on chaos engineering, see [Chaos engineering](./chaos-engineering.md).
- For information on failure and disaster recovery, see [Backup and disaster recovery for Azure applications](./backup-and-recovery.md).
- For information on testing applications, see [Testing your application and Azure environment](../devops/release-engineering-testing.md).

## Next steps

> [!div class="nextstepaction"]
> [Resiliency testing](./testing.md)
