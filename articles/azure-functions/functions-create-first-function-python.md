---
title: Creación de una función desencadenada mediante HTTP en Azure
description: Aprenda a crear su primera función de Python en Azure mediante Azure Functions Core Tools y la CLI de Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 88e8d543e8bc3c3ae07133ec333b33c4486c2684
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864510"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Creación de una función desencadenada mediante HTTP en Azure

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

En este artículo se explica cómo usar las herramientas de línea de comandos para crear un proyecto de Python que se ejecuta en Azure Functions. La función creada la desencadenan las solicitudes HTTP. Por último, publique el proyecto para que se ejecute como una [función sin servidor](functions-scale.md#consumption-plan) en Azure.

Este artículo es el primero de dos guías de inicio rápido de Azure Functions. Después de completar este artículo, [agregue el enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md) a la función.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, debe hacer lo siguiente:

+ Instale [Python 3.6](https://www.python.org/downloads/).

+ Instale [Azure Functions Core Tools](./functions-run-local.md#v2) versión 2.6.666 u otra posterior.

+ Instale la versión 2.x de la [CLI de Azure](/cli/azure/install-azure-cli) u otra posterior.

+ Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Creación y activación de un entorno virtual

Para desarrollar y probar las funciones de Python a nivel local, debe trabajar en un entorno de Python 3.6. Ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.env`.

### <a name="bash-or-a-terminal-window"></a>Bash o una ventana de terminal:

```bash
python3.6 -m venv .env
source .env/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell o un símbolo del sistema de Windows:

```powershell
py -3.6 -m venv .env
.env\scripts\activate
```

Los comandos restantes se ejecutan dentro del entorno virtual.

## <a name="create-a-local-functions-project"></a>Creación de un proyecto local de Functions

Un proyecto de Functions es el equivalente a una aplicación de función en Azure. Puede tener varias funciones que comparten la misma configuración local y de host.

En el entorno virtual, ejecute el siguiente comando y elija **python** como el rol de trabajo en tiempo de ejecución.

```command
func init MyFunctionProj
```

Se crea una carpeta denominada _MyFunctionProj_, que contiene los tres archivos siguientes:

* `local.settings.json` se usa para almacenar las cadenas de conexión y la configuración de la aplicación cuando la ejecución se realiza a nivel local. Este archivo no se publica en Azure.
* `requirements.txt` contiene la lista de paquetes que se instalarán al publicarlos en Azure.
* `host.json` contiene las opciones de configuración global que afectan a todas las funciones de una aplicación de función. Este archivo se publica en Azure.

Vaya a la nueva carpeta MyFunctionProj:

```command
cd MyFunctionProj
```

A continuación, actualice el archivo host.json para habilitar conjuntos de extensiones.  

## <a name="reference-bindings"></a>Enlaces de referencia

Los conjuntos de extensiones facilitan la adición de extensiones de enlace más adelante. También elimina el requisito de instalación del SDK de .NET Core 2.x. Los conjuntos de extensiones requieren la versión 2.6.1071 de Core Tools o una versión posterior. 

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Ahora, puede agregar una función al proyecto.

## <a name="create-a-function"></a>Creación de una función

Para agregar una función a su proyecto, ejecute el siguiente comando:

```command
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

```bash
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

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

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

Ahora ya está listo para publicar el proyecto de funciones local en la aplicación de función en Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado un proyecto de funciones de Python con una función desencadenada por HTTP, lo ha ejecutado en el equipo local y lo ha implementado en Azure. Ahora, amplíe la función como sigue...

> [!div class="nextstepaction"]
> [Adición de un enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md)
