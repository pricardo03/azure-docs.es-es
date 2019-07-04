---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 74e14e36b1ac0979da31203a2d16e2396ed821d0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186089"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

El comando siguiente inicia la aplicación de función. La aplicación se ejecuta con el mismo tiempo de ejecución de Azure Functions que en Azure.

```bash
func host start --build
```

La opción `--build` es necesaria para compilar proyectos de C#. Para los proyectos de JavaScript no lo es.

Cuando se inicia el host de Functions, escribe una salida similar a la siguiente, la que se truncó para mejorar la legibilidad:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Copie la dirección URL de la función `HttpTrigger` que aparece en la salida en entorno de ejecución y péguela en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. A continuación, se muestra la respuesta del explorador para la solicitud GET devuelta por la función local:

![Prueba local en el explorador](./media/functions-run-function-test-local/functions-test-local-browser.png)

Ahora que ejecutó localmente la función, puede crear la aplicación de función y otros recursos necesarios en Azure.