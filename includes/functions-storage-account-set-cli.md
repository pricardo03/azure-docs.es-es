---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 89e2e64910e33d43c107ee88137de718d020d7d2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839160"
---
Abra el archivo local.settings.json y copie el valor de `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Establezca la variable de entorno `AZURE_STORAGE_CONNECTION_STRING` en la cadena de conexión con el siguiente comando de Bash:

```azurecli-interactive
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

La establecer la cadena de conexión en la variable de entorno `AZURE_STORAGE_CONNECTION_STRING`, puede acceder a la cuenta de almacenamiento sin tener que autenticarse cada vez.