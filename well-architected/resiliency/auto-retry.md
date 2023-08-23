---
title: Automatic retry of failed backup jobs
description: Learn how to protect your data assets in Azure by automating the retry of failed backups due to system outages.
author: martinekuan
ms.author: martinek
ms.date: 04/25/2023
ms.topic: conceptual
ms.custom:
  - Automatic retry of failed backup jobs
  - article
  - devx-track-azurepowershell
categories:
  - management-and-governance
---

# Automatic retry of failed backup jobs

Azure Backup is a simple, secure, and cost-effective solution that comprehensively protects your data assets in Azure and requires zero infrastructure. Azure's built-in data protection covers a wide range of workloads, and helps protect your mission-critical workloads running in the cloud. Azure ensures that your backups are always available and managed at scale across your entire backup estate.

As a backup user or administrator, you can monitor all backup solutions and configure alerts to notify you about important events.

Many failures or outages are transient. You can solve them just by retrying the backup, or the restore job. However, waiting for an engineer to retry the job manually or assign the relevant permission wastes valuable time. Automation is the smarter way to retry failed jobs, and ensures that you continue to meet your target Recovery Point Objectives (RPOs) with one successful backup per day.

Retrieve relevant backup data through [Azure Resource Graph (ARG)](/azure/governance/resource-graph/overview) and combine the data with corrective PowerShell and CLI steps. This article explains how you can retry backups for all failed jobs using ARG and PowerShell.

## Prerequisites

You need an [Azure Automation](/azure/automation/automation-security-overview) account. You can use an existing account or [create a new account](/azure/automation/automation-create-standalone-account?tabs=azureportal) with one user-assigned managed identity, at minimum.

## Add modules

After you create the automation account, install the following modules by navigating to the individual module gallery:

- [Az.Accounts](/powershell/module/az.accounts/?view=azps-6.6.0add&preserve-view=true)
- [Az.RecoveryServices](/powershell/module/az.recoveryservices/?view=azps-6.6.0add&preserve-view=true)
- [Az.Resources](/powershell/module/az.resources/)
- [Az.ManagedServiceIdentity](/powershell/module/az.managedserviceidentity)
- [Az.Graph](/cli/azure/graph?view=azure-cli-latestadd&preserve-view=true)

## Assign permissions to managed identities

To assign permissions to managed identities, complete the following steps:

1. Sign in to Azure interactively using the [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-6.6.0add&preserve-view=true) cmdlet and follow the instructions:

   ```azurepowershell
   # Sign in to your Azure subscription 

   $sub = Get-AzSubscription -ErrorAction SilentlyContinue 
   if(-not($sub))
   { 
    Connect-AzAccount 
   } 

   # If you have multiple subscriptions, set the one to use 
   # Select-AzSubscription -SubscriptionId <SUBSCRIPTIONID> 
   ```

1. Provide an appropriate value for the following variables, and then run the script:

   ```azurepowershell
   $resourceGroup = "resourceGroupName" 

   # These values are used in this tutorial 
   $automationAccount = "xAutomationAccount" 
   $userAssignedManagedIdentity = "xUAMI" 
   ```

1. Use the PowerShell cmdlet [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment?view=azps-6.6.0add&preserve-view=true) to assign a role to the *system-assigned* managed identity:

   ```powershell
   $role1 = "DevTest Labs User" 

   $SAMI = (Get-AzAutomationAccount -ResourceGroupName $resourceGroup 
   -Name $automationAccount).Identity.PrincipalId 
   New-AzRoleAssignment 
    -ObjectId $SAMI 
    -ResourceGroupName $resourceGroup
    -RoleDefinitionName $role1 
   ```

1. You need the same role assignment for the *user-assigned* managed identity:

   ```powershell
   $UAMI = (Get-AzUserAssignedIdentity -ResourceGroupName 
   $resourceGroup -Name $userAssignedManagedIdentity).PrincipalId 
   New-AzRoleAssignment
       -ObjectId $UAMI
       -ResourceGroupName $resourceGroup
       -RoleDefinitionName $role1 
   ```

1. You need extra permissions for the *system-assigned* managed identity to run the cmdlets `Get-AzUserAssignedIdentity` and `Get-AzAutomationAccount`:

   ```powershell
   $role2 = "Reader" 
   New-AzRoleAssignment
    -ObjectId $SAMI
    -ResourceGroupName $resourceGroup 
    -RoleDefinitionName $role2 
   ```

## Create a PowerShell runbook

To create a runbook that can be run by managed identities, complete the following steps:

1. Sign in to the [Azure portal](https://ms.portal.azure.com) and navigate to your Automation account.
1. Under **Process Automation** in the side panel, select **Runbooks**.
1. Select **Create a runbook**:
   1. Name the runbook *miTesting*.
   1. From the **Runbook type** dropdown menu, select **PowerShell**.
   1. Select **Create**.
1. In the runbook editor, paste the following code:

   ```powershell
   $connection = Get-AutomationConnection -Name AzureRunAsConnection 

   $connectionResult = Connect-AzAccount 
   -ServicePrincipal
   -Tenant $connection.TenantID
   -ApplicationId $connection.ApplicationID
   -CertificateThumbprint $connection.CertificateThumbprint 
   "Login successful.."

   $query = "RecoveryServicesResources  
   | where type in~ ('microsoft.recoveryservices/vaults/backupjobs') 
   | extend vaultName = case(type =~ 'microsoft.dataprotection/backupVaults/backupJobs', properties.vaultName,type =~ 'Microsoft.RecoveryServices/vaults/backupJobs', split(split(id, '/Microsoft.RecoveryServices/vaults/')[1],'/')[0],'--') 
   | extend friendlyName = case(type =~ 'microsoft.dataprotection/backupVaults/backupJobs', strcat(properties.dataSourceSetName , '/', properties.dataSourceName), type =~ 'Microsoft.RecoveryServices/vaults/backupJobs', properties.entityFriendlyName, '--') 
   | extend dataSourceType = case(type =~ 'Microsoft.RecoveryServices/vaults/backupJobs', properties.backupManagementType, type =~ 'microsoft.dataprotection/backupVaults/backupJobs', properties.dataSourceType, '--') 
   | extend protectedItemName = split(split(properties.backupInstanceId, 'protectedItems')[1],'/')[1] 
   | extend vaultId = tostring(split(id, '/backupJobs')[0]) 
   | extend vaultSub = tostring( split(id, '/')[2]) 
   | extend jobStatus = case (properties.status == 'Completed' or properties.status == 'CompletedWithWarnings','Succeeded',properties.status == 'Failed', 'Failed', properties.status == 'InProgress', 'Started', properties.status), operation = case(type =~ 'microsoft.dataprotection/backupVaults/backupJobs' and tolower(properties.operationCategory) =~ 'backup' and properties.isUserTriggered == 'true', strcat('adhoc',properties.operationCategory), type =~ 'microsoft.dataprotection/backupVaults/backupJobs', tolower(properties.operationCategory), type =~ 'Microsoft.RecoveryServices/vaults/backupJobs' and tolower(properties.operation) =~ 'backup' and properties.isUserTriggered == 'true', strcat('adhoc', properties.operation), type =~ 'Microsoft.RecoveryServices/vaults/backupJobs', tolower(properties.operation), '--'), startTime = todatetime(properties.startTime), endTime = properties.endTime, duration = properties.duration  
   | where startTime >= ago(24h) 
   | where (dataSourceType in~ ('AzureIaasVM')) 
   | where jobStatus=='Failed' 
   | where operation == 'backup' or operation == 'adhocBackup' 
   | project vaultSub, vaultId, protectedItemName, startTime, endTime, jobStatus, operation 
   | sort by vaultSub" 

   $subscriptions = Get-AzSubscription | foreach {$_.SubscriptionId}

   $result = Search-AzGraph -Subscription $subscriptions -Query $query -First 5

   $result = $result.data

 
   $prevsub = "" 
   foreach($jobresponse in $result) 
   { 

                 if($jobresponse.vaultSub -ne $prevsub) 

              { 

                           Set-AzContext -SubscriptionId 
   $jobresponse.vaultSub 

                           $prevsub = $jobresponse.vaultSub 

              } 

              $item = Get-AzRecoveryServicesBackupItem -VaultId 
   $jobresponse.vaultId -BackupManagementType AzureVM -WorkloadType AzureVM -Name 
   $jobresponse.protectedItemName 

 
              Backup-AzRecoveryServicesBackupItem -ExpiryDateTimeUTC 
   (get-date).AddDays(10) -Item $item -VaultId $jobresponse.vaultId 

   }
   ```

1. Select **Save** and then **Test pane**.

You successfully created a PowerShell runbook.

## Create a new schedule with PowerShell

To create a new schedule with PowerShell:

- Use the [New-AzAutomationSchedule](/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-6.6.0add&preserve-view=true) cmdlet to create schedules.
- Specify the start time for the schedule and the frequency it should run.

The following code example shows how to create a recurring schedule that runs every day at *1:00 PM* for one year:

```powershell
PS C:\> $StartTime = Get-Date "13:00:00" 
PS C:\> $EndTime = $StartTime.AddYears(1) 
PS C:\> New-AzAutomationSchedule -AutomationAccountName "MyAutomationAccount" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

The first command creates a date object using the `Get-Date` cmdlet and then stores the object in the `$StartDate` variable. The specified time must be, at least, five minutes in the future.

The second command creates a date object using the `Get-Date` cmdlet and then stores the object in the `$EndDate` variable. The command specifies a future time.

The final command creates a daily schedule named *Schedule02* to begin at the time stored in `$StartDate` and expires at the time stored in `$EndDate`.

## Link a schedule to a runbook

Consider the following concepts when you link a schedule to a runbook:

- You can link a runbook to multiple schedules, and a schedule can have multiple runbooks linked to it.
- If a runbook has parameters, you can provide values for them.
- Provide values for any mandatory parameters and any optional parameters. These values are used each time the runbook is started by this schedule.
- You can attach the same runbook to another schedule and specify different parameter values.

## Link a schedule to a runbook by using PowerShell

To link a schedule to a runbook by using PowerShell:

- Use the [Register-AzAutomationScheduledRunbook](/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-6.6.0add&preserve-view=true) cmdlet to link a schedule.
- You can specify parameter values for the runbook with the [Parameters parameter](/azure/automation/shared-resources/schedules#link-a-schedule-to-a-runbook-with-powershell).

For more information about how to specify parameter values, see [Start a runbook in Azure Automation](/azure/automation/start-runbooks).

The following code example shows how to link a schedule to a runbook by using an Azure Resource Manager cmdlet with parameters:

```powershell
$automationAccountName = "MyAutomationAccount" 
$runbookName = "Test-Runbook" 
$scheduleName = "Sample-DailySchedule" 
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true} 
Register-AzAutomationScheduledRunbook -AutomationAccountName $automationAccountName  
-Name $runbookName -ScheduleName $scheduleName -Parameters $params ` 
-ResourceGroupName "ResourceGroup01" 
```

## Next steps

>[!div class="nextstepaction"]
>[Error handling](./app-design-error-handling.md)
