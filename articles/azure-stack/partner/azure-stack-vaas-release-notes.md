---
title: Notas de la versión de validación como servicio de Azure Stack | Microsoft Docs
description: Notas de la versión de validación como servicio de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 8b0c7bf97592309d68313ef7cc2a919f7aa1c324
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49644971"
---
# <a name="release-notes-for-validation-as-a-service"></a>Notas de la versión para la validación como servicio

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

En este artículo se incluyen las notas de la versión para la validación como servicio de Azure Stack.

## <a name="version-401"></a>Versión 4.0.1

8 de octubre de 2018

- Requisitos previos de VaaS

    `Install-VaaSPrerequisites` ya no requiere credenciales de administrador en la nube. Si ejecuta la versión más reciente de este cmdlet, consulte [Download and install the agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) (Descarga e instalación del agente) para los comandos revisados para instalar los requisitos previos. Estos son los comandos:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

## <a name="version-400"></a>Versión 4.0.0

29 de agosto de 2018

- Requisitos previos de VaaS y actualizaciones de VHD

    `Install-VaaSPrerequisites` ahora requiere credenciales de administrador en la nube para solucionar un problema durante la validación del paquete. La documentación en [Download and install the agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) (Descarga e instalación del agente) se ha actualizado con lo siguiente:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential "<cloudAdminDomain\username>", (ConvertTo-SecureString "<cloudAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region `
                              -CloudAdminCredentials $CloudAdminCreds
    ```
    > [!NOTE]
    > Los valores de `$CloudAdminCreds` que requiere el script corresponden a la instancia de Azure Stack que se está validando. No son las credenciales de Azure Active Directory usadas por el inquilino de VaaS.

- Actualización del agente local

    La versión anterior del agente local no es compatible con la versión actual 4.0.0 del servicio. Todos los usuarios deben actualizar sus agentes locales. Consulte [Download and install the agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) (Descarga e instalación del agente) para obtener instrucciones sobre cómo instalar el agente más reciente.

- Actualización de automatización de PowerShell

    Se realizaron cambios en scripts `LaunchVaaSTests` de PowerShell que requieren la versión más reciente de los paquetes de scripting. Consulte [Inicio del flujo de trabajo de la ejecución de la prueba](azure-stack-vaas-automate-with-powershell.md#launch-the-test-pass-workflow) para obtener instrucciones sobre cómo instalar la versión más reciente del paquete de scripting.

- Portal de validación como servicio

  - Notificaciones de firma de paquetes

    Cuando se envía un paquete de personalización de OEM como parte del flujo de trabajo de validación del paquete, se valida el formato del paquete para asegurarse de que sigue la especificación publicada. Si el paquete no cumple, se producirá un error en la ejecución. Se enviarán notificaciones por correo electrónico a la dirección de correo electrónico del contacto de Azure Active Directory registrada para el inquilino.

  - Categoría de prueba interactiva

    Se ha agregado la categoría de prueba **interactiva**. Estas pruebas permiten a los asociados probar escenarios de Azure Stack interactivos, no automatizados.

  - Comprobación de características interactivas

    La capacidad para proporcionar comentarios enfocados para determinadas características ahora está disponible en el flujo de trabajo de pruebas superadas. La prueba `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` comprueba si actualizaciones específicas se aplicaron correctamente y, a continuación, recopila comentarios.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de la validación como servicio](azure-stack-vaas-troubleshoot.md)