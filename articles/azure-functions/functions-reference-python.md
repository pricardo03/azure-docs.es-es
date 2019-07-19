---
title: Referencia para desarrolladores de Python para Azure Functions
description: Aprenda a desarrollar funciones con Python
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: funciones de azure, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor, python
ms.service: azure-functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: 249e5ac33b1420ada2cda45ea729471351f21adf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341991"
---
# <a name="azure-functions-python-developer-guide"></a>Guía de Azure Functions para desarrolladores de Python

Este artículo es una introducción al desarrollo de Azure Functions mediante Python. En lo que va a leer a continuación se supone que ya ha leído la [guía para desarrolladores de Azure Functions](functions-reference.md).

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Modelo de programación

Una función de Azure Function debe ser un método sin estado de un script de Python que procese entradas y produzca salidas. De forma predeterminada, el runtime espera que el modelo se implemente como un método global denominado `main()` en el archivo `__init__.py`.

Puede cambiar la configuración predeterminada si especifica las propiedades `scriptFile` y `entryPoint` en el archivo *function.json*. Por ejemplo, el archivo _function.json_ siguiente indica al runtime que use el método `customentry()` del archivo  _main.py_, como punto de entrada para la instancia de Azure Functions.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Los datos de los desencadenadores y enlaces se enlazan a la función a través de los atributos del método con la propiedad `name` definida en el archivo *function.json*. Por ejemplo, en el archivo _function.json_ siguiente se describe una función simple desencadenada por una solicitud HTTP denominada `req`:

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

Opcionalmente, para aprovechar las características de IntelliSense y Autocompletar proporcionadas por su editor de código, también puede declarar los tipos de atributo y el tipo de valor devuelto en la función mediante anotaciones de tipo Python. 

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
```

Hay un archivo [host.json](functions-host-json.md) compartido que se puede usar para configurar la aplicación de función. Cada función tiene su propio archivo de código y archivo de configuración de enlace (function.json). 

El código compartido debe mantenerse en una carpeta independiente. Para hacer referencia a los módulos en la carpeta SharedCode, puede usar la sintaxis siguiente:

```
from __app__.SharedCode import myFirstHelperFunction
```

Al implementar un proyecto de Functions en la aplicación de funciones en Azure, todo el contenido de la carpeta *FunctionApp* debe incluirse en el paquete, pero no en la carpeta.

## <a name="triggers-and-inputs"></a>Desencadenadores y entradas

Las entradas se dividen en dos categorías dentro de Azure Functions: una entrada del desencadenador y otra adicional. Aunque son diferentes en el archivo `function.json`, se usan igual en el código de Python.  Las cadenas de conexión o los secretos de los orígenes de entrada y el desencadenador se asignan a valores en el archivo `local.settings.json` al ejecutarse localmente y a la configuración de la aplicación al ejecutarse en Azure. 

Por ejemplo, el siguiente código muestra la diferencia entre las dos:

```json
// function.json
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

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Cuando se invoca la función, la solicitud HTTP se pasa a la función como `req`. Se recuperará una entrada de Azure Blob Storage según el _identificador_ de la dirección URL de la ruta y estará disponible como `obj` en el cuerpo de la función.  Aquí, la cuenta de almacenamiento especificada es la cadena de conexión encontrada en `AzureWebJobsStorage`, que es la misma cuenta de almacenamiento que usa la aplicación de funciones.


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

## <a name="async"></a>Async

Le recomendamos que escriba su instancia de Azure Functions como corrutina asincrónica mediante la instrucción `async def`.

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

## <a name="global-variables"></a>Variables globales

No se garantiza la conservación del estado de la aplicación para las ejecuciones futuras. Sin embargo, Azure Functions Runtime suele reutilizar el mismo proceso para varias ejecuciones de la misma aplicación. Para almacenar en caché los resultados de un cálculo costoso, debe declararse como variable global. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="python-version-and-package-management"></a>La versión de Python y la administración de paquetes.

Actualmente, Azure Functions admite solo Python 3.6.x (distribución oficial de CPython).

Al desarrollar localmente con Azure Functions Core Tools o Visual Studio Code, agregue los nombres y las versiones de los paquetes necesarios al archivo `requirements.txt` e instálelos mediante `pip`.

Por ejemplo, se puede usar el comando de archivo y pip siguiente para instalar el paquete `requests` desde PyPI.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Publicación en Azure

Cuando esté preparado para la publicación, asegúrese de que todas sus dependencias se muestran en el archivo *requirements.txt*, que se encuentra en la raíz del directorio del proyecto. Si usa un paquete que requiere un compilador y no admite la instalación de ruedas compatibles con manylinux desde PyPI, se producirá el error siguiente en la publicación en Azure: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Para compilar y configurar los archivos binarios necesarios automáticamente, [instale Docker](https://docs.docker.com/install/) en el equipo local y ejecute el siguiente comando para publicar con [Azure Functions Core Tools](functions-run-local.md#v2) (func). Reemplace `<app name>` por el nombre de la aplicación de función de Azure. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Interiormente, Core Tools usará Docker para ejecutar la imagen [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) como un contenedor en la máquina local. Con este entorno, se compilarán e instalarán los módulos necesarios desde la distribución de origen, antes de empaquetarlos para la implementación final en Azure.

Para crear sus dependencias y publicarlas mediante un sistema de entrega continua (CD), [use Azure DevOps Pipelines](https://docs.microsoft.com/azure/azure-functions/functions-how-to-azure-devops). 

## <a name="unit-testing"></a>Pruebas unitarias

Las funciones escritas en Python se pueden probar como otro código de Python mediante marcos de pruebas. Para la mayoría de los enlaces, es posible crear un objeto de entrada ficticio creando una instancia de una clase adecuada a partir del paquete `azure.functions`.

Por ejemplo, a continuación se muestra una prueba ficticia de una función desencadenada por HTTP:

```python
# myapp/__init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/my_function',
            params={'name': 'Test'})

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            'Hello, Test!',
        )
```

Este es otro ejemplo, con una función desencadenada por la cola:

```python
# myapp/__init__.py
import azure.functions as func


def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock Queue message.
        req = func.QueueMessage(
            body=b'test')

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp,
            'msg body: test',
        )
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
