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
ms.openlocfilehash: 9fefba9c79e9c58956243de8db67ca4cf316251c
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949974"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

Una vez creada la aplicación de función en Azure, puede usar el comando [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) de Core Tools para implementar el código del proyecto en Azure. En estos ejemplos, sustituya `<APP_NAME>` por el nombre de la aplicación del paso anterior.

### <a name="c--javascript"></a>C\# / JavaScript

```command
func azure functionapp publish <APP_NAME>
```

### <a name="typescript"></a>TypeScript

```command
npm run build:production 
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