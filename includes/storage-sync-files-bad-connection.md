---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 754562487f0fe9f825107445a3059cc15a1faa26
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146592"
---
Este error se puede producir cuando el servicio Azure File Sync no es accesible desde el servidor. Puede solucionar este error siguiendo estos pasos:

1. Compruebe que el servicio de Windows `FileSyncSvc.exe` no está bloqueado por el firewall.
2. Compruebe que el puerto 443 está abierto para las conexiones salientes al servicio Azure File Sync. Para ello, puede usar el cmdlet `Test-NetConnection`. La dirección URL para el marcador de posición `<azure-file-sync-endpoint>` a continuación se puede encontrar en el documento [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```PowerShell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Asegúrese de establecer la configuración del proxy como estaba previsto. Esto puede hacerse con el cmdlet `Get-StorageSyncProxyConfiguration`. Puede encontrar más información sobre cómo configurar la configuración del proxy de Azure File Sync en [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```PowerShell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
    
4. Póngase en contacto con el administrador de red si necesita más ayuda para resolver problemas de conectividad de red.