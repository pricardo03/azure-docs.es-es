---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: d804cb310a8638713cabf76c2f4192a0e4d0f43d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134397"
---
## <a name="create-a-project-zip-file"></a>Creación de un archivo ZIP de proyecto

Asegúrese de que todavía se encuentra en el directorio raíz del proyecto de ejemplo. Cree un archivo ZIP con todo el contenido del proyecto. El siguiente comando usa la herramienta predeterminada de su terminal:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

Posteriormente, cargue el archivo ZIP en Azure e impleméntelo en App Service.