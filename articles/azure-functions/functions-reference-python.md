---
title: Referencia para desarrolladores de Python para Azure Functions
description: Aprenda a desarrollar funciones con Python
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: funciones de azure, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor, python
ms.service: functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: 619db07204b88609314d0d3d06709eaa93cb7a43
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53188041"
---
# <a name="azure-functions-python-developer-guide"></a>Guía de Azure Functions para desarrolladores de Python

Este artículo es una introducción al desarrollo de Azure Functions mediante Python. En lo que va a leer a continuación se supone que ya ha leído la [guía para desarrolladores de Azure Functions](functions-reference.md).

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Modelo de programación

Una función de Azure Function debe ser un método sin estado de un script de Python que procese entradas y produzca salidas. De forma predeterminada, el entorno de ejecución espera que este se implemente como un método global denominado `main()` en el archivo `__init__.py`.

Puede cambiar la configuración predeterminada si especifica las propiedades `scriptFile` y `entryPoint` en el archivo `function.json`. Por ejemplo, el archivo _function.json_ siguiente indica al entorno de ejecución que use el método _customentry()_ del archivo _main.py_, como punto de entrada para la función de Azure.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Los datos de los desencadenadores y enlaces se enlazan a la función a través de los atributos del método con la propiedad `name` definida en el archivo de configuración `function.json`. Por ejemplo, en el archivo _function.json_ siguiente se describe una función simple desencadenada por una solicitud HTTP denominada `req`:

```json
{
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

El archivo `__init__.py` contiene el código de función siguiente:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Si lo desea, también puede declarar los tipos de parámetros y el tipo de valor devuelto de la función mediante las anotaciones de tipos de Python. Por ejemplo, puede escribir la misma función con anotaciones, como sigue:

```python
import azure.functions

def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```  

Utilice las anotaciones de Python incluidas en el paquete [azure.functions.*](/python/api/azure-functions/azure.functions?view=azure-python) para enlazar las entradas y las salidas a los métodos. 

## <a name="folder-structure"></a>Estructura de carpetas

La estructura de carpetas para un proyecto de Python de Azure Functions tiene la siguiente apariencia:

```
 FunctionApp
 | - MyFirstFunction
 | | - __init__.py
 | | - function.json
 | - MySecondFunction
 | | - __init__.py
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.py
 | | - mySecondHelperFunction.py
 | - host.json
 | - requirements.txt
 | - extensions.csproj
 | - bin
```

Hay un archivo [host.json](functions-host-json.md) compartido que se puede usar para configurar la aplicación de función. Cada función tiene su propio archivo de código y archivo de configuración de enlace (function.json). 

El código compartido debe mantenerse en una carpeta independiente. Para hacer referencia a los módulos en la carpeta SharedCode, puede usar la sintaxis siguiente:

```
from ..SharedCode import myFirstHelperFunction
```

Las extensiones de enlace que usa el entorno de ejecución de Functions se definen en el archivo `extensions.csproj`, mientras que los archivos de biblioteca reales se hallan en la carpeta `bin`. Al desarrollar de forma local, debe [registrar las extensiones de enlace](functions-triggers-bindings.md#local-development-azure-functions-core-tools) con Azure Functions Core Tools. 

Al implementar un proyecto de Functions en la aplicación de función en Azure, todo el contenido de la carpeta FunctionApp debe incluirse en el paquete, pero no en la carpeta.

## <a name="inputs"></a>Entradas

Las entradas se dividen en dos categorías dentro de Azure Functions: una entrada del desencadenador y otra adicional. Aunque son diferentes en `function.json`, se usan igual en el código de Python. Tomemos el siguiente fragmento de código a modo de ejemplo:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```python
import azure.functions as func
import logging

def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Cuando se invoca la función, la solicitud HTTP se pasa a la función como `req`. Se recuperará una entrada de Azure Blob Storage según el _identificador_ de la dirección URL de la ruta y estará disponible como `obj` en el cuerpo de la función.

## <a name="outputs"></a>Salidas

Las salidas se pueden expresar como valores devueltos y como parámetros de salida. Si hay una única salida, se recomienda usar el valor devuelto. Para varias salidas, deberá utilizar parámetros de salida.

Para usar el valor devuelto de una función como valor de un enlace de salida, la propiedad `name` del enlace debe establecerse como `$return` en `function.json`.

Si desea generar varias salidas, utilice el método `set()` que la interfaz `azure.functions.Out` ofrece para asignar un valor al enlace. Por ejemplo, la siguiente función puede insertar un mensaje en una cola y también devolver una respuesta HTTP.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func

def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Registro

El acceso al registrador del entorno de ejecución de Azure Functions está disponible a través de un controlador [`logging`](https://docs.python.org/3/library/logging.html#module-logging) raíz en la aplicación de función. Este registrador está asociado a Application Insights y permite marcar las advertencias y los errores detectados durante la ejecución de la función.

En el ejemplo siguiente se registra un mensaje de información cuando la función se invoca con un desencadenador HTTP.

```python
import logging

def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Hay métodos de registro adicionales disponibles que permiten escribir en la consola en otros niveles de seguimiento:

| Método                 | DESCRIPCIÓN                                |
| ---------------------- | ------------------------------------------ |
| logging.**critical(_message_)**   | Escribe un mensaje con el nivel CRÍTICO en el registrador de raíz.  |
| logging.**error(_message_)**   | Escribe un mensaje con el nivel ERROR en el registrador de raíz.    |
| logging.**warning(_message_)**    | Escribe un mensaje con el nivel ADVERTENCIA en el registrador de raíz.  |
| logging.**info(_message_)**    | Escribe un mensaje con el nivel INFO en el registrador de raíz.  |
| logging.**debug(_message_)** | Escribe un mensaje con el nivel DEBUG en el registrador de raíz.  |

## <a name="importing-shared-code-into-a-function-module"></a>Importación de código compartido en un módulo de función

Los módulos de Python que se publican junto con los módulos de función deben importarse con la sintaxis de importación relativa:

```python
from . import helpers  # Use more dots to navigate up the folder structure.
def main(req: func.HttpRequest):
    helpers.process_http_request(req)
```

Como alternativa, coloque el código compartido en un paquete independiente, publíquelo en una instancia de PyPI privada o pública, y especifíquelo como una dependencia normal.

## <a name="async"></a>Async

Puesto que solo puede existir un único proceso de Python por cada aplicación de función, se recomienda implementar la función de Azure como una corrutina asincrónica mediante la instrucción `async def`.

```python
# Will be run with asyncio directly
async def main():
    await some_nonblocking_socket_io_op()
```

Si la función main() es sincrónica (no tiene el calificador `async`), se ejecuta automáticamente en un grupo de subprocesos `asyncio`.

```python
# Would be run in an asyncio thread-pool
def main():
    some_blocking_socket_io()
```

## <a name="context"></a>Context

Para obtener el contexto de invocación de una función durante la ejecución, incluya el argumento `context` en su firma. 

Por ejemplo: 

```python
import azure.functions

def main(req: azure.functions.HttpRequest,
            context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

La clase **Context** tiene los métodos siguientes:

`function_directory`  
El directorio en que se ejecuta la función.

`function_name`  
El nombre de la función.

`invocation_id`  
El identificador de la invocación de la función actual.

## <a name="python-version-and-package-management"></a>La versión de Python y la administración de paquetes.

Actualmente, Azure Functions admite solo Python 3.6.x (distribución oficial de CPython).

Al desarrollar localmente con Azure Functions Core Tools o Visual Studio Code, agregue los nombres y las versiones de los paquetes necesarios al archivo `requirements.txt` e instálelos mediante `pip`.

Por ejemplo, se puede usar el comando de archivo y pip siguiente para instalar el paquete `requests` desde PyPI.

```bash
pip install requests
```

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

Cuando esté preparado para la publicación, asegúrese de que todas sus dependencias se muestran en el archivo `requirements.txt`, que se encuentra en la raíz del directorio del proyecto. Para ejecutar correctamente Azure Functions, el archivo de requisitos debe contener los siguientes paquetes, como mínimo:

```txt
azure-functions
azure-functions-worker
grpcio==1.14.1
grpcio-tools==1.14.1
protobuf==3.6.1
six==1.11.0
```

## <a name="publishing-to-azure"></a>Publicación en Azure

Si usa un paquete que requiere un compilador y no admite la instalación de ruedas compatibles con manylinux desde PyPI, se producirá el error siguiente en la publicación en Azure: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Para compilar y configurar los archivos binarios necesarios automáticamente, [instale Docker](https://docs.docker.com/install/) en el equipo local y ejecute el siguiente comando para publicar con [Azure Functions Core Tools](functions-run-local.md#v2) (func). Reemplace `<app name>` por el nombre de la aplicación de función de Azure. 

```bash
func azure functionapp <app name> --build-native-deps
```

Interiormente, Core Tools usará Docker para ejecutar la imagen [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) como un contenedor en la máquina local. Con este entorno, se compilarán e instalarán los módulos necesarios desde la distribución de origen, antes de empaquetarlos para la implementación final en Azure.

> [!NOTE]
> Core Tools (func) usa el programa PyInstaller para inmovilizar el código y las dependencias del usuario en un único ejecutable independiente que se ejecutará en Azure. Esta funcionalidad está actualmente en versión preliminar y no puede extenderse a todos los tipos de paquetes de Python. Si no se puede importar los módulos, intente publicar de nuevo con la opción `--no-bundler`. 
> ```
> func azure functionapp publish <app_name> --build-native-deps --no-bundler
> ```
> Si sigue teniendo problemas, díganoslo; para ello, [abra una incidencia](https://github.com/Azure/azure-functions-core-tools/issues/new) e incluya una descripción del problema. 


Para compilar las dependencias y publicar con un sistema de entrega e integración continuas (CI-CD), puede usar una [canalización de Azure](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=vsts) o un [script personalizado de Travis CI](https://docs.travis-ci.com/user/deployment/script/). 

A continuación se muestra un ejemplo de script `azure-pipelines.yml` para el proceso de compilación y publicación.
```yml
pool:
  vmImage: 'Ubuntu 16.04'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '8.x'

- script: |
    set -e
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo apt-get install -y apt-transport-https
    echo "install Azure CLI..."
    sudo apt-get update && sudo apt-get install -y azure-cli
    npm i -g azure-functions-core-tools --unsafe-perm true
    echo "installing dotnet core"
    curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 2.0
- script: |
    set -e
    az login --service-principal --username "$(APP_ID)" --password "$(PASSWORD)" --tenant "$(TENANT_ID)" 
    func settings add FUNCTIONS_WORKER_RUNTIME python
    func extensions install
    func azure functionapp publish $(APP_NAME) --build-native-deps
```

A continuación se muestra un ejemplo de script `.travis.yaml` para el proceso de compilación y publicación.

```yml
sudo: required

language: node_js

node_js:
  - "8"

services:
  - docker

before_install:
  - echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
  - curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
  - sudo apt-get install -y apt-transport-https
  - sudo apt-get update && sudo apt-get install -y azure-cli
  - npm i -g azure-functions-core-tools --unsafe-perm true


script:
  - az login --service-principal --username "$APP_ID" --password "$PASSWORD" --tenant "$TENANT_ID"
  - az account get-access-token --query "accessToken" | func azure functionapp publish $APP_NAME --build-native-deps

```

## <a name="known-issues-and-faq"></a>Problemas conocidos y preguntas más frecuentes

Todos los problemas conocidos y las solicitudes de características se siguen mediante la lista de[problemas de GitHub](https://github.com/Azure/azure-functions-python-worker/issues). Si le surge algún problema y no lo encuentra en GitHub, abra un nuevo problema e incluya una descripción detallada del mismo.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
* [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
* [Enlaces de Blob Storage](functions-bindings-storage-blob.md)
* [Enlaces de HTTP y de Webhook](functions-bindings-http-webhook.md)
* [Enlaces de Queue Storage](functions-bindings-storage-queue.md)
* [Desencadenador de temporizador](functions-bindings-timer.md)
