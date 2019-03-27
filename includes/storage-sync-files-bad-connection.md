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
ms.openlocfilehash: 359347e41264711a6ac0fa4d2dd0c3633590e917
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488032"
---
Este error se puede producir cuando el servicio Azure File Sync no es accesible desde el servidor. Puede solucionar este error siguiendo estos pasos:

1. Compruebe que el servicio de Windows `FileSyncSvc.exe` no está bloqueado por el firewall.
2. Compruebe que el puerto 443 está abierto para las conexiones salientes al servicio Azure File Sync. Para ello, puede usar el cmdlet `Test-NetConnection`. La dirección URL para el marcador de posición `<azure-file-sync-endpoint>` a continuación se puede encontrar en el documento [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Asegúrese de establecer la configuración del proxy como estaba previsto. Esto puede hacerse con el cmdlet `Get-StorageSyncProxyConfiguration`. Puede encontrar más información sobre cómo configurar la configuración del proxy de Azure File Sync en [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
    
4. Póngase en contacto con el administrador de red si necesita más ayuda para resolver problemas de conectividad de red.