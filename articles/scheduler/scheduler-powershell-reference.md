---
title: 'Referencia de cmdlets de PowerShell: Azure Scheduler'
description: Obtenga información sobre los cmdlets de PowerShell para Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: a06439150ac255e7b436082ecc88702bf0c1dec1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991103"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Referencia de cmdlets de PowerShell para Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se va a retirar. Para programar trabajos, [pruebe Azure Logic Apps en su lugar](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Para generar scripts para crear y administrar trabajos de Scheduler y colecciones de trabajos, puede usar cmdlets de PowerShell. En este artículo se enumeran los principales [cmdlets de PowerShell para Azure Scheduler](/powershell/module/azurerm.scheduler) con vínculos a los artículos de referencia. Para instalar Azure PowerShell para su suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview). Para más información sobre los [cmdlets de Azure PowerShell](/powershell/azure/overview), consulte [Uso de Azure PowerShell con Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | DESCRIPCIÓN |
|--------|-------------|
| [Disable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Deshabilita una colección de trabajos. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Habilita una colección de trabajos. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Obtiene trabajos de Scheduler. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Obtiene colecciones de trabajos. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Obtiene el historial de trabajos. |
| [New-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Crea un trabajo HTTP. |
| [New-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Crea una colección de trabajos. |
| [New-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Crea un trabajo de cola de Service Bus. |
| [New-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Crea un trabajo de tema de Service Bus. |
| [New-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Crea un trabajo de cola de Storage. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Elimina un trabajo de Scheduler. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Quita una colección de trabajos. |
| [Set-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Modifica un trabajo HTTP de Scheduler. |
| [Set-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Modifica una colección de trabajos. |
| [Set-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Modifica un trabajo de cola de Service Bus. |
| [Set-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Modifica un trabajo de tema de Service Bus. |
| [Set-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Modifica un trabajo de cola de Storage. |
||| 

Para obtener más detalles, puede ejecutar cualquiera de estos cmdlets: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Otras referencias

* [¿Qué es Azure Scheduler?](scheduler-intro.md)
* [Conceptos, terminología y jerarquía de entidades](scheduler-concepts-terms.md)
* [Crear y programar su primer trabajo: Azure Portal](scheduler-get-started-portal.md)
* [Referencia de API de REST de Azure Scheduler](https://msdn.microsoft.com/library/mt629143)
