---
title: Creación de una función desencadenada mediante HTTP en Azure
description: Aprenda a crear su primera función de Python en Azure mediante Azure Functions Core Tools y la CLI de Azure.
author: ggailey777
ms.author: glenga
ms.date: 09/11/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: gwallace
ms.openlocfilehash: 03b8e12d63ba84b4e20d7263f1c2ecb8d912936d
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203157"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Creación de una función desencadenada mediante HTTP en Azure

En este artículo se explica cómo usar las herramientas de línea de comandos para crear un proyecto de Python que se ejecuta en Azure Functions. También puede crear una función que desencadena una solicitud HTTP. Por último, publique el proyecto para que se ejecute como una [función sin servidor](functions-scale.md#consumption-plan) en Azure.

Este artículo es el primero de dos artículos de inicio rápido de Python para Azure Functions. Después de completar este inicio rápido, puede [agregar el enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md) a la función.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe hacer lo siguiente:

+ Instale [Python 3.6.x](https://www.python.org/downloads/).

+ Instale [Azure Functions Core Tools](./functions-run-local.md#v2) versión 2.7.1575 u otra posterior.

+ Instale la versión 2.x de la [CLI de Azure](/cli/azure/install-azure-cli) u otra posterior.

+ Tener una suscripción de Azure activa.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment-optional"></a>Creación y activación de un entorno virtual (opcional)

Debe usar un entorno de Python 3.6.x para desarrollar localmente funciones de Python. Ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.venv`.

> [!NOTE]
> Si Python no instaló venv en la distribución de Linux, puede instalarlo con el siguiente comando:
> ```command
> sudo apt-get install python3-venv

### <a name="bash"></a>Bash:

```bash
python -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell o un símbolo del sistema de Windows:

```powershell
py -m venv .venv
.venv\scripts\activate
```

Ahora que ha activado el entorno virtual, ejecute los comandos restantes en él. Para salir del entorno virtual, ejecute `deactivate`.

## <a name="create-a-local-functions-project"></a>Creación de un proyecto local de Functions

Un proyecto de Functions es el equivalente a una aplicación de funciones en Azure. Puede tener varias funciones que comparten la misma configuración local y de host.

1. En el entorno virtual, ejecute el comando siguiente:

    ```console
    func init MyFunctionProj
    ```

1. Seleccione **python** como el tiempo de ejecución de trabajo.

    El comando crea una carpeta _MyFunctionProj_. Contiene estos tres archivos:

    * *local.settings.json*: se usa para almacenar las cadenas de conexión y la configuración de la aplicación cuando la ejecución se realiza a nivel local. Este archivo no se publica en Azure.
    * *requirements. txt*: contiene la lista de paquetes que el sistema instalará en la publicación en Azure.
    * *host.json*: contiene las opciones de configuración global que afectan a todas las funciones de una aplicación de funciones. Este archivo se publica en Azure.

1. Vaya a la nueva carpeta *MyFunctionProj*:

    ```console
    cd MyFunctionProj
    ```

## <a name="create-a-function"></a>Creación de una función

Adición de una función al nuevo proyecto.

1. Para agregar una función a su proyecto, ejecute el siguiente comando:

    ```console
    func new
    ```

1. Use la flecha hacia abajo para seleccionar la plantilla **HTTP trigger**.

1. Cuando se le pida un nombre de función, escriba *HttpTrigger* y, a continuación, presione Entrar.

Estos comandos crean una subcarpeta llamada _HttpTrigger_. Contiene los archivos siguientes:

* *function.json*: archivo de configuración que define la función, el desencadenador y otros enlaces. Tenga en cuenta que, en este archivo, el valor de `scriptFile` apunta al archivo que contiene la función, mientras que la matriz `bindings` define el desencadenador de invocación y los enlaces.

    Cada enlace requiere una dirección, un tipo y un nombre único. El desencadenador HTTP tiene un enlace de entrada de tipo [`httpTrigger`](functions-bindings-http-webhook.md#trigger) y un enlace de salida de tipo [`http`](functions-bindings-http-webhook.md#output).

* *\_\_init\_\_.py*: archivo de script que es su función de desencadenador HTTP. Tenga en cuenta que este script tiene el valor predeterminado `main()`. Los datos HTTP del desencadenador pasan a la función mediante el `req` denominado `binding parameter`. `req`, que se define en function.json, es una instancia de la [clase azure.functions.HttpRequest](/python/api/azure-functions/azure.functions.httprequest). 

    El objeto devuelto, definido como `$return` en *function.json*, es una instancia de la [clase azure.functions.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse). Para más información, vea [Enlaces y desencadenadores HTTP de Azure Functions](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>Ejecución local de la función

La función se ejecuta localmente mediante el entorno de ejecución de Azure Functions.

1. Este comando inicia la aplicación de funciones:

    ```console
    func host start
    ```

    Cuando se inicia el host de Azure Functions, escribe una salida similar a la siguiente. Se trunca aquí para que pueda leerlo mejor:

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

1. Copie la dirección URL de la función `HttpTrigger` que aparece en la salida en entorno de ejecución y péguela en la barra de direcciones del explorador.

1. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. La captura de pantalla siguiente muestra la respuesta a la solicitud GET devuelta que la función local devuelve al explorador:

    ![Comprobación local en el explorador](./media/functions-create-first-function-python/function-test-local-browser.png)

1. Seleccione Ctrl+C para cerrar la aplicación de funciones.

Ahora que ejecutó localmente la función, puede crear la aplicación de función y otros recursos necesarios en Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Creación de una aplicación de función en Azure

Una aplicación de función proporciona un entorno para la ejecución del código de su función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos.

Ejecute el siguiente comando. Reemplace `<APP_NAME>` por un nombre único de aplicación de funciones. Reemplace `<STORAGE_NAME>` por el nombre de la cuenta de almacenamiento. `<APP_NAME>` también es el dominio DNS predeterminado de la aplicación de función. Este nombre debe ser único entre todas las aplicaciones de Azure.

> [!NOTE]
> No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos. Si tiene un grupo de recursos existente llamado `myResourceGroup` con una aplicación web o aplicación de función de Windows, debe usar un grupo de recursos diferente.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

El comando anterior también aprovisiona una instancia asociada de Azure Application Insights en el mismo grupo de recursos. Puede usar esta instancia para supervisar la aplicación de funciones y ver los registros.

Ahora ya está listo para publicar el proyecto de funciones local en la aplicación de función en Azure.

## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

Después de crear la aplicación de funciones en Azure, puede usar el comando [func azure functionapp publish](functions-run-local.md#project-file-deployment) de Core Tools para implementar el código del proyecto en Azure. En este ejemplo, reemplace `<APP_NAME>` por el nombre de la aplicación.

```console
func azure functionapp publish <APP_NAME> --build remote
```

La opción `--build remote` compila el proyecto de Python de forma remota en Azure desde los archivos del paquete de implementación. 

Verá una salida similar al del mensaje siguiente. Se trunca aquí para que pueda leerlo mejor:

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

Puede copiar el valor `Invoke url` para `HttpTrigger` y usarlo para comprobar la función en Azure. La dirección URL contiene un valor de cadena de consulta `code` que es la clave de función, lo que dificulta que otros usuarios llamen al punto de conexión del desencadenador HTTP en Azure.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> Para ver los registros casi en tiempo real de una aplicación de Python publicada, utilice [Live Metrics Stream de Application Insights](functions-monitoring.md#streaming-logs).

## <a name="next-steps"></a>Pasos siguientes

Ha creado un proyecto de funciones de Python con una función desencadenada por HTTP, lo ha ejecutado en el equipo local y lo ha implementado en Azure. Ahora, amplíe la función como sigue...

> [!div class="nextstepaction"]
> [Adición de un enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md)
