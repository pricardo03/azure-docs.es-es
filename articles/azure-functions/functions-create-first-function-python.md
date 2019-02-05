---
title: Creación de la primera función de Python en Azure
description: Aprenda a crear su primera función de Python en Azure mediante Azure Functions Core Tools y la CLI de Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 08/29/2018
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 631fc4e613311df9386084408ead625f6d018151
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911897"
---
# <a name="create-your-first-python-function-in-azure-preview"></a>Creación de la primera función de Python en Azure (versión preliminar)

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

Esta guía de inicio rápido le explica cómo usar la CLI de Azure para crear su primera aplicación de función de Python [sin servidor](https://azure.com/serverless) para que se ejecute en Linux. El código de función se crea localmente y, a continuación, se implementa en Azure mediante [Azure Functions Core Tools](functions-run-local.md). Para más información sobre consideraciones de versión preliminar para ejecutar las aplicaciones de función en Linux, consulte [este artículo sobre funciones en Linux](https://aka.ms/funclinux).

Los pasos siguientes se admiten en equipos Mac, Windows o Linux.

## <a name="prerequisites"></a>Requisitos previos

Para realizar la compilación y las pruebas de forma local tiene que:

+ Instale [Python 3.6](https://www.python.org/downloads/).

+ Instale [Azure Functions Core Tools](functions-run-local.md#v2) versión 2.2.70 u otra posterior (requiere el SDK de .NET Core 2.x).

Para publicar y ejecutar en Azure:

+ Instale la versión 2.x de la [CLI de Azure]( /cli/azure/install-azure-cli) u otra posterior.

+ Necesita una suscripción de Azure activa.
  [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Creación y activación de un entorno virtual

Para crear un proyecto de Functions, es necesario que trabaje en un entorno virtual de Python 3.6. Ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.env`.

```bash
# In Bash
python3.6 -m venv .env
source .env/bin/activate

# In PowerShell
py -3.6 -m venv .env
.env\scripts\activate
```

## <a name="create-a-local-functions-project"></a>Creación de un proyecto local de Functions

Ahora ya puede crear un proyecto local de Functions. Este directorio es el equivalente a una aplicación de función en Azure. Puede contener varias funciones que comparten la misma configuración local y de host.

En la ventana de terminal, o desde un símbolo del sistema, ejecute el siguiente comando:

```bash
func init MyFunctionProj
```

Elija **python** como el entorno de ejecución deseado.

```output
Select a worker runtime:
1. dotnet
2. node
3. python
```

Verá algo parecido a la siguiente salida.

```output
Installing wheel package
Installing azure-functions package
Installing azure-functions-worker package
Running pip freeze
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing /MyFunctionProj/.vscode/extensions.json
```

Se crea una carpeta nueva denominada _MyFunctionProj_. Para continuar, cambie el directorio a esta carpeta.

```bash
cd MyFunctionProj
```

## <a name="create-a-function"></a>Creación de una función

Para crear una función, ejecute el siguiente comando:

```bash
func new
```

Elija `HTTP Trigger` como la plantilla y proporcione un **nombre de función** de `HttpTrigger`.

```output
Select a template:
1. Blob trigger
2. Cosmos DB trigger
3. Event Grid trigger
4. Event Hub trigger
5. HTTP trigger
6. Queue trigger
7. Service Bus Queue trigger
8. Service Bus Topic trigger
9. Timer trigger

Choose option: 5
Function name: HttpTrigger
```

Verá algo parecido a la siguiente salida.

```output
Writing /MyFunctionProj/HttpTrigger/sample.dat
Writing /MyFunctionProj/HttpTrigger/__init__.py
Writing /MyFunctionProj/HttpTrigger/function.json
The function "HttpTrigger" was created successfully from the "HTTP trigger" template.
```

Se crea una subcarpeta denominada _HttpTrigger_. Este archivo contiene `__init__.py` que es el archivo de script principal y el archivo `function.json` que describe los desencadenadores y enlaces que usa la función. Para más información sobre el modelo de programación, puede hacer referencia a la [guía para desarrolladores de Python de Azure Functions](functions-reference-python.md).

## <a name="run-the-function-locally"></a>Ejecución local de la función

Utilice el siguiente comando para ejecutar el host de Functions localmente.

```bash
func host start
```

Cuando se inicia el host de Functions, este devuelve la dirección URL de la función desencadenada por HTTP. (Tenga en cuenta que toda la salida se ha truncado para mejorar la legibilidad).

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
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.
...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger
```

Copie la dirección URL de la función que aparece en la salida y péguela en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud.

    http://localhost:7071/api/HttpTrigger?name=<yourname>

En la siguiente captura de pantalla aparece la respuesta de la función cuando esta se desencadena desde el explorador:

![test](./media/functions-create-first-function-python/function-test-local-browser.png)

Ahora ya está listo para crear una aplicación de función y los demás recursos necesarios para la publicación en Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Creación de una aplicación de función de Linux en Azure

La aplicación de función proporciona un entorno para la ejecución del código de su función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. Cree una **aplicación de función de Python que se ejecute en Linux** con el comando [az functionapp create](/cli/azure/functionapp#az_functionapp_create).

Ejecute el siguiente comando mediante un nombre de aplicación de función único en lugar del marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. `<app_name>` también es el dominio DNS predeterminado de la aplicación de función. Este nombre debe ser único entre todas las aplicaciones de Azure.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <app_name> --storage-account  <storage_name>
```

> [!NOTE]
> Si tiene un grupo de recursos existente llamado `myResourceGroup` con cualquier aplicación de App Service que no sea de Linux, deberá usar un grupo de recursos diferente. No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos.  

Una vez creada la aplicación de función, verá el siguiente mensaje:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Ahora ya está listo para publicar el proyecto de funciones local en Function App en Azure.

## <a name="deploy-the-function-app-project-to-azure"></a>Implementación del proyecto de aplicación de función en Azure

En Azure Functions Core Tools, ejecute el siguiente comando. Sustituya `<app_name>` por el nombre de la aplicación del paso anterior.

```bash
func azure functionapp publish <app_name>
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

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo desarrollar funciones de Azure con Python.

> [!div class="nextstepaction"]
> [Guía para desarrolladores de Python de Azure Functions](functions-reference-python.md)
> [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
