---
title: Adición de un enlace de cola de Azure Storage a la función de Python
description: Aprenda a agregar un enlace de salida de cola de Azure Storage a la función de Python mediante la CLI de Azure y Functions Core Tools.
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
ms.openlocfilehash: 34ec7c678410b2e0814f8dbb7a69257886cb891d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639106"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Adición de un enlace de cola de Azure Storage a la función de Python

Azure Functions le permite conectar servicios de Azure y otros recursos a funciones sin tener que escribir su propio código de integración. Estos *enlaces*, que representan la entrada y la salida, se declaran dentro de la definición de función. Los datos de los enlaces se proporcionan a la función como parámetros. Un *desencadenador* es un tipo especial de enlace de entrada. Si bien una función tiene un único desencadenador, puede tener varios enlaces de entrada y salida. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

En este artículo se muestra cómo integrar la función que creó en el [artículo de inicio rápido anterior](functions-create-first-function-python.md) con una cola de Azure Storage. El enlace de salida que se agrega a esta función escribe datos de una solicitud HTTP en un mensaje de la cola.

La mayoría de los enlaces requieren una cadena de conexión almacenada que se usa en Functions para acceder al servicio enlazado. Para facilitar la conexión, usará la cuenta de almacenamiento que creó con la aplicación de funciones. La conexión a esta cuenta ya está almacenada en una configuración de aplicación llamada `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este artículo, realice los pasos de la [parte 1 del inicio rápido de Python](functions-create-first-function-python.md).

## <a name="download-the-function-app-settings"></a>Descarga de la configuración de la aplicación de función

En el artículo de inicio rápido anterior creó una aplicación de funciones en Azure junto con una cuenta de Storage. La cadena de conexión de esta cuenta se almacena de forma segura en la configuración de la aplicación en Azure. En este artículo, escribirá mensajes en una cola de almacenamiento de la misma cuenta. Para conectarse a su cuenta de almacenamiento cuando se ejecuta la función localmente, debe descargar la configuración de la aplicación en el archivo local.settings.json. Ejecute el siguiente el comando de Azure Functions Core Tools para descargar la configuración en local.settings.json y reemplace `<APP_NAME>` por el nombre de la aplicación de funciones del artículo anterior:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Es posible que tenga que iniciar sesión en su cuenta de Azure.

> [!IMPORTANT]  
> Como contiene secretos, el archivo local.settings.json nunca se publica y debe excluirse del control de código fuente.

Necesitará el valor `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Esta conexión se usa para comprobar que el enlace de salida funciona según lo previsto.

## <a name="enable-extension-bundles"></a>Habilitación de conjuntos de extensiones

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Ahora podrá agregar el enlace de salida de almacenamiento al proyecto.

## <a name="add-an-output-binding"></a>Adición de un enlace de salida

En Functions, para cada tipo de enlace es necesario definir los elementos `direction`, `type` y un valor único de `name` en el archivo function.json. Según el tipo de enlace, pueden ser necesarias propiedades adicionales. En la [configuración de salida de cola](functions-bindings-storage-queue.md#output---configuration) se describen los campos necesarios para un enlace de cola de Azure Storage.

Para crear un enlace, se agrega un objeto de configuración de enlace al archivo function.json. Edite el archivo function.json en la carpeta HttpTrigger para agregar un objeto a la matriz `bindings`, que tiene las siguientes propiedades:

| Propiedad | Valor | DESCRIPCIÓN |
| -------- | ----- | ----------- |
| **`name`** | `msg` | Nombre que identifica el parámetro de enlace al que se hace referencia en el código. |
| **`type`** | `queue` | El enlace es un enlace de cola de Azure Storage. |
| **`direction`** | `out` | El enlace es un enlace de salida. |
| **`queueName`** | `outqueue` | El nombre de la cola en la que escribe el enlace. Cuando no existe `queueName`, el enlace lo crea durante el primer uso. |
| **`connection`** | `AzureWebJobsStorage` | El nombre de una configuración de la aplicación que contiene la cadena de conexión de la cuenta de almacenamiento. El valor `AzureWebJobsStorage` contiene la cadena de conexión de la cuenta de almacenamiento que creó con la aplicación de función. |

El archivo function.json debe ser ahora similar al siguiente ejemplo:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
  {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

## <a name="add-code-that-uses-the-output-binding"></a>Adición de código que utilice el enlace de salida

Una vez configurado `name`, puede empezar a usarlo para acceder al enlace como atributo de método en la signatura de función. En el ejemplo siguiente, `msg` es una instancia de [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest).

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> str:

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        msg.set(name)
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

Al usar un enlace de salida, no tiene que usar el código del SDK de Azure Storage para autenticarse, obtener una referencia de cola o escribir datos. El sistema en tiempo de ejecución de Functions y el enlace de salida de cola realizan esas tareas automáticamente.

## <a name="run-the-function-locally"></a>Ejecución local de la función

Como antes, use el siguiente comando para iniciar el sistema en tiempo de ejecución de Functions localmente:

```bash
func host start
```

> [!NOTE]  
> Como en el artículo de inicio rápido anterior habilitó las agrupaciones de extensiones en el archivo host.json, la [extensión de enlace de almacenamiento](functions-bindings-storage-blob.md#packages---functions-2x) se descargó e instaló automáticamente durante el inicio junto con el resto de extensiones de enlace de Microsoft.

Copie la dirección URL de la función `HttpTrigger` que aparece en la salida en entorno de ejecución y péguela en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. Verá la misma respuesta en el explorador que en el artículo anterior.

Esta vez, el enlace de salida también crea una cola denominada `outqueue` en la cuenta de almacenamiento y agrega un mensaje con esta misma cadena.

A continuación, se usa la CLI de Azure para ver la nueva cola y comprobar que se ha agregado un mensaje. También puede ver la cola mediante el [Explorador de Microsoft Azure Storage][Azure Storage Explorer] o en [Azure Portal](https://portal.azure.com).

### <a name="set-the-storage-account-connection"></a>Establecimiento de la conexión de la cuenta de almacenamiento

Abra el archivo local.settings.json y copie el valor de `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Establezca la variable de entorno `AZURE_STORAGE_CONNECTION_STRING` en la cadena de conexión con el siguiente comando de Bash:

```azurecli-interactive
export AZURE_STORAGE_CONNECTION_STRING=<STORAGE_CONNECTION_STRING>
```

La establecer la cadena de conexión en la variable de entorno `AZURE_STORAGE_CONNECTION_STRING`, puede acceder a la cuenta de almacenamiento sin tener que autenticarse cada vez.

### <a name="query-the-storage-queue"></a>Consulta de la cola de almacenamiento

Puede usar el comando [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) para ver las colas de almacenamiento de la cuenta, como en el ejemplo siguiente:

```azurecli-interactive
az storage queue list --output tsv
```

La salida de este comando incluye una cola denominada `outqueue`, que es la cola que se creó cuando se ejecutó la función.

A continuación, use el comando [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) para ver los mensajes de esta cola, como se muestra en este ejemplo:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

La cadena devuelta debe ser la misma que el mensaje que envió para probar la función.

> [!NOTE]  
> En el ejemplo anterior se descodifica la cadena devuelta desde base64. Esto se debe a que los enlaces de Queue Storage escriben y leen en Azure Storage como [cadenas de base64](functions-bindings-storage-queue.md#encoding).

Ahora es el momento de volver a publicar la aplicación de funciones actualizada en Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

De nuevo, puede usar cURL o un explorador para probar la función implementada. Como antes, anexe la cadena de consulta `&name=<yourname>` a la dirección URL, como en este ejemplo:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

Puede [examinar el mensaje de la cola de almacenamiento](#query-the-storage-queue) para comprobar que el enlace de salida genera otra vez un nuevo mensaje en la cola.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha actualizado la función desencadenada por HTTP para escribir datos en una cola de almacenamiento. Para más información sobre el desarrollo de Azure Functions con Python, consulte la [Guía de Azure Functions para desarrolladores de Python](functions-reference-python.md) y [Desencadenadores y enlaces en Azure Functions](functions-triggers-bindings.md).

A continuación, debe habilitar la supervisión de Application Insights para su aplicación de función:

> [!div class="nextstepaction"]
> [Habilitación de la integración de Application Insights](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/