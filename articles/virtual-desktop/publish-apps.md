---
title: 'Publicar aplicaciones integradas en Windows Virtual Desktop: Azure'
description: Cómo publicar aplicaciones modernas en Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 11/25/2019
ms.author: helohr
ms.openlocfilehash: 26ebcc7f2196267719a5f49aeeb8de00d48b9c3e
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483739"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Publicar aplicaciones integradas en Windows Virtual Desktop

En este artículo se explica cómo publicar aplicaciones en el entorno de Windows Virtual Desktop.

## <a name="publish-built-in-apps"></a>Publicación de aplicaciones integradas

Para publicar una aplicación integrada:

1. Conéctese a una de las máquinas virtuales del grupo de hosts.
2. Para obtener el elemento **PackageFamilyName** de la aplicación que quiere publicar, siga las instrucciones de [este artículo](https://docs.microsoft.com/powershell/module/appx/get-appxpackage?view=win10-ps).
3. Por último, ejecute el siguiente cmdlet con `<PackageFamilyName>` reemplazado por el elemento **PackageFamilyName** que encontró en el paso anterior:
   
   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows Virtual Desktop solo admite la publicación de aplicaciones con ubicaciones de instalación que comienzan por `C:\Program Files\Windows Apps`.

## <a name="update-app-icons"></a>Actualizar los iconos de aplicación

Después de publicar una aplicación, tendrá el icono de aplicación de Windows predeterminado, en lugar de su imagen de icono normal. Para cambiar el icono a su icono normal, coloque la imagen del icono que quiera en un recurso compartido de red. Los formatos de imagen admitidos son PNG, BMP, GIF, JPG, JPEG e ICO.

## <a name="publish-microsoft-edge"></a>Publicar Microsoft Edge

El proceso que se usa para publicar Microsoft Edge es un poco diferente del proceso de publicación de otras aplicaciones. Para publicar Microsoft Edge con la página principal predeterminada, ejecute este cmdlet:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" 
```



