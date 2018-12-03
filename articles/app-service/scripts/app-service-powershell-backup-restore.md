---
title: 'Ejemplo de script de Azure PowerShell: Restauración de una aplicación web desde una copia de seguridad | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: Restauración de una aplicación web desde una copia de seguridad'
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6e8d950db6facacd9ae84566c6e85a14657050fe
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291609"
---
# <a name="restore-a-web-app-from-a-backup"></a>Restauración de una aplicación web desde una copia de seguridad

Este script de ejemplo recupera una copia de seguridad completada previamente desde una aplicación web existente y la restaura sobrescribiendo su contenido. 

Si es necesario, instale Azure PowerShell con la instrucción que se encuentra en la [Guía de Azure PowerShell](/powershell/azure/overview) y, luego, ejecute `Connect-AzureRmAccount` para crear una conexión con Azure. 

## <a name="sample-script"></a>Script de ejemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore/backup-restore.ps1?highlight=1-2 "Restore a web app from a backup")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Cuando ya no necesite la aplicación web, use el comando siguiente para quitar el grupo de recursos, la aplicación web y todos los recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Obtiene una lista de copias de seguridad de una aplicación web. |
| [Restore-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Restaura una aplicación web desde una copia de seguridad completada previamente. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Puede encontrar ejemplos de Azure PowerShell para Azure App Service Web Apps en los [ejemplos de PowerShell](../app-service-powershell-samples.md).
