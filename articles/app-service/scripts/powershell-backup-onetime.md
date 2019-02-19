---
title: 'Ejemplo de script de Azure PowerShell: copia de seguridad de una aplicación web | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: copia de seguridad de una aplicación web'
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 13d985b95d971ea28ec5345dbb86df7a3271b716
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56115188"
---
# <a name="back-up-a-web-app-using-powershell"></a>Back up a web app using PowerShell (Copia de seguridad de una aplicación web mediante PowerShell)

Este script de ejemplo crea una aplicación web en App Service con sus recursos relacionados y, después, crea una copia de seguridad única para ella. 

Si es necesario, instale Azure PowerShell con la instrucción que se encuentra en la [Guía de Azure PowerShell](/powershell/azure/overview) y, luego, ejecute `Connect-AzAccount` para crear una conexión con Azure. 

## <a name="sample-script"></a>Script de ejemplo

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-onetime/backup-onetime.ps1?highlight=1-5 "Back up a web app")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos, la aplicación web y todos los recursos relacionados.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Crea una cuenta de Storage. |
| [New-AzStorageContainer](/powershell/module/azure.storage/new-AzStoragecontainer) | Crea un contenedor de Azure Storage. |
| [New-AzStorageContainerSASToken](/powershell/module/azure.storage/new-AzStoragecontainersastoken) | Genera un token de SAS para un contenedor de Azure Storage.  |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Crea un plan de App Service, |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Crea una aplicación web. |
| [New-AzWebAppBackup](/powershell/module/az.websites/new-azwebappbackup) | Crea una copia de seguridad para una aplicación web. |
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Obtiene una lista de copias de seguridad de una aplicación web. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Puede encontrar ejemplos de Azure PowerShell para Azure App Service Web Apps en los [ejemplos de PowerShell](../samples-powershell.md).
