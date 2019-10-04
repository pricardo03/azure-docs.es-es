---
title: 'Restauración de aplicaciones de App Service eliminadas: Azure App Service'
description: Aprenda a restaurar una aplicación de App Service eliminada con PowerShell.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.service: app-service
ms.openlocfilehash: a766f7ed7a82874df8ef7506226de0d6f38a5847
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219541"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Restauración de aplicaciones de App Service eliminadas con PowerShell

Si por accidente ha eliminado su aplicación de Azure App Service, puede restaurarla mediante los comandos del [módulo de PowerShell Az](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0).

## <a name="list-deleted-apps"></a>Mostrar una lista de aplicaciones eliminadas

Para obtener la colección de aplicaciones eliminadas, puede usar `Get-AzDeletedWebApp`.

Para más información sobre una aplicación eliminada específica, puede usar:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app>
```

La información detallada incluye:

- **DeletedSiteId**: identificador único de la aplicación, que se usa con escenarios en los que se han eliminado varias aplicaciones con el mismo nombre.
- **SubscriptionID**: suscripción que contiene el recurso eliminado.
- **Ubicación**: ubicación de la aplicación original.
- **ResourceGroupName**: nombre del grupo de recursos original.
- **Nombre**: nombre de la aplicación original.
- **Ranura**: el nombre de la ranura.
- **Hora de eliminación**: cuándo se eliminó la aplicación.  

## <a name="restore-deleted-app"></a>Restauración de la aplicación eliminada

Una vez identificada la aplicación que quiere restaurar, puede hacerlo mediante `Restore-AzDeletedWebApp`.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```

Las entradas del comando son:

- **Grupo de recursos**: grupo de recursos de destino donde se restaurará la aplicación.
- **Nombre**: nombre de la aplicación, debe ser globalmente único.
- **TargetAppServicePlanName**: plan de App Service vinculado a la aplicación.

De forma predeterminada `Restore-AzDeletedWebApp` restaurará también la configuración de la aplicación y el contenido. Si solo quiere restaurar el contenido, use la marca `-RestoreContentOnly` con este cmdlet.
