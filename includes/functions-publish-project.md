---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: fbb537c9584c948af37694b3bfc77a7c345e084d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639129"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

Una vez creada la aplicación de función en Azure, puede usar el comando [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) de Core Tools para implementar el código del proyecto en Azure. En el comando siguiente, reemplace `<APP_NAME>` por el nombre de la aplicación del paso anterior.

```bash
func azure functionapp publish <APP_NAME>
```

Verá una salida similar a la siguiente, que se truncó para mejorar la legibilidad:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Copie el valor de `Invoke url` para su `HttpTrigger`, que ahora puede usar para probar la función en Azure. La dirección URL contiene un valor de cadena de consulta `code` que es la clave de la función. Esta clave dificulta que otros llamen a su punto de conexión del desencadenador HTTP en Azure.