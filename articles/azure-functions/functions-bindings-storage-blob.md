---
title: Enlaces de Azure Blob Storage para Azure Functions
description: Descubra cómo utilizar desencadenadores y enlaces de Azure Blob Storage en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 11/15/2018
ms.author: cshoe
ms.openlocfilehash: 5113b59f314a40feb02485b3fdc99fe4df406d88
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168104"
---
# <a name="azure-blob-storage-bindings-for-azure-functions"></a>Enlaces de Azure Blob Storage para Azure Functions

En este artículo se explica cómo trabajar con enlaces de Azure Blob Storage en Azure Functions. Azure Functions admite enlaces de desencadenador, entrada y salida para blobs. El artículo incluye una sección para cada enlace:

* [Desencadenador de blobs](#trigger)
* [Enlace de entrada de blob](#input)
* [Enlace de salida de blob](#output)

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Utilice el desencadenador de Event Grid en lugar del desencadenador de Blob Storage para las cuentas de almacenamiento solo para blobs, para operaciones a gran escala o para reducir la latencia. Para obtener más información, consulte la sección [Desencadenador](#trigger).

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

Los enlaces de Blob Storage se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), versión 2.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Blob) de GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Paquetes: Functions 2.x y versiones posteriores

Los enlaces de almacenamiento de Blob se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage), versión 3.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Desencadenador

El desencadenador de Blob Storage inicia una función cuando se detecta un blob nuevo o actualizado. El contenido del blob se proporciona a modo de entrada para la función.

El [desencadenador de Event Grid](functions-bindings-event-grid.md) tiene compatibilidad integrada para [eventos de blob](../storage/blobs/storage-blob-event-overview.md) y también puede utilizarse para iniciar una función cuando se detecta un blob nuevo o actualizado. Para obtener un ejemplo, consulte el tutorial [Cambio de tamaño de una imagen con Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).

Utilice Event Grid en lugar del desencadenador de Blob Storage en los escenarios siguientes:

* Cuentas de Almacenamiento de blobs
* Gran escala
* Minimización de la latencia

### <a name="blob-storage-accounts"></a>Cuentas de Almacenamiento de blobs

Las [cuentas de Blob Storage](../storage/common/storage-account-overview.md#types-of-storage-accounts) se admiten para enlaces de entrada y salida de blobs, pero no para desencadenadores de blobs. Los desencadenadores de Blob Storage necesitan una cuenta de almacenamiento de uso general.

### <a name="high-scale"></a>Gran escala

Gran escala se aplica generalmente a contenedores que tienen más de 100 000 blobs en ellos o a cuentas de almacenamiento que tienen más de 100 actualizaciones de blob por segundo.

### <a name="latency-issues"></a>Problemas de latencia

Si la aplicación de función está en un plan de consumo, puede haber un retraso de hasta 10 minutos en el procesamiento de nuevos blobs si una aplicación de función ha quedado inactiva. Para evitar esta latencia, puede cambiar a un plan de App Service con Always On habilitado. También puede usar un [desencadenador de Event Grid](functions-bindings-event-grid.md) con su cuenta de almacenamiento de blobs. Para ver un ejemplo, consulte el [tutorial de Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json). 

### <a name="queue-storage-trigger"></a>Desencadenador de Queue Storage

Además de Event Grid, otra alternativa para el procesamiento de blobs es el desencadenador de Queue Storage, pero no tiene ninguna compatibilidad integrada para los eventos de blobs. Debería crear mensajes en cola al crear o actualizar blobs. Para ver un ejemplo que presupone que ya ha realizado eso, consulte el [ejemplo de enlaces de entrada de blob más adelante en este artículo](#input---example).

## <a name="trigger---example"></a>Desencadenador: ejemplo

# <a name="c"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que escribe un registro cuando se agrega o se actualiza un blob en el contenedor `samples-workitems`.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#trigger---blob-name-patterns) que aparece más adelante en este artículo.

Para obtener más información sobre el atributo `BlobTrigger`, consulte [Desencadenador: atributos](#trigger---attributes).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

En el siguiente ejemplo se muestra un enlace de desencadenador de blob en un archivo *function.json* y el código que usa el enlace. La función escribe un registro cuando se agrega o actualiza un blob en el [contenedor](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) `samples-workitems`.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#trigger---blob-name-patterns) que aparece más adelante en este artículo.

Para más información sobre las propiedades del archivo *function.json*, consulte la sección [Configuración](#trigger---configuration) donde se explican estas propiedades.

Este es el código de script de C# que se enlaza a `Stream`:

```cs
public static void Run(Stream myBlob, string name, ILogger log)
{
   log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Este es el código de script de C# que se enlaza a `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador de blob en un archivo *function.json* y el [código de JavaScript](functions-reference-node.md) que usa el enlace. La función escribe un registro cuando se agrega o actualiza un blob en el contenedor `samples-workitems`.

Este es el archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#trigger---blob-name-patterns) que aparece más adelante en este artículo.

Para más información sobre las propiedades del archivo *function.json*, consulte la sección [Configuración](#trigger---configuration) donde se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra un enlace de desencadenador de blobs en un archivo *function.json* y el [código de Python](functions-reference-python.md) que usa el enlace. La función escribe un registro cuando se agrega o actualiza un blob en el [contenedor](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) `samples-workitems`.

Este es el archivo *function.json*:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,
    "bindings": [
        {
            "name": "myblob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#trigger---blob-name-patterns) que aparece más adelante en este artículo.

Para más información sobre las propiedades del archivo *function.json*, consulte la sección [Configuración](#trigger---configuration) donde se explican estas propiedades.

Este es el código de Python:

```python
import logging
import azure.functions as func


def main(myblob: func.InputStream):
    logging.info('Python Blob trigger function processed %s', myblob.name)
```

# <a name="java"></a>[Java](#tab/java)

Esta función escribe un registro cuando se agrega o actualiza un blob en el contenedor `myblob`.

```java
@FunctionName("blobprocessor")
public void run(
  @BlobTrigger(name = "file",
               dataType = "binary",
               path = "myblob/{name}",
               connection = "MyStorageAccountAppSetting") byte[] content,
  @BindingName("name") String filename,
  final ExecutionContext context
) {
  context.getLogger().info("Name: " + filename + " Size: " + content.length + " bytes");
}
```

---

## <a name="trigger---attributes"></a>Desencadenador: atributos

# <a name="c"></a>[C#](#tab/csharp)

Para [bibliotecas de clases de C#](functions-dotnet-class-library.md), utilice los siguientes atributos para configurar un desencadenador de blob:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobTriggerAttribute.cs)

  El constructor del atributo toma una cadena de ruta de acceso que indica al contenedor que inspeccione y, opcionalmente, un [patrón de nombre de blob](#trigger---blob-name-patterns). Este es un ejemplo:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
   ```

  Para obtener un ejemplo completo, vea el [ejemplo de desencadenador](#trigger---example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de almacenamiento que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de almacenamiento. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ....
  }
  ```

La cuenta de almacenamiento que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `BlobTrigger`.
* El atributo `StorageAccount` aplicado al mismo parámetro que el atributo `BlobTrigger`.
* El atributo `StorageAccount` aplicado a la función.
* El atributo `StorageAccount` aplicado a la clase.
* La cuenta de almacenamiento predeterminada de la aplicación de función (configuración de aplicación "AzureWebJobsStorage").

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobTrigger` se usa para facilitar el acceso al blob que desencadenó la función. Vea el [ejemplo de desencadenador](#trigger---example) para más información.

---

## <a name="trigger---configuration"></a>Desencadenador: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `BlobTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `blobTrigger`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**direction** | N/D | Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. Las excepciones se indican en la sección [uso](#trigger---usage). |
|**name** | N/D | Nombre de la variable que representa el blob en el código de la función. |
|**path** | **BlobPath** |El [contenedor](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) que se va a supervisar.  Puede ser un [patrón de nombre de blob](#trigger---blob-name-patterns). |
|**connection** | **Connection** | El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno de ejecución de Functions busca una configuración de aplicación denominada "MyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.<br><br>La cadena de conexión debe ser para una cuenta de almacenamiento de uso general, no una [cuenta de Blob Storage](../storage/common/storage-account-overview.md#types-of-storage-accounts).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Desencadenador: uso

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Acceda a los datos de BLOB mediante `context.bindings.<NAME>` donde `<NAME>` coincide con el valor definido en *function.json*.

# <a name="python"></a>[Python](#tab/python)

Acceda a los datos de blob a través del parámetro con el tipo [InputStream](https://docs.microsoft.com/python/api/azure-functions/azure.functions.inputstream?view=azure-python). Vea el [ejemplo de desencadenador](#trigger---example) para más información.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobTrigger` se usa para facilitar el acceso al blob que desencadenó la función. Vea el [ejemplo de desencadenador](#trigger---example) para más información.

---

## <a name="trigger---blob-name-patterns"></a>Desencadenador: patrones de nombre de blob

Puede especificar un patrón de nombre de blob en la propiedad `path` en *function.json* o en el constructor de atributos `BlobTrigger`. El patrón de nombre puede ser una [expresión de filtro o enlace](./functions-bindings-expressions-patterns.md). En las siguientes secciones se proporcionan ejemplos.

### <a name="get-file-name-and-extension"></a>Obtener el nombre y la extensión de archivo

En el ejemplo siguiente se muestra cómo enlazar a nombre de archivo del blob y la extensión por separado:

```json
"path": "input/{blobname}.{blobextension}",
```

Si el blob se denomina *original-Blob1.txt*, los valores de las variables `blobname` y `blobextension` del código de la función son *original-Blob1* y *txt*.

### <a name="filter-on-blob-name"></a>Filtrar por nombre de blob

El ejemplo siguiente se desencadena solo en blobs del contenedor `input` que comienzan con la cadena "original-":

```json
"path": "input/original-{name}",
```

Si el nombre de blob es *original Blob1.txt*, el valor de la variable `name` en el código de la función es `Blob1`.

### <a name="filter-on-file-type"></a>Filtrar por tipo de archivo

El siguiente ejemplo se desencadena solo en archivos *.png*:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filtrar por llaves en nombres de archivo

Para buscar llaves en nombres de archivo, escape las llaves mediante dos llaves. En el ejemplo siguiente se filtran blobs que tienen llaves en el nombre:

```json
"path": "images/{{20140101}}-{name}",
```

Si el blob se denomina *{20140101}-soundfile.mp3*, el valor de variable `name` en el código de la función es *soundfile.mp3*.

## <a name="trigger---metadata"></a>Desencadenador: metadatos

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

En Python no hay metadatos disponibles.

# <a name="java"></a>[Java](#tab/java)

En Java no hay metadatos disponibles.

---

## <a name="trigger---blob-receipts"></a>Desencadenador: recepciones de blobs

El entorno en tiempo de ejecución de Azure Functions garantiza que no se llame más de una vez a ninguna función de desencadenador de blobs para un mismo blob, ya sea nuevo o actualizado. Mantiene *recepciones de blobs* para determinar si se ha procesado una determinada versión de blob.

Azure Functions almacena confirmaciones de blobs en un contenedor llamado *azure-webjobs-hosts* en la cuenta de almacenamiento de Azure de la aplicación de función (que se especifica mediante la configuración de la aplicación `AzureWebJobsStorage`). Una recepción de blobs tiene la información siguiente:

* La función desencadenada (" *&lt;nombre de aplicación de función>* .Functions. *&lt;nombre de función>* ", por ejemplo: "MyFunctionApp.Functions.CopyBlob")
* El nombre del contenedor
* El tipo de blob ("BlockBlob" o "PageBlob")
* El nombre del blob
* ETag (un identificador de la versión del blob, por ejemplo: "0x8D1DC6E70A277EF")

Si desea forzar el reprocesamiento de un blob, puede eliminar manualmente la recepción de ese blob desde el contenedor *azure-webjobs-hosts* . Al volver a procesar podría no producirse inmediatamente, pero se garantiza que se producirá más adelante en un momento dado.

## <a name="trigger---poison-blobs"></a>Desencadenador: blobs dudosos

Si se produce un error en una función de desencadenador de blob, Azure Functions vuelve a intentar ejecutar esa función hasta 5 veces de forma predeterminada.

Si se produce un error en los 5 intentos, Azure Functions agregará un mensaje a una cola de Storage llamada *webjobs-blobtrigger-poison*. El mensaje de cola para los blobs dudosos es un objeto JSON que contiene las siguientes propiedades:

* FunctionId (con el formato *&lt;nombre de aplicación de función>* .Functions. *&lt;nombre de función>* )
* BlobType ("BlockBlob" o "PageBlob")
* ContainerName
* BlobName
* ETag (un identificador de la versión del blob, por ejemplo: "0x8D1DC6E70A277EF")

## <a name="trigger---concurrency-and-memory-usage"></a>Desencadenador: uso de simultaneidad y memoria

El desencadenador de blob utiliza una cola internamente, por lo que el número máximo de invocaciones de funciones simultáneas lo controla la [configuración de colas en host.json](functions-host-json.md#queues). La configuración predeterminada limita la simultaneidad a 24 invocaciones. Este límite se aplica por separado a cada función que usa un desencadenador de blob.

[El plan de consumo](functions-scale.md#how-the-consumption-and-premium-plans-work) limita una aplicación de función de una máquina virtual (VM) a 1,5 GB de memoria. Tanto las instancias de función que se ejecutan de forma simultánea como el entorno de ejecución de Functions utilizan la memoria. Si una función desencadenada por un blob carga el blob entero a la memoria, la memoria máxima utilizada por esa función solo para blobs tiene un tamaño máximo de blob de 24 *. Por ejemplo, una aplicación de función con tres funciones desencadenadas por un blob y la configuración predeterminada tendrían una simultaneidad máxima por máquina virtual de 3*24 = 72 invocaciones de función.

Las funciones de JavaScript y Java cargan el blob entero a la memoria, mientras que las funciones de C# lo hacen si establece un enlace a `string`, `Byte[]` o POCO.

## <a name="trigger---polling"></a>Desencadenador: sondeo

El sondeo funciona como un híbrido entre la inspección de registros y la ejecución de exámenes periódicos de contenedores. Los blobs se examinan en grupos de 10 000 a la vez con un token de continuación que se usa entre intervalos.

> [!WARNING]
> Además, [se crean registros de almacenamiento basados en el principio del "mejor esfuerzo"](/rest/api/storageservices/About-Storage-Analytics-Logging). No hay ninguna garantía de que se capturen todos los eventos. En algunos casos, podrían faltar registros.
> 
> Si necesita un procesamiento de blobs más rápido o confiable, considere crear un [mensaje de cola](../storage/queues/storage-dotnet-how-to-use-queues.md) al crear el blob. A continuación, use un [desencadenador de cola](functions-bindings-storage-queue.md), en lugar de un desencadenador de blob, para procesar el blob. Otra opción consiste en usar Event Grid; consulte el tutorial [Automatizar el cambio de tamaño de imágenes cargadas mediante Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).
>

## <a name="input"></a>Entrada

Utilice un enlace de entrada de almacenamiento de blobs para leer blobs.

## <a name="input---example"></a>Ejemplo de entrada

# <a name="c"></a>[C#](#tab/csharp)

El ejemplo siguiente es una [función de C#](functions-dotnet-class-library.md) que utiliza un desencadenador de cola y un enlaces de blobs de entrada. El mensaje de cola contiene el nombre del blob y la función registra el tamaño del blob.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y código de [script de C# (.csx)](functions-reference-csharp.md) que usa los enlaces. La función realiza una copia de un blob de texto. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de script de C#:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y código de [script de JavaScript](functions-reference-node.md) que usa los enlaces. La función realiza una copia de un blob. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y el [código de Python](functions-reference-python.md) que usa los enlaces. La función realiza una copia de un blob. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de Python:

```python
import logging
import azure.functions as func


def main(queuemsg: func.QueueMessage, inputblob: func.InputStream) -> func.InputStream:
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    return inputblob
```

# <a name="java"></a>[Java](#tab/java)

En esta sección se incluyen los ejemplos siguientes:

* [Desencadenador de HTTP, búsqueda de nombre de blob en la cadena de consulta](#http-trigger-look-up-blob-name-from-query-string)
* [Desencadenador de cola, recepción del nombre del blob de la cola de mensajes](#queue-trigger-receive-blob-name-from-queue-message)

#### <a name="http-trigger-look-up-blob-name-from-query-string"></a>Desencadenador de HTTP, búsqueda de nombre de blob en la cadena de consulta

 El ejemplo siguiente muestra una función de Java que usa la anotación `HttpTrigger` para recibir un parámetro que contiene el nombre de un archivo en un contenedor de Blob Storage. Posteriormente, la anotación `BlobInput` lee el archivo y pasa el contenido a la función como `byte[]`.

```java
  @FunctionName("getBlobSizeHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage blobSize(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    final ExecutionContext context) {
      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-receive-blob-name-from-queue-message"></a>Desencadenador de cola, recepción del nombre del blob de la cola de mensajes

 El ejemplo siguiente muestra una función de Java que usa la anotación `QueueTrigger` para recibir un mensaje que contiene el nombre de un archivo en un contenedor de Blob Storage. Posteriormente, la anotación `BlobInput` lee el archivo y pasa el contenido a la función como `byte[]`.

```java
  @FunctionName("getBlobSize")
  @StorageAccount("Storage_Account_Connection_String")
  public void blobSize(
    @QueueTrigger(
      name = "filename", 
      queueName = "myqueue-items-sample") 
    String filename,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{queueTrigger}") 
    byte[] content,
    final ExecutionContext context) {
      context.getLogger().info("The size of \"" + filename + "\" is: " + content.length + " bytes");
  }
```

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@BlobInput` en los parámetros cuyo valor provendría de un blob.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante `Optional<T>`.

---

## <a name="input---attributes"></a>Entrada: atributos

# <a name="c"></a>[C#](#tab/csharp)

En las [bibliotecas de clase C#](functions-dotnet-class-library.md), use [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

El constructor del atributo toma la ruta de acceso al blob y un parámetro `FileAccess` que indica lectura o escritura, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

Puede usar el atributo `StorageAccount` para especificar la cuenta de almacenamiento en el nivel de clase, método o parámetro. Para obtener más información, consulte [Desencadenador: atributos](#trigger---attributes).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobInput` da acceso al blob que desencadenó la función. Si utiliza una matriz de bytes con el atributo, establezca `dataType` en `binary`. Vea el [ejemplo de entrada](#input---example) para más información.

---

## <a name="input---configuration"></a>Entrada: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `Blob`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `blob`. |
|**direction** | N/D | Se debe establecer en `in`. Las excepciones se indican en la sección [uso](#input---usage). |
|**name** | N/D | Nombre de la variable que representa el blob en el código de la función.|
|**path** |**BlobPath** | Ruta de acceso al blob. |
|**connection** |**Connection**| El nombre de una configuración de aplicación que contiene la [cadena de conexión de almacenamiento](../storage/common/storage-configure-connection-string.md) que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno de ejecución de Functions busca una configuración de aplicación denominada "MyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.<br><br>La cadena de conexión debe ser para una cuenta de almacenamiento de uso general, no una [cuenta de almacenamiento solo de blob](../storage/common/storage-account-overview.md#types-of-storage-accounts).|
|N/D | **Acceder** | Indica si va a leer o escribir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Uso de entradas

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-input-usage.md](../../includes/functions-bindings-blob-storage-input-usage.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-input-usage.md](../../includes/functions-bindings-blob-storage-input-usage.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Acceda a los datos de BLOB mediante `context.bindings.<NAME>` donde `<NAME>` coincide con el valor definido en *function.json*.

# <a name="python"></a>[Python](#tab/python)

Acceda a los datos de blob a través del parámetro con el tipo [InputStream](https://docs.microsoft.com/python/api/azure-functions/azure.functions.inputstream?view=azure-python). Vea el [ejemplo de entrada](#input---example) para más información.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobInput` da acceso al blob que desencadenó la función. Si utiliza una matriz de bytes con el atributo, establezca `dataType` en `binary`. Vea el [ejemplo de entrada](#input---example) para más información.

---

## <a name="output"></a>Output

Use enlaces de salida de almacenamiento de blobs para escribir blobs.

## <a name="output---example"></a>Salida: ejemplo

# <a name="c"></a>[C#](#tab/csharp)

El ejemplo siguiente es una [función de C#](functions-dotnet-class-library.md) que utiliza un desencadenador de blobs y dos enlaces de blobs de salida. La función se activa por la creación de un blob de imágenes en el contenedor *sample-images*. Crea copias pequeñas y medianas del blob de imágenes.

```csharp
using System.Collections.Generic;
using System.IO;
using Microsoft.Azure.WebJobs;
using SixLabors.ImageSharp;
using SixLabors.ImageSharp.Formats;
using SixLabors.ImageSharp.PixelFormats;
using SixLabors.ImageSharp.Processing;

public class ResizeImages
{
    [FunctionName("ResizeImage")]
    public static void Run([BlobTrigger("sample-images/{name}")] Stream image,
        [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall,
        [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
    {
        IImageFormat format;

        using (Image<Rgba32> input = Image.Load<Rgba32>(image, out format))
        {
            ResizeImage(input, imageSmall, ImageSize.Small, format);
        }

        image.Position = 0;
        using (Image<Rgba32> input = Image.Load<Rgba32>(image, out format))
        {
            ResizeImage(input, imageMedium, ImageSize.Medium, format);
        }
    }

    public static void ResizeImage(Image<Rgba32> input, Stream output, ImageSize size, IImageFormat format)
    {
        var dimensions = imageDimensionsTable[size];

        input.Mutate(x => x.Resize(dimensions.Item1, dimensions.Item2));
        input.Save(output, format);
    }

    public enum ImageSize { ExtraSmall, Small, Medium }

    private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
        { ImageSize.ExtraSmall, (320, 200) },
        { ImageSize.Small,      (640, 400) },
        { ImageSize.Medium,     (800, 600) }
    };

}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y código de [script de C# (.csx)](functions-reference-csharp.md) que usa los enlaces. La función realiza una copia de un blob de texto. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de script de C#:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y código de [script de JavaScript](functions-reference-node.md) que usa los enlaces. La función realiza una copia de un blob. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y el [código de Python](functions-reference-python.md) que usa los enlaces. La función realiza una copia de un blob. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "outputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de Python:

```python
import logging
import azure.functions as func


def main(queuemsg: func.QueueMessage, inputblob: func.InputStream,
         outputblob: func.Out[func.InputStream]):
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    outputblob.set(inputblob)
```

# <a name="java"></a>[Java](#tab/java)

En esta sección se incluyen los ejemplos siguientes:

* [Desencadenador HTTP, uso de OutputBinding](#http-trigger-using-outputbinding-java)
* [Desencadenador de cola, uso del valor devuelto de la función](#queue-trigger-using-function-return-value-java)

#### <a name="http-trigger-using-outputbinding-java"></a>Desencadenador HTTP, uso de OutputBinding (Java)

 El ejemplo siguiente muestra una función de Java que usa la anotación `HttpTrigger` para recibir un parámetro que contiene el nombre de un archivo en un contenedor de Blob Storage. Posteriormente, la anotación `BlobInput` lee el archivo y pasa el contenido a la función como `byte[]`. La anotación `BlobOutput` enlaza a `OutputBinding outputItem`. La función usa este posteriormente para escribir el contenido del blob de entrada en el contenedor de almacenamiento configurado.

```java
  @FunctionName("copyBlobHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage copyBlobHttp(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    @BlobOutput(
      name = "target", 
      path = "myblob/{Query.file}-CopyViaHttp")
    OutputBinding<String> outputItem,
    final ExecutionContext context) {
      // Save blob to outputItem
      outputItem.setValue(new String(content, StandardCharsets.UTF_8));

      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-using-function-return-value-java"></a>Desencadenador de cola, uso del valor devuelto de la función (Java)

 El ejemplo siguiente muestra una función de Java que usa la anotación `QueueTrigger` para recibir un mensaje que contiene el nombre de un archivo en un contenedor de Blob Storage. Posteriormente, la anotación `BlobInput` lee el archivo y pasa el contenido a la función como `byte[]`. La anotación `BlobOutput` enlaza al valor devuelto de la función. El entorno de ejecución usa este valor posteriormente para escribir el contenido del blob de entrada en el contenedor de almacenamiento configurado.

```java
  @FunctionName("copyBlobQueueTrigger")
  @StorageAccount("Storage_Account_Connection_String")
  @BlobOutput(
    name = "target", 
    path = "myblob/{queueTrigger}-Copy")
  public String copyBlobQueue(
    @QueueTrigger(
      name = "filename", 
      dataType = "string",
      queueName = "myqueue-items") 
    String filename,
    @BlobInput(
      name = "file", 
      path = "samples-workitems/{queueTrigger}") 
    String content,
    final ExecutionContext context) {
      context.getLogger().info("The content of \"" + filename + "\" is: " + content);
      return content;
  }
```

 En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@BlobOutput` en los parámetros de función cuyo valor se escribiría en un objeto del almacenamiento de blobs.  El parámetro type debe ser `OutputBinding<T>`, donde T es cualquier tipo nativo de Java o un POJO.

---

## <a name="output---attributes"></a>Salida: atributos

# <a name="c"></a>[C#](#tab/csharp)

En las [bibliotecas de clase C#](functions-dotnet-class-library.md), use [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

El constructor del atributo toma la ruta de acceso al blob y un parámetro `FileAccess` que indica lectura o escritura, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobOutput` da acceso al blob que desencadenó la función. Si utiliza una matriz de bytes con el atributo, establezca `dataType` en `binary`. Vea el [ejemplo de salida](#output---example) para más información.

---

Para obtener un ejemplo completo, vea el [ejemplo de salida](#output---example).

Puede usar el atributo `StorageAccount` para especificar la cuenta de almacenamiento en el nivel de clase, método o parámetro. Para obtener más información, consulte [Desencadenador: atributos](#trigger---attributes).

## <a name="output---configuration"></a>Salida: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `Blob`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `blob`. |
|**direction** | N/D | Debe establecerse en `out` para un enlace de salida. Las excepciones se indican en la sección [uso](#output---usage). |
|**name** | N/D | Nombre de la variable que representa el blob en el código de la función.  Se establece en `$return` para hacer referencia al valor devuelto de la función.|
|**path** |**BlobPath** | Ruta de acceso al contenedor de blobs. |
|**connection** |**Connection**| El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno de ejecución de Functions busca una configuración de aplicación denominada "MyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.<br><br>La cadena de conexión debe ser para una cuenta de almacenamiento de uso general, no una [cuenta de almacenamiento solo de blob](../storage/common/storage-account-overview.md#types-of-storage-accounts).|
|N/D | **Acceder** | Indica si va a leer o escribir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uso de salidas

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En JavaScript, acceda a los datos de blob mediante `context.bindings.<name from function.json>`.

# <a name="python"></a>[Python](#tab/python)

Puede declarar parámetros de función como los siguientes tipos para escribir en el almacenamiento de blobs:

* Cadenas como `func.Out(str)`
* Secuencias como `func.Out(func.InputStream)`

Vea el [ejemplo de salida](#output---example) para más información.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobOutput` da acceso al blob que desencadenó la función. Si utiliza una matriz de bytes con el atributo, establezca `dataType` en `binary`. Vea el [ejemplo de salida](#output---example) para más información.

---

## <a name="exceptions-and-return-codes"></a>Excepciones y códigos de retorno

| Enlace |  Referencia |
|---|---|
| Blob | [Códigos de error de blobs](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| Blob, tabla, cola |  [Códigos de error de almacenamiento](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, tabla, cola |  [Solución de problemas](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Blob storage trigger](functions-create-storage-blob-triggered-function.md)
--->
