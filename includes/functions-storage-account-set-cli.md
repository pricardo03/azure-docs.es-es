---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 07780c94960c581ce69e4fdd45b346c75ab8b098
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329565"
---
### <a name="set-the-storage-account-connection"></a>Establecimiento de la conexión de la cuenta de almacenamiento

Abra el archivo local.settings.json y copie el valor de `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Establezca la variable de entorno `AZURE_STORAGE_CONNECTION_STRING` en la cadena de conexión con el siguiente comando de Bash:

```azurecli-interactive
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

La establecer la cadena de conexión en la variable de entorno `AZURE_STORAGE_CONNECTION_STRING`, puede acceder a la cuenta de almacenamiento sin tener que autenticarse cada vez.