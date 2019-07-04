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
ms.openlocfilehash: 48bb91b3b2e9a31de63e515edb857bc2a170ea79
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186100"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

Una vez creada la aplicación de función en Azure, puede usar el comando [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) de Core Tools para implementar el código del proyecto en Azure. En el comando siguiente, reemplace `<APP_NAME>` por el nombre de la aplicación del paso anterior.

```bash
func azure functionapp publish <APP_NAME>
```

Verá resultados similares a los siguientes, que se truncaron para mejorar la legibilidad.

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

Copie el valor de la dirección URL de invocación del desencadenador HttpTrigger, que ahora puede usar para probar su función en Azure. La dirección URL contiene un valor de cadena de consulta `code` que es la clave de la función. Esta clave dificulta que otros llamen a su punto de conexión del desencadenador HTTP en Azure.