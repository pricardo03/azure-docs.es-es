---
title: Actualización de módulos de Azure en Azure Automation
description: En este artículo se describe cómo puede actualizar ahora módulos comunes de Azure PowerShell proporcionados de forma predeterminada en Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fbb57753117f3c60010fe910616b8d0af5178360
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434830"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Actualización de módulos de Azure PowerShell en Azure Automation

Los módulos de Azure PowerShell más comunes se proporcionan de forma predeterminada en cada cuenta de Automation. El equipo de Azure actualiza los módulos de Azure con regularidad, por lo que en la cuenta de Automation se proporciona una manera de actualizar los módulos de la cuenta cuando hay nuevas versiones disponibles en el portal.

Dado que el grupo del producto actualiza los módulos con regularidad, pueden producirse cambios con los cmdlets incluidos, lo que puede afectar negativamente a los runbooks según el tipo de cambio, por ejemplo, al cambiar el nombre de un parámetro o al dejar de usar un cmdlet por completo. Para evitar afectar a los runbooks y a los procesos que automatizan, se recomienda probarlos y validarlos antes de continuar. Si no tiene una cuenta de Automation dedicada destinada para este propósito, considere la posibilidad de crear una para poder probar muchos escenarios y permutaciones diferentes durante el desarrollo de los runbooks, además de los cambios iterativos, como la actualización de los módulos de PowerShell. Una vez validados los resultados y aplicados los cambios necesarios, continúe con la coordinación de la migración de los runbooks que requieran modificación y realice la siguiente actualización tal y como se describe en un entorno de producción.

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

Si usa cmdlets de estos módulos de Azure PowerShell en sus runbooks, le interesará realizar este proceso de actualización cada mes, por ejemplo, para asegurarse de que dispone de los módulos más recientes. Azure Automation usa la conexión AzureRunAsConnection para la autenticación al actualizar los módulos, si la entidad de servicio ha expirado o ya no existe en el nivel de suscripción, se producirá un error en la actualización del módulo.

## <a name="alternative-ways-to-update-your-modules"></a>Formas alternativas de actualizar los módulos

Como se mencionó, el botón **Actualizar módulos de Azure** no está disponible en las nubes soberanas, solo está disponible en la nube de Azure global. Esto se debe a que la última versión de los módulos de Azure PowerShell de la Galería PowerShell pueden no funcionar con los servicios de Resource Manager actualmente implementados en estas nubes.

La actualización de los módulos se puede realizar mediante la importación del runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) a su cuenta de Automation y su ejecución.

Utilice el parámetro `AzureRmEnvironment` para pasar el entorno correcto al runbook.  Los valores permitidos son **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** y **AzureUSGovernmentCloud**. Si no pasa un valor a este parámetro, el runbook de manera predeterminada será la nube pública de Azure **AzureCloud**.

Si desea utilizar una versión específica del módulo Azure PowerShell en lugar de la última disponible en la Galería PowerShell, pase estas versiones al parámetro opcional `ModuleVersionOverrides` del runbook **Update-AzureModule**. Para ver ejemplos, consulte el runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1). Los módulos de Azure PowerShell que no se mencionan en el parámetro `ModuleVersionOverrides` se actualizan con las últimas versiones de los módulos en la Galería de PowerShell. Si no se pasa nada al parámetro `ModuleVersionOverrides`, todos los módulos se actualizan con las últimas versiones de módulos en la Galería de PowerShell, que es el comportamiento del botón **Actualizar módulos de Azure**.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los módulos de integración y cómo crear módulos personalizados para integrar Automation con otros sistemas, servicios o soluciones, consulte [Módulos de integración](automation-integration-modules.md).

* Considere la integración del control de origen con [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) o [Azure DevOps](automation-scenario-source-control-integration-with-vsts.md) para controlar las versiones de la cartera de configuración y el runbook de Automation y administrarlas de forma centralizada.  
