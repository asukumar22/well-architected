---
title: Automation overview of goals, best practices, and types in Azure
description: Learn how automation improves workload operations, removes risks associated with manual processes, and lets engineers focus on tasks that add business value.
author: martinekuan
ms.author: martinek
ms.date: 05/19/2023
ms.topic: conceptual
ms.custom:
  - article
---

# Automation overview: Goals, best practices, and types

Automation has revolutionized how businesses operate and this trend continues to evolve. Businesses have moved to automating manual processes so that engineers can focus attention on tasks that add business value. Automating business solutions lets you:

- Activate resources on demand.
- Deploy solutions rapidly.
- Minimize human error in configuring repetitive tasks.
- Produce consistent and repeatable results.

For more information, see [Deployment considerations for automation](./release-engineering-cd.md#automation).

## Goals of automation

When you automate technical processes, a common approach for some organizations is to automate what they can and leave the more difficult processes for humans to perform manually.

A goal of automation is to make tools that do what humans can do, only better. For example, a human can perform any given task once. But when the task requires repetitive runs, especially over long time periods, an automated system is better equipped to do that work with more predictable, error-free results. Increasing speed is another goal in automation. When you practice these automation goals, you can build systems that are faster, repetitive, and can run on a daily basis.

## Automate toil to improve efficiency

Most automation involves a percentage of *toil*. Toil is the operational work that's related to a process that's manual, repetitive, can be automated, and has minimal value. It's counterproductive to automation but in many organizations, a small amount of toil is unavoidable. It becomes an issue when too much toil slows progress. A project's production velocity decreases if engineers are continuously interrupted by manual tasks attributed to toil, either planned or unplanned. Too much toil can impact job satisfaction. Engineers become dissatisfied when they find themselves spending too much time on operational toil rather than on other projects.

Automation should be developed, and increased, so engineers can eliminate future toil. By reducing toil, engineers can concentrate on innovating business solutions.

For more information, see [Toil automation](https://www.coursera.org/lecture/developing-a-google-sre-culture/toil-automation-BpNqj).

## Automation best practices

- **Ensure consistency:** The more manual processes are involved, the more prone to human error. Manual processes can lead to mistakes, oversights, reduction of data quality, and reliability problems.
- **Centralize mistakes:** Choose a platform that lets you fix bugs in one place in order to fix them everywhere. This best practice reduces the chance of error and the possibility of the bug being reintroduced.
- **Identify issues quickly:** Complex issue might not always be identifiable in a timely manner. But with good automation, detection of these issues should occur quickly.
- **Maximize employee productivity:** Automation leads to more innovative solutions, and in general provides more value to the business. This improvement in turn raises morale and job satisfaction. Once a process is automated, training and maintenance can be greatly reduced or eliminated. This shift frees engineers to spend less time on manual processes and more time on automating business solutions.

## Types of automation

Three types of automation are described in this article:

- Infrastructure deployment
- Infrastructure configuration
- Operational tasks

These categories share the same goals and best practices mentioned previously. They differ in areas where Azure provides solutions that help achieve optimal automation. Other types of automation, such as continuous deployment and continuous delivery, are described further in the Operational Excellence pillar.

### Infrastructure deployment

As businesses move to the cloud, they need to *repeatedly* deploy their solutions and know that their infrastructure is in a *reliable* state. To meet these challenges, you can automate deployments by using a practice referred to as [infrastructure as code](./automation-infrastructure.md). In code, you define the infrastructure that needs to be deployed.

There are many deployment technologies you can use with Azure. Here are three examples:

- [Azure Resource Manager (ARM) templates](./automation-infrastructure.md#automate-deployments-with-arm-templates)
- [Azure Bicep](/azure/azure-resource-manager/bicep/)
- [Terraform](./automation-infrastructure.md#automate-deployments-with-terraform)

These technologies use a *declarative* approach. This approach lets you state what you intend to deploy without having to write the sequence of programming commands to create it. You can deploy not only virtual machines, but also the network infrastructure, storage systems, and any other resources you might need.

### Infrastructure configuration

If you don't manage configuration carefully, your business could encounter disruptions such as systems outages and security issues. Optimal configuration enables you to quickly detect and correct configurations that could interrupt or slow performance.

When creating new resources on Azure, you can take advantage of configuration as code to [bootstrap](./automation-configuration.md#bootstrap-automation) the deployment.

Configuration tools can also be used to [configure and manage](./automation-configuration.md#configuration-management) the ongoing state of deployed resources.

### Operational tasks

As the demand for speed in performing operational tasks increases over time, you're expected to deliver things faster and faster. Manually performing operational tasks will fail to scale as demand increases. This is where automation can help. To meet on-demand delivery using an automation platform, you need to develop automation components (such as runbooks and configurations), create integrations to systems that are already in place efficiently, and operate and troubleshoot.

Advantages of automating operational tasks include:

- Optimize and extend existing processes.
- Deliver flexible and reliable services.
- Lower costs.
- Improve predictability.

Two popular options for automating operational tasks are:

- [Azure Functions](./automation-tasks.md#azure-functions) - Run code without managing the underlying infrastructure on where the code is run.
- [Azure Automation](./automation-tasks.md#azure-automation) - Uses programming and scripting language to automate operational tasks in code and run on demand.

For more information, see [Automation](./automation-tasks.md).

## Next steps

> [!div class="nextstepaction"]
> [Automate repeatable infrastructure](./automation-infrastructure.md)
