---
title: Conexión a Azure Stack con PowerShell como operador | Microsoft Docs
description: Obtenga información sobre cómo conectarse a Azure Stack con PowerShell como operador
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/17/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: aa86b44364ee84a9640fe0b39b6279982f5594ad
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982532"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Conexión a Azure Stack con PowerShell como operador

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede configurar Azure Stack para usar PowerShell a fin de administrar recursos como la creación de ofertas, planes, cuotas y alertas. Este tema le ayuda a configurar el entorno de operador.

## <a name="prerequisites"></a>Requisitos previos

Implemente los siguientes requisitos previos desde el [kit de desarrollo](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) o desde un cliente externo basado en Windows, si está [conectado a través de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). 

 - Instale los [módulos de Azure PowerShell compatibles con Azure Stack](azure-stack-powershell-install.md).  
 - Descargue las [herramientas necesarias para trabajar con Azure Stack](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configuración del entorno de operador e inicio de sesión en Azure Stack

Configuración del entorno de operador de Azure Stack con PowerShell Ejecute uno de los scripts siguientes: reemplace los valores de tenantName, punto de conexión GraphAudience y ArmEndpoint de Azure AD por su propia configuración de entorno.

````PowerShell  
    # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
    # To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Admin Resource Manager endpoint for your environment>"

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
````

## <a name="test-the-connectivity"></a>Prueba de la conectividad

Ahora que todo está configurado, use PowerShell para crear recursos en Azure Stack. Por ejemplo, puede crear un grupo de recursos para una aplicación y agregar una máquina virtual. Use el comando siguiente para crear un grupo de recursos denominado **MyResourceGroup**.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Pasos siguientes

 - [Desarrollo de plantillas para Azure Stack](user/azure-stack-develop-templates.md)
 - [Implementación de plantillas con PowerShell](user/azure-stack-deploy-template-powershell.md)