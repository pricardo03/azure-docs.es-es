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
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 3c3e064ea229db97c59cc5b49107b568a2fdaa98
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334455"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatización de la validación de Azure Stack con PowerShell

La validación como servicio (VaaS) permite automatizar el inicio de las pruebas con el script **LaunchVaaSTests.ps1**.

Puede usar PowerShell para el flujo de trabajo siguiente:

- Prueba superada

En este tutorial, aprenderá a crear un script que:

> [!div class="checklist"]
> * Instala los requisitos previos
> * Instala e inicia el agente local
> * Inicia una categoría de pruebas, como integración, funcional o confiabilidad
> * Notifica los resultados de la prueba

## <a name="launch-the-test-pass-workflow"></a>Inicio del flujo de trabajo Prueba superada

1. Abra un símbolo del sistema de PowerShell con privilegios elevados.

2. Ejecute el script siguiente para descargar el script de automatización:

    ```PowerShell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Ejecute el siguiente script con los valores de parámetros adecuados:

    ```PowerShell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parámetros**

    | Parámetro | DESCRIPCIÓN |
    | --- | --- |
    | VaaSUserld | Identificador de usuario de VaaS. |
    | VaaSUserPassword | Contraseña de VaaS. |
    | VaaSAccountTenantId | El GUID del inquilino de VaaS. |
    | VaaSSolutionName | El nombre de la solución de VaaS en la que se ejecutará la prueba. |
    | VaaSTestPassName | El nombre del flujo de trabajo de ejecución de la prueba de VaaS que se va a crear. |
    | VaaSTestCategories | `Integration`, `Functional` o. `Reliability`. Si usa varios valores, separe cada valor por una coma.  |
    | ServiceAdminUserName | Cuenta de administrador del servicio de Azure Stack.  |
    | ServiceAdminPassword | Contraseña del servicio de Azure Stack.  |
    | TenantAdminUserName | Administrador del inquilino principal.  |
    | TenantAdminPassword | Contraseña del inquilino principal.  |
    | CloudAdminUserName | El nombre de usuario del administrador de la nube.  |
    | CloudAdminPassword | Contraseña del usuario administrador de la nube.  |

4. Revise los resultados de la prueba. Para otras opciones, consulte [Supervisión y administración de pruebas en el portal de VaaS](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre PowerShell en Azure Stack, revise los módulos más recientes.

> [!div class="nextstepaction"]
> [Módulo de Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.5.0)
