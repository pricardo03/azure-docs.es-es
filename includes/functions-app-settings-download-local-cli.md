---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 801613f4e3fb1e754856d716c6815895ea5da3aa
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839104"
---
Ya ha creado una aplicación de funciones en Azure junto con la cuenta de Storage necesaria. La cadena de conexión de esta cuenta se almacena de forma segura en la configuración de la aplicación en Azure. En este artículo, escribirá mensajes en una cola de almacenamiento de la misma cuenta. Para conectarse a su cuenta de almacenamiento cuando se ejecuta la función localmente, debe descargar la configuración de la aplicación en el archivo local.settings.json. Ejecute el siguiente el comando de Azure Functions Core Tools para descargar la configuración en local.settings.json y reemplace `<APP_NAME>` por el nombre de la aplicación de funciones del artículo anterior:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Es posible que tenga que iniciar sesión en su cuenta de Azure.

> [!IMPORTANT]  
> Como contiene secretos, el archivo local.settings.json nunca se publica y debe excluirse del control de código fuente.

Necesitará el valor `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Esta conexión se usa para comprobar que el enlace de salida funciona según lo previsto.
