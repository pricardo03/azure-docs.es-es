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
ms.openlocfilehash: daf960bec0fac5f0c96749c219304bc77a4ba905
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358556"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Referencia de cmdlets de PowerShell para Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se va a retirar. Para programar trabajos, [pruebe Azure Logic Apps en su lugar](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para generar scripts para crear y administrar trabajos de Scheduler y colecciones de trabajos, puede usar cmdlets de PowerShell. En este artículo se enumeran los principales [cmdlets de PowerShell para Azure Scheduler](/powershell/module/azurerm.scheduler) con vínculos a los artículos de referencia. Para instalar Azure PowerShell para su suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview). Para más información sobre los [cmdlets de Azure PowerShell](/powershell/azure/overview), consulte [Uso de Azure PowerShell con Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | DESCRIPCIÓN |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azschedulerjobcollection) |Deshabilita una colección de trabajos. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azschedulerjobcollection) |Habilita una colección de trabajos. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azschedulerjob) |Obtiene trabajos de Scheduler. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azschedulerjobcollection) |Obtiene colecciones de trabajos. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azschedulerjobhistory) |Obtiene el historial de trabajos. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azschedulerhttpjob) |Crea un trabajo HTTP. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azschedulerjobcollection) |Crea una colección de trabajos. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azschedulerservicebusqueuejob) | Crea un trabajo de cola de Service Bus. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azschedulerservicebustopicjob) |Crea un trabajo de tema de Service Bus. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azschedulerstoragequeuejob) |Crea un trabajo de cola de Storage. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azschedulerjob) |Elimina un trabajo de Scheduler. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azschedulerjobcollection) |Quita una colección de trabajos. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azschedulerhttpjob) |Modifica un trabajo HTTP de Scheduler. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azschedulerjobcollection) |Modifica una colección de trabajos. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azschedulerservicebusqueuejob) |Modifica un trabajo de cola de Service Bus. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azschedulerservicebustopicjob) |Modifica un trabajo de tema de Service Bus. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azschedulerstoragequeuejob) |Modifica un trabajo de cola de Storage. |
||| 

Para obtener más detalles, puede ejecutar cualquiera de estos cmdlets: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Vea también

* [¿Qué es Azure Scheduler?](scheduler-intro.md)
* [Conceptos, terminología y jerarquía de entidades](scheduler-concepts-terms.md)
* [Crear y programar su primer trabajo - Azure portal](scheduler-get-started-portal.md)
* [Referencia de API de REST del programador de Azure](https://msdn.microsoft.com/library/mt629143)
