---
title: Actualización de módulos de Azure en Azure Automation
description: En este artículo se describe cómo puede actualizar ahora módulos comunes de Azure PowerShell proporcionados de forma predeterminada en Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d8f57310cf4dbc2a27761fc44cfde6c8fd2791a2
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005546"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Actualización de módulos de Azure PowerShell en Azure Automation

Para actualizar los módulos de Azure en la cuenta de Automation se recomienda que use el [runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), de código abierto. Además, todavía puede usar el botón **Actualizar módulos de Azure** en el portal para actualizarlos. Para más información sobre el uso del runbook de código abierto, consulte [Actualización de módulos de Azure con el runbook de código abierto](#open-source).

Los módulos de Azure PowerShell más comunes se proporcionan de forma predeterminada en cada cuenta de Automation. El equipo de Azure actualiza periódicamente los módulos de Azure. En su cuenta de Automation, se le proporciona una manera de actualizar los módulos de la cuenta cuando hay nuevas versiones disponibles desde el portal.

Dado que los módulos se actualizan periódicamente por el grupo de productos, pueden producirse cambios con los cmdlets incluidos. Esta acción puede afectar negativamente los runbooks según el tipo de cambio, como cambiar el nombre de un parámetro o dejar de usar un cmdlet por completo.

Para evitar afectar los runbooks y los procesos que automatizan, pruébelos y valídelos antes de continuar. Si no tiene una cuenta de Automation dedicada pensada para este propósito, considere la posibilidad de crear una para que pueda probar muchos escenarios diferentes durante el desarrollo de los runbooks. Estas pruebas deben incluir cambios iterativos, como la actualización de los módulos de PowerShell. 

Si desarrolla sus scripts de forma local, se recomienda tener localmente las mismas versiones del módulo que tiene en la cuenta de Automation al realizar pruebas, a fin de asegurarse de que obtendrá los mismos resultados. Una vez validados los resultados y aplicados los cambios necesarios, puede mover los cambios a producción.

> [!NOTE]
> Es posible que una cuenta de Automation nueva no contenga los módulos más recientes.

## <a name="open-source"></a>Actualización de módulos de Azure con el runbook de código abierto

Para empezar a usar el runbook **Update-AutomationAzureModulesForAccount** para actualizar los módulos de Azure, descárguelo desde el [repositorio del runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) en GitHub. A continuación, puede importarlo en su cuenta de Automation o ejecutarlo como un script. Las instrucciones sobre cómo hacer esto se encuentran en el [repositorio del runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update).

### <a name="considerations"></a>Consideraciones

Éstas son algunas consideraciones a tener en cuenta al usar este proceso para actualizar los módulos de Azure:

* Si importa este runbook con el nombre original `Update-AutomationAzureModulesForAccount`, reemplazará el runbook interno por este nombre. Como resultado, el runbook importado se ejecutará cuando se presione el botón **Actualizar módulos de Azure** o cuando se invoque este runbook directamente a través de la API de Azure Resource Manager para esta cuenta de Automation.

* Actualmente solo se admiten los módulos `Azure` y `AzureRM.*`. Los nuevos [módulos Az de Azure PowerShell](/powershell/azure/new-azureps-module-az) aún no se admiten.

* Evite iniciar este runbook en cuentas de Automation que contengan módulos Az.

* Antes de iniciar este runbook, asegúrese de que la cuenta de Automation tiene una [credencial de cuenta de ejecución de Azure](manage-runas-account.md) creada.

* Puede utilizar este código como un script normal de PowerShell en lugar de un runbook: simplemente inicie sesión en Azure mediante el comando [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) y, después, pase `-Login $false` al script.

* Para usar este runbook en las nubes soberanas, use el parámetro `AzureRmEnvironment` para pasar el entorno correcto al runbook.  Los valores permitidos son **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** y **AzureUSGovernment**. Estos valores se pueden obtener si se usa `Get-AzureRmEnvironment | select Name`. Si no pasa un valor a este parámetro, el runbook de manera predeterminada será la nube pública de Azure **AzureCloud**.

* Si desea utilizar una versión específica del módulo Azure PowerShell en lugar de la última disponible en la Galería de PowerShell, pase estas versiones al parámetro opcional `ModuleVersionOverrides` del runbook **Update-AutomationAzureModulesForAccount**. Para obtener ejemplos, consulte el runbook [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
). Los módulos de Azure PowerShell que no se mencionan en el parámetro `ModuleVersionOverrides` se actualizan con las últimas versiones de los módulos en la Galería de PowerShell. Si no se pasa nada al parámetro `ModuleVersionOverrides`, todos los módulos se actualizan con las versiones de módulo más recientes de la Galería de PowerShell. Se trata del mismo comportamiento que tiene el botón **Actualizar módulos de Azure**.

## <a name="update-azure-modules-in-the-azure-portal"></a>Actualización de módulos de Azure en Azure Portal

1. En la página Módulos de la cuenta de Automation existe una opción denominada **Actualizar módulos de Azure**. Siempre está habilitada.<br><br> ![Opción Actualizar módulos de Azure de la página Módulos](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Antes de actualizar los módulos de Azure, se recomienda actualizarlos en una cuenta de prueba de Automation para asegurarse de que los scripts existentes funcionan como se espera antes de actualizar los módulos de Azure.
  >
  > El botón **Actualizar módulos de Azure** solo está disponible en la nube pública. No está disponible en las [regiones soberanas](https://azure.microsoft.com/global-infrastructure/). Use el runbook **Update-AutomationAzureModulesForAccount** para actualizar los módulos de Azure. Puede descargarlo desde el [repositorio del runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update). Para más información sobre el uso del runbook de código abierto, consulte [Actualización de módulos de Azure con el runbook de código abierto](#open-source).

2. Haga clic en **Actualizar módulos de Azure** y aparecerá una notificación de confirmación que le preguntará si desea continuar.<br><br> ![Notificación de Update Azure Modules (Actualizar módulos de Azure)](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Haga clic en **Sí** y comenzará el proceso de actualización de módulos. El proceso de actualización tarda aproximadamente 15-20 minutos en actualizar los siguientes módulos:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Si los módulos ya están actualizados, el proceso se completará en unos segundos. Cuando el proceso de actualización se complete, recibirá una notificación.<br><br> ![Estado de actualización de Update Azure Modules (Actualizar módulos de Azure)](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    Los módulos AzureRm (AzureRm.*.Core) de .NET Core no son compatibles con Azure Automation y no se pueden importar.

> [!NOTE]
> Azure Automation utiliza los módulos más recientes en su cuenta de Automation al ejecutar un nuevo trabajo programado.  

Si usa cmdlets de estos módulos de Azure PowerShell en sus runbooks, le interesará realizar este proceso de actualización cada mes, por ejemplo, para asegurarse de que dispone de los módulos más recientes. Azure Automation utiliza la conexión `AzureRunAsConnection` para la autenticación cuando se actualizan los módulos. Si la entidad de servicio ha expirado o ya no existe en el nivel de suscripción, se producirá un error en la actualización del módulo.

## <a name="next-steps"></a>Pasos siguientes

Visite el [runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) de código abierto para más información.
