---
title: Best practices for testing app reliability
description: Learn about best practices for testing reliability in Azure apps. Implement practices to meet business requirements, so apps run in a healthy state with little downtime.
author: martinekuan
ms.author: martinek
ms.date: 04/27/2023
ms.topic: conceptual
---

# Best practices for testing reliability in Azure applications

This article lists Azure best practices to enhance the testing of Azure applications for reliability. These best practices are derived from our experience with Azure reliability and the experiences of customers like yourself.

While you design the architecture, focus on implementing practices that meet your business requirements, and ensure that applications run in a healthy state without significant downtime.

## Test regularly

Test regularly to validate existing thresholds, targets, and assumptions. Regular testing should be performed as part of each major change and, if possible, on a regular basis. While most testing should be performed within the testing and staging environments, it's often beneficial to also run a subset of tests against the production system.

## Test for resiliency

To test resiliency, you should verify how the end-to-end workload performs under intermittent failure conditions. Consider performing the following tests:

- Performance testing
- Simulation testing
- Fault injection testing
- Load testing
- Operational readiness testing
- Failover and failback testing

## Design a backup strategy

Design a backup strategy that's tailored for the specific business requirements and circumstances of the application. At a high level, the approaches can be divided into these categories: 1) Redeploy on disaster, 2) Warm spare (Active/Passive), and 3) Hot spare (Active/Active).

## Design a disaster recovery strategy

When parts of the Azure network are inaccessible, you might not be able to access your application or data. For this scenario, design a disaster recovery strategy to run most applications with reduced functionality.

## Codify steps to failover and fallback

Codify steps, preferably automatically, to failover and fallback the application to the primary region once a failover-triggering issue has been addressed. Doing this should ensure capabilities exist to effectively respond to an outage in a way that limits its impact.

## Plan for regional failures

Use Azure to create applications that are distributed across regions. Such distribution helps to minimize the possibility that a failure in one region affects other regions.

## Implement retry logic

Track the number of transient exceptions and retries over time to uncover issues or failures in your application's retry logic. A trend of increasing exceptions over time might indicate that the service has an issue and might fail.

## Configure and test health probes

Configure and test health probes for your load balancers and traffic managers. Ensure that your health endpoint checks the critical parts of the system and responds appropriately.

## Segregate read and write interfaces

By implementing the Command and Query Responsibility Segregation (CQRS) pattern to segregate read and write interfaces, you can achieve levels of scale and performance needed for your solution.

## Next steps

> [!div class="nextstepaction"]
> [Monitoring for reliability](./monitor-checklist.md)

Go back to the main article: [Checklist for reliability testing](test-checklist.md)
