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
ms.openlocfilehash: 1ad1d6b9f59fa51b9e27fe5b70ce4e2a5d36f3ad
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300890"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Referencia de cmdlets de PowerShell para Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para generar scripts para crear y administrar trabajos de Scheduler y colecciones de trabajos, puede usar cmdlets de PowerShell. En este artículo se enumeran los principales cmdlets de PowerShell para Azure Scheduler con vínculos a los artículos de referencia. Para instalar Azure PowerShell para su suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview). Para más información sobre los [cmdlets de Azure PowerShell](/powershell/azure/overview), consulte [Uso de Azure PowerShell con Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | DESCRIPCIÓN |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Deshabilita una colección de trabajos. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Habilita una colección de trabajos. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Obtiene trabajos de Scheduler. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Obtiene colecciones de trabajos. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Obtiene el historial de trabajos. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Crea un trabajo HTTP. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Crea una colección de trabajos. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Crea un trabajo de cola de Service Bus. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Crea un trabajo de tema de Service Bus. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Crea un trabajo de cola de Storage. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Elimina un trabajo de Scheduler. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Quita una colección de trabajos. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Modifica un trabajo HTTP de Scheduler. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Modifica una colección de trabajos. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Modifica un trabajo de cola de Service Bus. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Modifica un trabajo de tema de Service Bus. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Modifica un trabajo de cola de Storage. |
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
