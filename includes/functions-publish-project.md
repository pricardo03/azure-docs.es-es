---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/27/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 1b553cbd720fcb76899844712ce5053af46f7ccb
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452962"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

Una vez creada la aplicación de función en Azure, puede usar el comando [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) para implementar el código del proyecto en Azure.

```bash
func azure functionapp publish <FunctionAppName>
```

Verá una salida parecida a la siguiente, que se ha truncado para mejorar la legibilidad.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

Ahora puede probar las funciones en Azure.