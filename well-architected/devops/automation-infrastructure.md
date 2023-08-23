---
title: Repeatable Infrastructure
description: Learn about the concept of repeatable infrastructure. Use Azure landing zones. Deploy infrastructure with code. Automate deployments with ARM templates and Terraform.
author: martinekuan
ms.author: martinek
ms.date: 04/28/2023
ms.topic: conceptual
ms.custom:
  - article
  - devx-track-bicep
  - devx-track-terraform
---

# Repeatable infrastructure

Historically, deploying a new service or application involves manual work such as procuring and preparing hardware, configuring operating environments, and enabling monitoring solutions. Ideally, an organization would have multiple environments in which to test deployments. These test environments should be similar enough to production that deployment and run time issues are detected before deployment to production. This manual work takes time, is error-prone, and can produce inconsistencies between the environments if not done well.

Cloud computing changes the way we procure infrastructure. No longer are we unboxing, racking, and cabling physical infrastructure. We have internet accessible management portals and REST interfaces to help us. We can now provision virtual machines, databases, and other cloud services on demand and globally. When we no longer need cloud services, they can be easily deleted. 

However, cloud computing alone doesn't remove the effort and risk in provisioning infrastructure. When you use a cloud portal to build systems, many of the same manual configuration tasks remain. Application servers require configuration, databases need networking, and firewalls need firewalling.

## Azure landing zones (repeatable environment configuration)

Organizations that manage, govern, or support multiple workloads in the cloud will require repeatable **and consistent** environments. [Azure landing zones](/azure/cloud-adoption-framework/ready/landing-zone/) provide central operations teams (also known as platform teams) with a repeatable approach to environmental configuration. To deliver consistent environments, all Azure landing zones provide a set of common design areas, reference architecture, reference implementation, and a process to modify that deployment to fit the organization design requirements.

This environment is primarily enabled by using the [Azure landing zone design principles](/azure/cloud-adoption-framework/ready/landing-zone/design-principles) of policy-driven governance alongside subscription democratization.

The following links are from the Cloud Adoption Framework to help you deploy Azure landing zones:

- Azure landing zones adhere to a [common set of design areas](/azure/cloud-adoption-framework/ready/landing-zone/design-areas) to guide configuration of required environment considerations including: Identity, Network topology and connectivity, Resource organization, Governance disciplines, Operations baseline, and Business continuity and disaster recovery (BCDR)
- Azure landing zones can be deployed through via:
    - [Azure landing zones accelerator portal](/azure/cloud-adoption-framework/ready/landing-zone/#azure-landing-zone-accelerator)
    - [Azure landing zones - Bicep modules design considerations](/azure/architecture/landing-zones/bicep/landing-zone-bicep)
    - [Azure landing zones - Terraform modules design considerations](/azure/architecture/landing-zones/terraform/landing-zone-terraform)

To get started with Azure landing zones to create consistent, repeatable environment configuration, see the article series on [Azure landing zones](/azure/cloud-adoption-framework/ready/landing-zone/).

## Deploy infrastructure with code

To fully realize deployment optimization, reduce configuration effort, and automate full environments' deployment, something more is required. One option is referred to as infrastructure as code.

Infrastructure as code (IaC) is the management of infrastructure&mdash;such as virtual machines, load balancers, and connection topology&mdash;in a descriptive model, using a versioning system that's similar to what's used for source code. When you're creating an application, the same source code generates the same binary every time it's compiled. In a similar manner, an IaC model generates the same environment every time it's applied. IaC is a key DevOps practice, and it's often used with continuous delivery.

Ultimately, IaC lets you and your team develop and release changes faster, but with higher confidence in your deployments.

### Gain higher confidence

One of the biggest benefits of IaC is the level of confidence you can have in your deployments, and in your understanding of the infrastructure and its configuration.

**Integrate with your process.** If you have a process by which code changes are peer reviewed, you can use that same process for reviewing infrastructure changes. This review process can help proactively detect problematic configurations that are difficult to see when making manual infrastructure changes.

**Consistency.** Following an IaC process ensures that the whole team follows a standard, well-established process. Historically, some organizations designate a single or small set of individuals responsible for deploying and configuring infrastructure. By following a fully automated process, responsibility for infrastructure deployments moves from individuals into the automation process and tooling. This move broadens the number of team members who can initiate infrastructure deployments while maintaining consistency and quality.

**Automated scanning.** Many types of IaC configurations can be scanned by automated tooling. One such type of tooling is linting to check for errors in the code. Another type will scan the proposed changes to your Azure infrastructure to ensure they follow security and performance best practices. Automated scanning can be an important part of a [Continuous security](/training/modules/explore-devops-continuous-security-operations/2-explore-continuous-security) approach.

**Secret management.** Most solutions require secrets to be maintained and managed. These include connection strings, API keys, client secrets, and certificates. Following an IaC approach means that you need to adopt best practices for managing secrets. For example, Azure Key Vault is used to store secrets securely. It integrates with many IaC tools and configurations to ensure that the person conducting the deployment doesn't need access to production secrets. This approach, in turn, helps you adhere to the security principle of least privilege.

**Access control.** A fully automated IaC deployment pipeline means that an automated process should perform all changes to your infrastructure resources. This approach has many security benefits. For more information, see [Security design principles](/azure/architecture/security/security-principles#embrace-automation). By automating your deployments, you can be confident that changes deployed to your environment have followed the correct procedure. You can even consider expanding the number of people who can initiate a deployment since the deployment itself is done in a fully automated way. Ideally, you would remove the ability for humans to manually modify your cloud resources and instead rely completely on automated processes. In emergencies, you can allow for this ability to be overridden, by using a [break glass account](/azure/active-directory/roles/security-emergency-access) or [Privileged identity management](/azure/active-directory/privileged-identity-management/).

**Avoid configuration drift.** When you use IaC, you can redeploy all of your environment templates on every release of your solution. IaC tooling is built to be idempotent, which means that it can run several times and will produce the same result each time.

Running infrastructure as code operations frequently has the following benefits:

* Avoids deployment staleness, which prevents unforeseen issues during a redeployment. For example, as part of a disaster recovery plan.
* Reduces complexity overall as there's one process that's rehearsed often.
* Helps to avoid configuration drift. Accidental changes outside of the regular pipeline are corrected quickly and the source of truth for your environment's configuration remains in code.

### Manage multiple environments

Many organizations maintain multiple environments, for example, test, staging, and production. In some cases, multiple production environments are maintained for multi-tenanted solutions or geographically distributed applications. Ensuring consistency across these environments can be difficult; using infrastructure as code solutions can help.

**Manage non-production environments.** A common pain point for organizations is when nonproduction environments are dissimilar to production environments. Often, when you build production and nonproduction environments by hand, the configuration of each won't match. This mismatch slows down the testing of changes and reduces confidence that changes won't harm a production system. When following an IaC approach, this problem is minimized. When you use IaC automation, the same infrastructure configuration files can be used for all environments, producing almost identical environments. When needed, differentiation can be achieved by using input parameters for each environment.

**Dynamically provision environments.** Once you have your IaC configurations defined, you can use them to provision new environments more efficiently. This agility can be enormously helpful when you're testing your solution. For example, you could quickly provision a duplicate of your production environment that can then be used for security penetration tests, load testing or help a developer track down a bug.

**Scale production environments.** IaC configurations can be used to deploy more instances of your solution, ensuring consistency between all environments. This model can be useful if you're following certain deployment patterns or if you need to extend your services to a new geographic region. An example is the [Deployment stamps pattern](/azure/architecture/patterns/deployment-stamp).

**Disaster recovery.** In some situations, where recovery time might not be time-sensitive, IaC configurations can be used as part of a disaster recovery plan. For example, if infrastructure needs to be recreated in a second region, your IaC configurations can be used to do so. You need to consider deployment time and restoring the state of your infrastructure and the infrastructure itself.

When you plan for disaster and recovery, ensure that your disaster recovery plans are fully tested and that they meet your [Business metrics](/azure/architecture/framework/resiliency/business-metrics).

### Better understand your cloud resources

IaC can also help you better understand the state of your cloud resources.

**Audit changes.** Changes to your IaC configurations will be version-controlled in the same way as your code, such as through Git's version history. Version control means you can review each change that has happened, and understand who made it and when. This approach can be helpful if you're trying to understand why a resource is configured a specific way.

**Metadata.** Many types of IaC configurations let you add metadata, like code comments, to help explain why something is done a particular way. If your organization has a culture of documenting your code, apply the same principles to your infrastructure code.

**Keep everything together.** It's common for a developer to work on features that require both code and infrastructure changes. By keeping infrastructure defined as code, you can group application and infrastructure code to understand the relationship between them better. For example, if you see a change to an IaC configuration on a feature branch or in a pull request, you'll have a clearer understanding of what that change relates to.

**Better understand Azure itself.** The Azure portal is a great way to provision and configure resources; but it often simplifies the underlying resource model used. Using IaC means that you gain a deeper understanding of what's happening in Azure and how to troubleshoot it if something isn't working correctly. For example, when creating a set of virtual machines in the Azure portal, some of the underlying resource creation is abstracted for the deployment process. When you use IaC, not only do you have explicit control over resource creation, little is abstracted from the process, which provides a richer understanding of what's deployed and how it's configured.

## Categories of IaC tooling

You can use many declarative infrastructure deployment technologies with Azure. Deployment technologies fall into two main categories.

* **Imperative IaC** involves writing scripts in a language like Bash, PowerShell, C# script files, or Python. These scripts programmatically run a series of steps to create or modify your resources. When you use imperative deployments, it's up to you to manage things like dependency sequencing, error control, and resource updates.
* **Declarative IaC** involves writing a definition of how you want your environment to look; the tooling then figures out how to make this happen by inspecting your current state, comparing it to the target state you've requested, and applying the differences. For more information, see [Use automation to redue effort and error](/training/modules/azure-well-architected-operational-excellence/4-use-automation-to-reduce-effort-and-error).

There are great Azure tooling options for both models. Here we describe three of the commonly used declarative IaC technologies for Azure: ARM templates, Bicep, and Terraform.

## Automate deployments with ARM Templates

Azure Resource Manager (ARM) Templates provide an Azure native infrastructure as code solution. ARM Templates are written in a language derived from JavaScript Object Notation (JSON), and they define the infrastructure and configurations for Azure deployments. An ARM template is declarative. You state what you want to deploy, provide configuration values, and the Azure engine takes care of making the necessary Azure REST API put requests. Other benefits you gain by using ARM templates for infrastructure deployments include:

- **Parallel resource deployment:** The Azure deployment engine sequences resource deployments based on defined dependencies. If dependencies don't exist between two resources, they're deployed at the same time.
- **Modular deployments:** ARM templates can be broken up into multiple template files for reusability and modularization.
- **Day one resource support:** ARM templates support all Azure resources and resource properties as they're released.
- **Extensibility:** Azure deployments can be extended by using deployment scripts and other automation solutions.
- **Validation:** Azure deployments are evaluated against a validation API to catch configuration mistakes.
- **Testing:** The [ARM template test toolkit](/azure/azure-resource-manager/templates/test-toolkit) provides a static code analysis framework for testing ARM templates.
- **Change preview:** [ARM template what-if](/azure/azure-resource-manager/templates/template-deploy-what-if?tabs=azure-powershell) lets you see what will be changed before deploying an ARM template.
- **Tooling:** Language service extensions are available for both [Visual Studio Code](/azure/azure-resource-manager/templates/quickstart-create-templates-use-visual-studio-code) and [Visual Studio](/azure/azure-resource-manager/templates/create-visual-studio-deployment-project) to help you author ARM templates.

The following example demonstrates a simple ARM template that deploys a single Azure Storage account. In this example, a single parameter is defined to take in a name for the storage account. Under the resources section, a storage account is defined, the *storageName* parameter is used to provide a name, and the storage account details are defined. See the included documentation for an in-depth explanation of the different sections and configurations for ARM templates.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string",
            "defaultValue": "newStorageAccount"
        }
    },
    "resources": [
        {
            "name": "[parameters('storageName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "location": "[resourceGroup().location]",
            "kind": "StorageV2",
            "sku": {
                "name": "Premium_LRS",
                "tier": "Premium"
            }
        }
    ]
}
```

**Learn more**

- [What are ARM templates?](/azure/azure-resource-manager/templates/overview)
- [Deploy consistent infrastructure with ARM Templates](/training/modules/create-azure-resource-manager-template-vs-code/)
- [ARM templates code samples](/samples/mspnp/samples/azure-well-architected-framework-sample-arm-template/)

## Automate deployments with Bicep

Bicep is a domain-specific language (DSL) that uses declarative syntax to deploy Azure resources. Bicep provides concise syntax, reliable type safety, and support for code reuse.

You can think of Bicep as a revision to the Azure Resource Manager template (ARM template) language rather than a new language. The syntax is different, but the core functionality and runtime remain the same.

The following example shows a simple Bicep file that defines a storage account.

```bicep
param location string = resourceGroup().location
param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'

resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

**Learn more**

- [Documentation: What is Bicep?](/azure/azure-resource-manager/bicep/overview)
- [Documentation: Frequently asked questions](/azure/azure-resource-manager/bicep/frequently-asked-questions)
- [Documentation: Key Bicep concepts](/azure/azure-resource-manager/bicep/file)


## Automate deployments with Terraform

Terraform is a declarative framework for deploying and configuring infrastructure that supports many private and public clouds, Azure being one of them. It has the main advantage of offering a cloud-agnostic framework. While Terraform configurations are specific to each cloud, the framework itself is the same for all of them. Terraform configurations are written in a domain-specific language (DSL) called Hashicorp Configuration Language.

The following example demonstrates a simple Terraform configuration that deploys an Azure resource group and a single Azure Storage account.

```hcl
resource "azurerm_resource_group" "example" {
  name     = "newStorageAccount"
  location = "eastus"
}

resource "azurerm_storage_account" "example" {
  name                     = "storageaccountname"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "GRS"
}
```

Take note, the Terraform provider for Azure is an abstraction on top of Azure APIs. This abstraction is beneficial because the API complexities are obfuscated. This abstraction comes at a cost; the Terraform provider for Azure doesn't always provide parity with the Azure APIs' capabilities. To learn more about using Terraform on Azure, see [Using Terraform on Azure](/azure/developer/terraform/overview)

## Automate infrastructure deployment with Azure Deployment Environments

For organizations that support multiple development teams working in multiple environments, it is essential to have repeatable, consistent, and secure environments. Development infrastructure administrators (dev infra admins) are responsible for managing and controlling access to the resources that developers need. Developers typically work on multiple projects, and might need several different development environments. Developers are usually organized in development teams, led by a development team lead.

Dev infra admin teams must be able to provide developers with environments that include the preconfigured resources needed for different environment types, like development, test, and production. Ideally, developers should be able to create a new instance of an environment appropriate for their project quickly and easily, rather than having to wait for the dev infra team.

By using project level templates, organizations can create environment templates that define the resources and configuration required for each project. Controlling access to each project enables the organization to set an identity security perimeter, so only approved admins can manage the project, and only approved developers can use it.

Creating environments by using templates helps organizations to avoid the pitfalls of manually creating each environment. Creating environments manually is time-consuming, often results in inconsistencies, and presents difficulties in ensuring compliance. When evaluating the use of templates, organizations should consider the overhead of creating new templates, maintaining existing templates, and retiring unused templates.

Azure Deployment Environments enables development teams to deploy application infrastructure with project-based templates. Dev infra admins can deploy preconfigured resources defined by an IaC template. Environments can be platform as a service (PaaS) or infrastructure as a service (IaaS) environments based on the content of the template.

Dev infra admins can automate development infrastructure deployment by using Azure Deployment Environments. It automates the process of applying policies, permissions, and settings on environments, controlling the resource configuration that developers can create, and centrally tracking environments across projects by:

- Providing a project-based, curated set of reusable IaC templates.
- Defining specific Azure deployment configurations per project and per environment type.
- Providing self-service experience without giving control over subscriptions.
- Tracking costs and ensuring compliance with enterprise governance policies.

**Learn more**

- [Azure Deployment Environments](/azure/deployment-environments/)

## Manual deployment

Manual deployment steps introduce significant risks regarding human error and also increase overall deployment times. However, in some cases, manual steps might be required. For these cases, ensure that any manual steps are documented, including roles and responsibilities.

## Hotfix process

In some cases, you might have an unplanned deployment need. For instance, to deploy critical hotfixes or security remediation patches. A defined process for unplanned deployments can help prevent service availability and other deployment issues during these critical events.

## Next steps

> [!div class="nextstepaction"]
> [Automate infrastructure configuration](./automation-configuration.md)
