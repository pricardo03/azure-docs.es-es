---
title: Creación de una función desencadenada mediante HTTP en Azure
description: Aprenda a crear su primera función de Python en Azure mediante Azure Functions Core Tools y la CLI de Azure.
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: gwallace
ms.openlocfilehash: cb7f5a10169c8baaecae0fc1916a439d61bfbf7c
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70170874"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Creación de una función desencadenada mediante HTTP en Azure

En este artículo se explica cómo usar las herramientas de línea de comandos para crear un proyecto de Python que se ejecuta en Azure Functions. La función creada la desencadenan las solicitudes HTTP. Por último, publique el proyecto para que se ejecute como una [función sin servidor](functions-scale.md#consumption-plan) en Azure.

Este artículo es el primero de dos guías de inicio rápido de Azure Functions. Después de completar este artículo, [agregue el enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md) a la función.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, debe hacer lo siguiente:

+ Instale [Python 3.6.x](https://www.python.org/downloads/).

+ Instale [Azure Functions Core Tools](./functions-run-local.md#v2) versión 2.7.1575 u otra posterior.

+ Instale la versión 2.x de la [CLI de Azure](/cli/azure/install-azure-cli) u otra posterior.

+ Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment-optional"></a>Creación y activación de un entorno virtual (opcional)

Para desarrollar y probar las funciones de Python en el entorno local, se recomienda usar un entorno de Python 3.6. Ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.venv`.

### <a name="bash"></a>Bash:

```bash
python3.6 -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell o un símbolo del sistema de Windows:

```powershell
py -3.6 -m venv .venv
.venv\scripts\activate
```

Los comandos restantes se ejecutan dentro del entorno virtual.

## <a name="create-a-local-functions-project"></a>Creación de un proyecto local de Functions

Un proyecto de Functions es el equivalente a una aplicación de función en Azure. Puede tener varias funciones que comparten la misma configuración local y de host.

En el entorno virtual, ejecute el siguiente comando y elija **python** como el rol de trabajo en tiempo de ejecución.

```console
func init MyFunctionProj
```

Se crea una carpeta denominada _MyFunctionProj_, que contiene los tres archivos siguientes:

* `local.settings.json` se usa para almacenar las cadenas de conexión y la configuración de la aplicación cuando la ejecución se realiza a nivel local. Este archivo no se publica en Azure.
* `requirements.txt` contiene la lista de paquetes que se instalarán al publicarlos en Azure.
* `host.json` contiene las opciones de configuración global que afectan a todas las funciones de una aplicación de función. Este archivo se publica en Azure.

Vaya a la nueva carpeta MyFunctionProj:

```console
cd MyFunctionProj
```

## <a name="create-a-function"></a>Creación de una función

Para agregar una función a su proyecto, ejecute el siguiente comando:

```console
func new
```

Elija la plantilla **Desencadenador de HTTP** , escriba `HttpTrigger` como el nombre de la función y, a continuación, presione ENTRAR.

Se crea una subcarpeta denominada _HttpTrigger_, que contiene los archivos siguientes:

* **function.json**: archivo de configuración que define la función, el desencadenador y otros enlaces. Revise el archivo y observe que el valor de `scriptFile` apunta al archivo que contiene la función, mientras que el desencadenador de invocación y los enlaces se definen en la matriz `bindings`.

  Cada enlace requiere una dirección, un tipo y un nombre único. El desencadenador HTTP tiene un enlace de entrada de tipo [`httpTrigger`](functions-bindings-http-webhook.md#trigger) y un enlace de salida de tipo [`http`](functions-bindings-http-webhook.md#output).

* **\_\_init\_\_.py**: archivo de script que es su función de desencadenador HTTP. Revise este script y observe que contiene un valor `main()` predeterminado. Los datos HTTP del desencadenador se pasan a esta función con el uso del valor `req` que se llama parámetro de enlace. `req` es una instancia de la [clase azure.functions.HttpRequest](/python/api/azure-functions/azure.functions.httprequest), que se define en function.json. 

    El objeto devuelto, definido como `$return` en function.json, es una instancia de la [clase azure.functions.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse). Para más información, vea [Enlaces y desencadenadores HTTP de Azure Functions](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>Ejecución local de la función

El comando siguiente inicia la aplicación de función, que se ejecuta localmente mediante el mismo entorno de ejecución de Azure Functions que se encuentra en Azure.

```console
func host start
```

Cuando se inicia el host de Functions, escribe una salida similar a la siguiente, que se truncó para mejorar la legibilidad:

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

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Copie la dirección URL de la función `HttpTrigger` que aparece en la salida en entorno de ejecución y péguela en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. A continuación, se muestra la respuesta del explorador para la solicitud GET devuelta por la función local:

![Prueba local en el explorador](./media/functions-create-first-function-python/function-test-local-browser.png)

Ahora que ejecutó localmente la función, puede crear la aplicación de función y otros recursos necesarios en Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Creación de una aplicación de función en Azure

Una aplicación de función proporciona un entorno para la ejecución del código de su función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos.

Ejecute el siguiente comando mediante un nombre de aplicación de función único en lugar del marcador de posición `<APP_NAME>` y el nombre de la cuenta de almacenamiento para `<STORAGE_NAME>`. `<APP_NAME>` también es el dominio DNS predeterminado de la aplicación de función. Este nombre debe ser único entre todas las aplicaciones de Azure.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```
> [!NOTE]
> No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos. Si tiene un grupo de recursos existente llamado `myResourceGroup` con una aplicación web o aplicación de función de Windows, debe usar un grupo de recursos diferente.

Este comando también aprovisionará una instancia de la instancia de Azure Application Insights asociada en el mismo grupo de recursos que se puede usar para supervisar y ver registros.

Ahora ya está listo para publicar el proyecto de funciones local en la aplicación de función en Azure.

## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

Una vez creada la aplicación de función en Azure, puede usar el comando [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) de Core Tools para implementar el código del proyecto en Azure. En estos ejemplos, sustituya `<APP_NAME>` por el nombre de la aplicación del paso anterior.

```command
func azure functionapp publish <APP_NAME> --build remote
```

La opción `--build remote` compila el proyecto de Python de forma remota en Azure desde los archivos del paquete de implementación. 

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

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> Para ver los registros casi en tiempo real de una aplicación de Python publicada, se recomienda usar [Live Metrics Stream de Application Insights](functions-monitoring.md#streaming-logs)

## <a name="next-steps"></a>Pasos siguientes

Ha creado un proyecto de funciones de Python con una función desencadenada por HTTP, lo ha ejecutado en el equipo local y lo ha implementado en Azure. Ahora, amplíe la función como sigue...

> [!div class="nextstepaction"]
> [Adición de un enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md)
