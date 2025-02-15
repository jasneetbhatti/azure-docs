---
title: Use HCX Run Commands 
description: Use HCX Run Commands in Azure VMware Solution
ms.topic: how-to
ms.service: azure-vmware
ms.custom: engagement-fy23
ms.date: 04/11/2023
---

# Use HCX Run Commands
In this article, you learn how to use HCX run commands. Use run commands to perform operations that would normally require elevated privileges through a collection of PowerShell cmdlets. This document outlines the available HCX run commands and how to use them. 

This article describes two HCX commands: **Restart HCX Manager** and **Scale HCX Manager**. 

## Restart HCX Manager 

This Command checks for active HCX migrations and replications. If none are found, it restarts the HCX cloud manager (HCX VM's guest OS). 

1. Navigate to the run Command panel in an Azure VMware private cloud on the Azure portal.

   :::image type="content" source="media/hcx-commands/run-command-private-cloud.png" alt-text="Diagram that  lists all available Run command packages and Run commands." border="false" lightbox="media/hcx-commands/run-command-private-cloud.png":::        
   
1. Select the **Microsoft.AVS.Management** package dropdown menu and select the **Restart-HcxManager** command. 
1. Set parameters and select **Run**. 
Optional run command parameters.   

    If the parameters are used incorrectly, they can halt active migrations, and replications and cause other issues. Brief description of each parameter with an example of when it should be used.  
    
    **Hard Reboot Parameter** - Restarts the virtual machine instead of the default of a GuestOS Reboot. This command is like pulling the power plug on a machine. We don't want to risk disk corruption so this should only be used if a normal reboot fails, and we have exhausted all other options.  
    
    **Force Parameter** - If there are ANY active HCX migrations/replications, this parameter avoids the check for active HCX migrations/replications. If the Virtual machine is in a powered off state, this parameter powers the machine on.  

    **Scenario 1**: A customer has a migration that has been stuck in an active state for weeks and they need a restart of HCX for a separate issue. Without this parameter, the script will fail due to the detection of the active migration. 
    **Scenario 2**: The HCX Manager is powered off and the customer would like to power it back on.

    :::image type="content" source="media/hcx-commands/restart-command.png" alt-text="Diagram that shows run command parameters for Restart-HcxManager command." border="false" lightbox="media/hcx-commands/restart-command.png":::   

1. Wait for command to finish. It may take few minutes for the HCX appliance to come online. 

## Scale HCX manager  
Use the Scale HCX manager run command to increase the resource allocation of your HCX Manager virtual machine to 8 vCPUs and 24-GB RAM from the default setting of 4 vCPUs and 12-GB RAM, ensuring scalability. 

**Scenario**: Mobility Optimize Networking (MON) requires HCX  Scalability. For more details on [MON scaling](https://kb.vmware.com/s/article/88401)  

>[!NOTE] 
> HCX cloud manager will be rebooted during this operation, and this may affect any ongoing migration processes. 

1. Navigate to the run Command panel on in an AVS private cloud on the Azure portal. 

1. Select the **Microsoft.AVS.Management** package dropdown menu and select the ``Set-HcxScaledCpuAndMemorySetting`` command.
 
    :::image type="content" source="media/hcx-commands/set-hcx-scale.png" alt-text="Diagram that shows run command parameters for Set-HcxScaledCpuAndMemorySetting command." border="false" lightbox="media/hcx-commands/set-hcx-scale.png"::: 
 
1. Agree to restart HCX by toggling ``AgreeToRestartHCX`` to **True**. 
    You must acknowledge that the virtual machine will be restarted.  
    
     
    >[!NOTE]
    > If this required parameter is set to false that cmdlet execution will fail. 

1. Select **Run** to execute.
    This process may take between 10-15 minutes.  
   
    >[!NOTE]
    > HCX cloud manager will be unavailable during the scaling. 

 ## Next step
To learn more about run commands, see [Run commands](concepts-run-command.md)