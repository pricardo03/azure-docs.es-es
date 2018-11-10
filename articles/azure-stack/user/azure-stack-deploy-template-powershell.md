---
title: Implementación de plantillas mediante PowerShell en Azure Stack | Microsoft Docs
description: Implementación de una plantilla en Azure Stack mediante PowerShell.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: cf39ba6b1754a5e809e7295f2783846e95636ce8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241743"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Implementación de una plantilla en Azure Stack mediante PowerShell

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede usar PowerShell para implementar plantillas de Azure Resource Manager en Azure Stack. En este artículo, se describe cómo usar PowerShell para implementar una plantilla.

## <a name="run-azurerm-powershell-cmdlets"></a>Ejecución de cmdlets de AzureRM PowerShell

En este ejemplo, se utilizan cmdlets de AzureRM PowerShell y una plantilla almacenada en GitHub. La plantilla crea una máquina virtual del centro de datos para Windows Server 2012 R2.

>[!NOTE]
>Antes de probar este ejemplo, asegúrese de que se haya [configurado PowerShell](azure-stack-powershell-configure-user.md) para un usuario de Azure Stack.

1. Vaya a [http://aka.ms/AzureStackGitHub](https://aka.ms/AzureStackGitHub) y busque la plantilla **101-simple-windows-vm**. Guarde la plantilla en esta ubicación: C:\\templates\\azuredeploy-101-simple-windows-vm.json.
2. Abra un símbolo del sistema de PowerShell con privilegios elevados.
3. En el siguiente script, sustituya *username* (nombre de usuario) y *password* (contraseña) por su nombre de usuario y contraseña, y luego ejecute el script.

   ```PowerShell
   # Set deployment variables
   $myNum = "001" #Modify this per deployment
   $RGName = "myRG$myNum"
   $myLocation = "local"
   
   # Create resource group for template deployment
   New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
   # Deploy simple IaaS template
   New-AzureRmResourceGroupDeployment `
       -Name myDeployment$myNum `
       -ResourceGroupName $RGName `
       -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
       -NewStorageAccountName mystorage$myNum `
       -DnsNameForPublicIP mydns$myNum `
       -AdminUsername <username> `
       -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
       -VmName myVM$myNum `
       -WindowsOSVersion 2012-R2-Datacenter
   ```

   >[!IMPORTANT]
   >Incremente el valor del parámetro `$myNum` cada vez que ejecute este script para evitar sobrescribir la implementación.

4. Abra el portal de Azure Stack, seleccione **Examinar**, luego seleccione **Máquinas virtuales** y busque su nueva máquina virtual (**myDeployment001**).

## <a name="next-steps"></a>Pasos siguientes

[Implementación de plantillas con Visual Studio](azure-stack-deploy-template-visual-studio.md)
