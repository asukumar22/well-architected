---
title: Backup and disaster recovery for apps
description: Explore backup and disaster recovery approaches in Azure. Disaster recovery is the process of restoring application functionality after a catastrophic loss.
author: martinekuan
ms.author: martinek
ms.date: 04/26/2023
ms.topic: conceptual
ms.custom:
  - How are you handling DR (Backup & Restore) for this workload?
  - article
categories:
  - management-and-governance
---

# Backup and disaster recovery for Azure applications

*Disaster recovery* is the process of restoring application functionality after a catastrophic loss.

In cloud environments, we acknowledge up front that failures happen. Instead of trying to prevent failures altogether, the goal is to minimize the effects of a single failing component. Testing is one way to minimize these effects. You should automate testing of your applications where possible, but you also need to be prepared for when they fail. When a failure happens, having backup and recovery strategies becomes important.

Your tolerance for reduced functionality during a disaster is a business decision that varies from one application to the next. It might be acceptable for some applications to be temporarily unavailable, or partially available with reduced functionality or delayed processing. For other applications, any reduced functionality is unacceptable.

## Key points

- Create and test a disaster recovery plan regularly using key failure scenarios.
- Design a disaster recovery strategy to run most applications with reduced functionality.
- Design a backup strategy that's tailored for the business requirements and circumstances of the application.
- Automate failover and failback steps and processes.
- Test and validate the failover and failback approach successfully at least once.

## Disaster recovery plan

Start by creating a recovery plan. The plan is considered complete after it's been fully tested. Include the people, processes, and applications needed to restore functionality within the service-level agreement (SLA) that you've defined for your customers.

Consider the following suggestions when you create and test your disaster recovery plan:

- Describe how to contact support and how to escalate issues. This information helps to avoid prolonged downtime as you work out the recovery process for the first time.
- Evaluate the business impact of application failures.
- Choose a cross-region recovery architecture for mission-critical applications.
- Identify a specific owner of the disaster recovery plan, including automation and testing.
- Document the process, especially any manual steps.
- Automate the process as much as possible.
- Establish a backup strategy for all reference and transactional data, and test backup restoration regularly.
- Set up alerts for the stack of Azure services consumed by your application.
- Train operations staff to execute the plan.
- Perform regular disaster simulations to validate and improve the plan.

If you use [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) to replicate virtual machines (VMs), create a fully automated recovery plan to failover the entire application.

## Operational readiness testing

Perform an operational readiness test for failover to the secondary region and for failback to the primary region. Many Azure services support manual failover or test failover for disaster recovery drills. You can simulate an outage by shutting down or removing Azure services.

Automated operational responses should be tested frequently as part of the normal application lifecycle to ensure operational effectiveness.

## Failover and failback testing

Test failover and failback to verify that your application's dependent services come back up in a synchronized manner during disaster recovery. Changes to systems and operations might affect failover and failback functions, but the impact might not be detected until the main system fails or becomes overloaded. Test failover capabilities *before* they're required to compensate for a live problem. Also, be sure dependent services failover and failback in the correct order.

If you use [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) to replicate VMs, run disaster recovery drills periodically by testing failovers to validate your replication strategy. A test failover doesn't affect the ongoing VM replication or your production environment. For more information, see [Run a disaster recovery drill to Azure](/azure/site-recovery/site-recovery-test-failover-to-azure).

## Dependent service outage

For each dependent service, you should understand the implications of service disruption and how the application responds. Many services include features that support resiliency and availability, so evaluating each service independently is likely to improve your disaster recovery plan. For example, Azure Event Hubs supports [failing over](/azure/event-hubs/event-hubs-geo-dr#setup-and-failover-flow) to the secondary namespace.

## Network outage

When parts of the Azure network are inaccessible, you might not be able to access your application or data. For this situation, you should design the disaster recovery strategy to run most applications with reduced functionality.

If reducing functionality isn't an option, the remaining options are application downtime or failover to an alternate region.

In a reduced functionality scenario:

- If your application can't access its data because of an Azure network outage, you can run locally with reduced application functionality by using cached data.
- You can store data in an alternate location until connectivity is restored.

## Recovery automation

The steps required to recover or failover the application to a secondary Azure region in failure situations should be codified, preferably in an automated manner, to ensure capabilities exist to respond effectively to an outage in a way that limits impact. Similar codified steps should also exist to capture the process required to failback the application to the primary region once a failover triggering issue has been addressed.

When automating failover procedures, ensure that the tooling used for orchestrating the failover is also considered in the failover strategy. For example, if you run your failover from Jenkins running on a VM, you'll be in trouble if that virtual machine is part of the outage. Azure DevOps Projects are scoped to a region too.

## Backup strategy

Many alternative strategies are available for implementing distributed compute across regions. These strategies must be tailored to the specific business requirements and circumstances of the application. At a high level, the approaches can be divided into the following categories:

- **Redeploy on disaster**: In this approach, the application is redeployed from scratch at the time of disaster. Redeploying from scratch is appropriate for non-critical applications that don't require a guaranteed recovery time.

- **Warm spare (Active/Passive)**: Create a secondary hosted service in an alternate region, and deploy roles to guarantee minimal capacity. However, these roles don't receive production traffic. This approach is useful for applications that haven't been designed to distribute traffic across regions.

- **Hot spare (Active/Active)**: The application is designed to receive production load in multiple regions. The cloud services in each region might be configured for higher capacity than required for disaster recovery purposes. Instead, the cloud services might scale out as necessary at the time of a disaster and failover. This approach requires a large investment in application design, but it has significant benefits. These include low and guaranteed recovery time, continuous testing of all recovery locations, and efficient usage of capacity.

## Plan for regional failures

Azure is divided physically and logically into units called regions. A region consists of one or more datacenters in close proximity. Many regions and services also support [availability zones](/azure/availability-zones/az-overview), which can be used to provide more resiliency against outages in a single datacenter. Consider using regions with availability zones to improve the availability of your solution.

Under rare circumstances, it's possible that facilities in an entire availability zone or region can become inaccessible, for example, because of network failures. Or, facilities can be lost entirely, for example, because of a natural disaster. Azure has capabilities for creating applications that are distributed across zones and regions. Such distribution helps to minimize the possibility that a failure in one zone or region could affect other zones or regions.

## Related links

- For information on testing failovers, see [Run a disaster recovery drill to Azure](/azure/site-recovery/site-recovery-test-failover-to-azure).
- For information on Event Hubs, see [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Go back to the main article: [Testing for reliability](test-checklist.md)

## Next step

> [!div class="nextstepaction"]
> [Automatic retry of failed backup jobs](./auto-retry.md)
