---
title: Automatización de la validación de Azure Stack con PowerShell | Microsoft Docs
description: Puede automatizar la validación de Azure Stack con PowerShell.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1cb4b1a7cfd72ea302676244a53af58e77215aa9
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234472"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatización de la validación de Azure Stack con PowerShell 

La validación como servicio (VaaS) permite automatizar el inicio de las pruebas con el script **LaunchVaaSTests.ps1**.

Puede usar PowerShell para el flujo de trabajo siguiente:

- Flujo de trabajo de la ejecución de la prueba

Este script abarca los cuatro elementos de un flujo de trabajo:

- Instala los requisitos previos.
- Instala e inicia el agente local.
- Inicia una categoría de pruebas, como integración, funcional o confiabilidad.
- Informa del resultado de cada ejecución de la prueba con fines de supervisión y generación del archivo de informe.

## <a name="launch-the-test-pass-workflow"></a>Inicio del flujo de trabajo de la ejecución de la prueba

1. Abra un símbolo del sistema de PowerShell con privilegios elevados.

2. Ejecute el script siguiente para descargar el script de automatización:

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. Ejecute el siguiente script con sus valores:

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parámetros**

    | Parámetro | DESCRIPCIÓN |
    | --- | --- |
    | VaaSUserld | Identificador de usuario de VaaS. | 
    | VaaSUserPassword | Contraseña de VaaS. |
    | ServiceAdminUser | Cuenta de administrador del servicio de Azure Stack.  |
    | ServiceAdminPassword | Contraseña del servicio de Azure Stack.  |
    | TenantAdminUser | Administrador del inquilino principal.  |
    | TenantAdminPassword | Contraseña del inquilino principal.  |
    | FunctionalCategory| Integration, Functional o. Reliability. Si usa varios valores, separe cada valor por una coma.  |

4. Revise los resultados de la prueba. Para obtener información acerca de cómo leer los resultados de las pruebas, consulte [Supervisión de pruebas](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Pasos siguientes

 - Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
 - Para más información acerca de PowerShell en Azure Stack, consulte la referencia del [Módulo de Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0).