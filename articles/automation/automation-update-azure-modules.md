---
title: Actualización de módulos de Azure en Azure Automation
description: En este artículo se describe cómo puede actualizar ahora módulos comunes de Azure PowerShell proporcionados de forma predeterminada en Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 11/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9fc605ab45241280d9331ad7d515ba007a015daa
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583660"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Actualización de módulos de Azure PowerShell en Azure Automation

Los módulos de Azure PowerShell más comunes se proporcionan de forma predeterminada en cada cuenta de Automation. El equipo de Azure actualiza periódicamente los módulos de Azure. En su cuenta de Automation, se le proporciona una manera de actualizar los módulos de la cuenta cuando hay nuevas versiones disponibles desde el portal.

Dado que los módulos se actualizan periódicamente por el grupo de productos, pueden producirse cambios con los cmdlets incluidos. Esta acción puede afectar negativamente los runbooks según el tipo de cambio, como cambiar el nombre de un parámetro o dejar de usar un cmdlet por completo. Para evitar afectar los runbooks y los procesos que automatizan, pruébelos y valídelos antes de continuar. Si no tiene una cuenta de Automation dedicada pensada para este propósito, considere la posibilidad de crear una para que pueda probar muchos escenarios diferentes durante el desarrollo de los runbooks. Estas pruebas deben incluir cambios iterativos, como la actualización de los módulos de PowerShell. Una vez validados los resultados y aplicados los cambios necesarios, puede mover los cambios a producción.

> [!NOTE]
> Es posible que una cuenta de Automation nueva no contenga los módulos más recientes.

## <a name="updating-azure-modules"></a>Actualización de módulos de Azure

1. En la página Módulos de la cuenta de Automation existe una opción denominada **Actualizar módulos de Azure**. Siempre está habilitada.<br><br> ![Opción Actualizar módulos de Azure de la página Módulos](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Antes de actualizar los módulos de Azure, se recomienda actualizarlos en una cuenta de prueba de Automation para asegurarse de que los scripts existentes funcionan como se espera antes de actualizar los módulos de Azure.
  >
  > El botón **Actualizar módulos de Azure** solo está disponible en la nube pública. No está disponible en las [regiones soberanas](https://azure.microsoft.com/global-infrastructure/). Consulte la sección [Formas alternativas de actualizar los módulos](#alternative-ways-to-update-your-modules) para más información.


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

## <a name="alternative-ways-to-update-your-modules"></a>Formas alternativas de actualizar los módulos

Como se mencionó, el botón **Actualizar módulos de Azure** no está disponible en las nubes soberanas, sino que solo está disponible en la nube de Azure global. Esto se debe a que la última versión de los módulos de Azure PowerShell de la Galería de PowerShell pueden no funcionar con los recursos de Resource Manager actualmente implementados en estas nubes.

Puede importar y ejecutar el runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) para intentar actualizar los módulos de Azure en su cuenta de Automation. Este proceso puede generar un error si las versiones que intenta importar desde la Galería no son compatibles con los servicios de Azure que están implementados actualmente en el entorno de Azure de destino. Esto puede requerir que se asegure de que se especifican las versiones compatibles de los módulos en los parámetros del runbook.

Utilice el parámetro `AzureRmEnvironment` para pasar el entorno correcto al runbook.  Los valores permitidos son **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** y **AzureUSGovernment**. Estos valores se pueden obtener si se usa `Get-AzureRmEnvironment | select Name`. Si no pasa un valor a este parámetro, el runbook de manera predeterminada será la nube pública de Azure **AzureCloud**.

Si desea utilizar una versión específica del módulo Azure PowerShell en lugar de la última disponible en la Galería PowerShell, pase estas versiones al parámetro opcional `ModuleVersionOverrides` del runbook **Update-AzureModule**. Para ver ejemplos, consulte el runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1). Los módulos de Azure PowerShell que no se mencionan en el parámetro `ModuleVersionOverrides` se actualizan con las últimas versiones de los módulos en la Galería de PowerShell. Si no se pasa nada al parámetro `ModuleVersionOverrides`, todos los módulos se actualizan con las versiones de módulo más recientes de la Galería de PowerShell. Se trata del mismo comportamiento que tiene el botón **Actualizar módulos de Azure**.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los módulos de integración y cómo crear módulos personalizados para integrar Automation con otros sistemas, servicios o soluciones, consulte [Módulos de integración](automation-integration-modules.md).

