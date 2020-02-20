---
title: Adición de un enlace de cola de Azure Storage a la función de Python
description: Integración de una cola de Azure Storage con una función de Python mediante un enlace de salida.
ms.date: 01/15/2020
ms.topic: quickstart
ms.openlocfilehash: f5527e0e636c3f8c9ee3723570ed9811f0df3641
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198486"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Adición de un enlace de cola de Azure Storage a la función de Python

En este artículo, integrará una cola de Azure Storage con la función y la cuenta de almacenamiento que creó en [Creación de una función de Python desencadenada por HTTP](functions-create-first-function-python.md). Para lograr esta integración se usa un *enlace de salida* que escribe los datos de una solicitud HTTP en un mensaje de la cola. Completar este artículo no supone ningún costo adicional sobre el pequeño importe del inicio rápido anterior.

## <a name="prerequisites"></a>Prerrequisitos

- Complete el inicio rápido, [Creación de una función de Python desencadenada mediante HTTP](functions-create-first-function-python.md). Si ya ha limpiado los recursos al final de ese artículo, siga los pasos de nuevo para volver a crear la aplicación Functions en Azure, pero deje los recursos en su lugar.

## <a name="retrieve-the-azure-storage-connection-string"></a>Recuperación de la cadena de conexión de Azure Storage

Al crear una aplicación de funciones en Azure en el inicio rápido anterior, también creó una cuenta de Storage. La cadena de conexión de esta cuenta se almacena de forma segura en la configuración de la aplicación en Azure. Mediante la descarga de la configuración en el archivo *local.settings.json*, puede usar esa conexión para escribir en una cola de Storage de la misma cuenta cuando ejecute la función de forma local. 

1. En la raíz del proyecto, ejecute el comando siguiente, reemplazando `<app_name>` por el nombre de la aplicación de funciones del inicio rápido anterior. Este comando sobrescribirá los valores existentes en el archivo.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. Abra *local.settings.json* y busque el valor denominado `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Usará el nombre `AzureWebJobsStorage` y la cadena de conexión en otras secciones de este artículo.

> [!IMPORTANT]
> Como *local.settings.json* contiene secretos descargados de Azure, excluya siempre este archivo del control de código fuente. El archivo *.gitignore* que se creó con un proyecto de Functions local excluye el archivo de forma predeterminada.

## <a name="add-an-output-binding-to-functionjson"></a>Adición de un enlace de salida a function.json

Aunque ninguna de las funciones puede tener más de un desencadenador, puede tener varios enlaces de entrada y salida que le permiten conectarse a otros servicios y recursos de Azure sin escribir código de integración personalizado. Estos enlaces se declaran en el archivo *function.json* en la carpeta function según sea necesario para el lenguaje que se usa para la función.

En el inicio rápido anterior, el archivo *function.json* de la carpeta *HttpExample* contiene dos enlaces en la colección `bindings`:

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
    }
  ]
}
```

Cada enlace tiene al menos un tipo, una dirección y un nombre. En el ejemplo anterior, el primer enlace es del tipo `httpTrigger` con la dirección `in`. En el caso de la dirección `in`, `name` especifica el nombre de un parámetro de entrada que se envía a la función cuando la invoca el desencadenador.

El segundo enlace es del tipo `http` con la dirección `out`, en cuyo caso la variable `name` especial de `$return` indica que este enlace utiliza el valor devuelto de la función, en lugar de proporcionar un parámetro de entrada.

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

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

En este caso, `msg` se asigna a la función como argumento de salida. En el caso de un tipo `queue`, también debe especificar el nombre de la cola en `queueName` y proporcionar el *nombre* de la conexión Azure Storage (desde *local.settings.json*) en `connection`.

Para más información sobre los enlaces, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md) y [configuración de la cola de salida](functions-bindings-storage-queue.md#output---configuration).

## <a name="add-code-to-use-the-output-binding"></a>Adición de código para usar el enlace de salida

Con el enlace de cola especificado en *function.json*, puede actualizar la función para que reciba el parámetro de salida `msg` y escribir mensajes en la cola.

Actualice *HttpExample\\\_\_init\_\_.py* para que se ajuste al siguiente código y agregue el parámetro `msg` a la definición de la función y `msg.set(name)` en la instrucción `if name:`.

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

El parámetro `msg` es una instancia de [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest). Su método `set` escribe un mensaje de cadena en la cola; en este caso, el nombre que se pasa a la función en la cadena de consulta de la dirección URL.

Observe que *no* necesita escribir código para la autenticación, para obtener una referencia de la cola ni para escribir datos. Todas estas tareas de integración se administran de manera adecuada en el entorno de ejecución de Azure Functions y en el enlace de salida de la cola.

## <a name="run-and-test-the-function-locally"></a>Ejecución y prueba de la función en un entorno local

1. En un terminal o símbolo del sistema, vaya a la carpeta del proyecto function, *LocalFunctionProj*.

1. Inicie el host en tiempo de ejecución de Azure Functions, para lo que debe usar el siguiente comando.

    ```
    func host start
    ```

1. Una vez que se complete el inicio y vea la dirección URL del punto de conexión `HttpExample`, copie su dirección URL en un explorador y anexe la cadena de consulta `?name=<your-name>`, lo que hace que la dirección URL completa sea `http://localhost:7071/api/HttpExample?name=Guido`. El examinador debería mostrar el mensaje `Hello Guido` como en el artículo anterior.

    Si no ve el punto de conexión `HttpExample`, detenga el host con **Ctrl**+**C** y compruebe si hay errores en la salida. Por ejemplo, el host no activará el punto de conexión si hay un error en *function.json*. Compruebe también que ejecuta `func host start` desde la carpeta del proyecto functions, no desde la carpeta *HttpExample*.

1. Cuando haya terminado, detenga el host con **Ctrl**+**C**.

> [!TIP]
> Durante el inicio, el host descarga e instala la [extensión de enlace de Storage](functions-bindings-storage-blob.md#add-to-your-functions-app) y otras extensiones de enlace de Microsoft. Esta instalación se produce porque las extensiones de enlace se habilitan de forma predeterminada en el archivo *host.json* con las siguientes propiedades:
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> Si encuentra algún error relacionado con las extensiones de enlace, compruebe que las propiedades anteriores están presentes en *host.json*.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Visualización del mensaje en la cola de Azure Storage

Cuando una función genera una respuesta HTTP para el explorador web, también llama a `msg.set(name)`, que escribe un mensaje en una cola de Azure Storage denominada `outqueue`, tal como se especifica en el enlace de la cola. La cola se puede ver en [Azure Portal](../storage/queues/storage-quickstart-queues-portal.md) o en el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/). También puede ver la cola en la CLI de Azure como se indica en los pasos siguientes:

1. Abra el archivo *local.setting.json* del proyecto de Functions y copie el valor de la cadena de conexión. En una ventana de terminal o de comandos, ejecute el siguiente comando para crear una variable de entorno denominada `AZURE_STORAGE_CONNECTION_STRING` y pegue la cadena de conexión concreta en lugar de `<connection_string>`. (Esta variable de entorno significa que no es necesario proporcionar la cadena de conexión a cada comando posterior mediante el argumento `--connection-string`).

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<connection_string>"
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    ---
    
1. (Opcional) Puede usar el comando [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) para ver las colas de Storage de la cuenta. La salida de este comando debe incluir una cola denominada `outqueue`, la cual se creó cuando la función escribió su primer mensaje en esa cola.
    
    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    az storage queue list --output tsv
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    az storage queue list --output tsv
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    az storage queue list --output tsv
    ```
    
    ---


1. Use el comando [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) para ver los mensajes de esta cola, la cual debe ser el nombre que utilizó al probar la función anteriormente. El comando recupera el primer mensaje de la cola con [codificación Base64](functions-bindings-storage-queue.md#encoding), por lo que también deberá descodificar el mensaje para verlo como texto.

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    Como debe desreferenciar la colección de mensajes y descodificar a partir de Base64, ejecute PowerShell y use el comando de PowerShell.

    ---
    
## <a name="redeploy-the-project-to-azure"></a>Nueva implementación del proyecto en Azure

Ahora que ha probado la función en un entorno local y ha comprobado que ha escrito un mensaje en la cola de Azure Storage, puede volver a implementar el proyecto para actualizar el punto de conexión que se ejecuta en Azure.

1. En la carpeta *LocalFunctionsProj*, use el comando [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) para volver a implementar el proyecto y reemplace`<app_name>` el nombre de la aplicación.

    ```
    func azure functionapp publish <app_name>
    ```
    
1. Como en el inicio rápido anterior, use un explorador o CURL para probar la función que ha vuelto a implementar.

    # <a name="browser"></a>[Browser](#tab/browser)
    
    Copie el valor completo de **Invoke URL** que se muestra en la salida del comando de publicación en una barra de direcciones del explorador, anexando el parámetro de consulta `&name=Azure`. El explorador debe mostrar una salida similar a cuando ejecutó la función localmente.

    ![La salida de la función ejecutada en Azure en un explorador](./media/functions-create-first-function-python/function-test-cloud-browser.png)

    # <a name="curl"></a>[curl](#tab/curl)
    
    Ejecute [curl](https://curl.haxx.se/) con la **dirección URL de invocación**  y anexe el parámetro `&name=Azure`. El resultado del comando debería ser el texto "Hello Azure".
    
    ![La salida de la función ejecutada en Azure en un mediante curl](./media/functions-create-first-function-python/function-test-cloud-curl.png)

    --- 

1. Vuelva a examinar la cola de Storage, como se describe en la sección anterior, para comprobar que contiene el nuevo mensaje escrito en la cola.

    Si utiliza la CLI de Azure para examinar la cola, el comando `az storage message peek` muestra solo el primer mensaje de la cola. Para simular el procesamiento de los mensajes, utilice `az storage message get` en su lugar con los mismos argumentos. El comando `get` devuelve el mensaje y lo quita de la cola. Después, puede repetir el mismo comando hasta que la cola esté vacía (y el comando genere un error).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si continúa con el paso siguiente, [Habilitación de la integración de Application Insights](functions-monitoring.md#manually-connect-an-app-insights-resource), conserve todos los recursos intactos, ya que va a crear a partir de lo que ya ha hecho.

De lo contrario, use el siguiente comando para eliminar el grupo de recursos y todos los recursos que contiene para evitar incurrir en costos adicionales.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Habilitación de la integración de Application Insights con una aplicación Azure Functions](functions-monitoring.md#manually-connect-an-app-insights-resource)

Otros recursos:

- [Ejemplos de proyectos de Function completos en Python](/samples/browse/?products=azure-functions&languages=python).
- [Guía de Azure Functions para desarrolladores de Python](functions-reference-python.md)
- [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).
- [Página de precios de Functions](https://azure.microsoft.com/pricing/details/functions/)
- Artículo [Cálculo de costos según el plan de consumo](functions-consumption-costs.md).
