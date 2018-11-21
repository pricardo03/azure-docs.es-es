---
title: Referencia de Azure Functions para desarrolladores de Java | Microsoft Docs
description: Aprenda a desarrollar funciones con Java.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure Functions, funciones, procesamiento de eventos, webhooks, proceso dinámico, arquitectura sin servidor, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: routlaw
ms.openlocfilehash: 5f74ee390ac327a9e697d3dc67da4ea604b64d69
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686899"
---
# <a name="azure-functions-java-developer-guide"></a>Guía de Azure Functions para desarrolladores de Java

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Modelo de programación 

Los conceptos de [desencadenadores y enlaces](functions-triggers-bindings.md) son fundamentales en Azure Functions. Los desencadenadores inician la ejecución del código. Los enlaces, por otro lado, proporcionan una manera de pasar y devolver datos de una función, sin tener que escribir un código de acceso a datos personalizados.

Una función debe ser un método sin estado para poder procesar la entrada y generar el resultado. Además, la función no debe depender de los campos de instancia de la clase. Todos los métodos de función deben ser `public` y el método que cuenta con la anotación @FunctionName debe ser único, como el nombre del método que define la entrada de una función.

## <a name="folder-structure"></a>Estructura de carpetas

Esta es la estructura de carpetas de un proyecto de Java de una función de Azure:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

Hay un archivo [host.json](functions-host-json.md) compartido que se puede usar para configurar la aplicación de función. Cada función tiene su propio archivo de código (.java) y archivo de configuración de enlace (function.json).

Puede colocar más de una función en un proyecto. Evite colocar las funciones en archivos JAR independientes. La aplicación de función del directorio de destino es lo que se implementa en la aplicación de función en Azure.

## <a name="triggers-and-annotations"></a>Desencadenadores y anotaciones

 Las funciones de Azure se invocan mediante un desencadenador, como una solicitud HTTP, un temporizador o una actualización de datos. La función debe procesar ese desencadenador y las demás entradas para generar una o más salidas.

Utilice las anotaciones de Java incluidas en el paquete [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) para enlazar las entradas y salidas a los métodos. Para obtener más información, consulte los [documentos de referencia de Java](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Debe configurar una cuenta de Azure Storage en [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) para ejecutar los desencadenadores de Azure Blob/Queue/Table Storage de manera local.

Ejemplo:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Aquí está el elemento `function.json` correspondiente que generó [azure-functions-maven-plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin):

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="jdk-runtime-availability-and-support"></a>Disponibilidad y soporte técnico del entorno de ejecución de JDK 

Descargue y utilice los JDK de [Azul Zulu for Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) de [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) para el desarrollo local de aplicaciones de función Java. Los JDK están disponibles para Windows, Linux y macOS. El [Soporte técnico de Azure](https://support.microsoft.com/en-us/help/4026305/sql-contact-microsoft-azure-support) está disponible con un [plan de soporte técnico completo](https://azure.microsoft.com/support/plans/).

## <a name="third-party-libraries"></a>Bibliotecas de terceros 

Azure Functions admite el uso de bibliotecas de terceros. De forma predeterminada, todas las dependencias especificadas en el archivo `pom.xml` del proyecto se incluirán automáticamente durante el objetivo de [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage). Para las bibliotecas no especificadas como dependencias en el archivo `pom.xml`, colóquelas en un directorio `lib` del directorio raíz de la función. Las dependencias colocadas en el directorio `lib` se agregarán al cargador de clases del sistema en tiempo de ejecución.

La dependencia `com.microsoft.azure.functions:azure-functions-java-library` se proporciona en la ruta de acceso de la clase de forma predeterminada y no deben incluirse en el directorio `lib`. Asimismo, las dependencias que se enumeran [aquí](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) se agregan a la ruta de clase gracias a [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker).

## <a name="data-type-support"></a>Compatibilidad con los tipos de datos

Puede usar objetos de Java POJO (Plain Old Java Object), tipos definidos en `azure-functions-java-library` o tipos de datos primitivos como String, Integer para enlazar con los enlaces de entrada o salida.

### <a name="plain-old-java-objects-pojos"></a>Objetos POJO

Para convertir los datos de entrada a POJO, [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) usa la biblioteca [gson](https://github.com/google/gson). Los tipos de POJO que se usan como entradas para las funciones deben ser `public`.

### <a name="binary-data"></a>Datos binarios

Enlace entradas o salidas binarias a `byte[]`; para ello, establezca el campo `dataType` en function.json como `binary`:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Use `Optional<T>` por si se esperan valores NULL.

## <a name="bindings"></a>Enlaces

Los enlaces de entrada y de salida permiten conectarse de manera declarativa a los datos desde el código. Cada función puede tener varios enlaces de entrada y de salida.

### <a name="example-input-binding"></a>Ejemplo de un enlace de entrada

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Esta función se invoca con una solicitud HTTP. 
- La carga útil de la solicitud HTTP se pasa como `String` para el argumento `inputReq`.
- Una entrada se recupera de Azure Table Storage y se pasa como `TestInputData` al argumento `inputData`.

Para recibir un lote de entradas, puede enlazarse a `String[]`, `POJO[]`, `List<String>` o `List<POJO>`.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Esta función se desencadena cuando hay nuevos datos en el centro de eventos configurado. Como `cardinality` se establece en `MANY`, la función recibe un lote de mensajes desde el centro de eventos. El elemento EventData del centro de eventos se convierte a `TestEventData` para la ejecución de la función.

### <a name="example-output-binding"></a>Ejemplo de enlace de salida

Puede enlazar un enlace de salida al valor de retorno con `$return`. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Si hay varios enlaces de salida, use el valor devuelto para solo uno de ellos.

Para enviar varios valores de salida, use el tipo `OutputBinding<T>` que se define en el paquete `azure-functions-java-library`. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

La función anterior se invoca en un elemento HttpRequest y escribe varios valores en la cola de Azure.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage y HttpResponseMessage

 Los tipos HttpRequestMessage y HttpResponseMessage que se definen en `azure-functions-java-library` son tipos de ayuda que le permitirán trabajar con las funciones HttpTrigger.

| Tipo especializado      |       Destino        | Uso típico                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    Desencadenador HTTP     | Método get, encabezados o consultas |
| `HttpResponseMessage` | Enlace de salida HTTP | Estado de retorno distinto de 200   |

## <a name="metadata"></a>Metadatos

Algunos desencadenadores envían [metadatos de desencadenador](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties) junto con datos de entrada. Puede usar la anotación `@BindingName` para enlazarla a los metadatos de desencadenador.


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
En el ejemplo anterior, el elemento `queryValue` está enlazado al parámetro de cadena de consulta `name` de la dirección URL de solicitud de HTTP `http://{example.host}/api/metadata?name=test`. A continuación, se muestra otro ejemplo para enlazarse a `Id` desde los metadatos de desencadenador de la cola.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " whith metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> El nombre que se proporciona en la anotación debe coincidir con la propiedad de metadatos.

## <a name="execution-context"></a>Contexto de ejecución

El elemento `ExecutionContext` definido en `azure-functions-java-library` contiene métodos de ayuda que le permitirán comunicarse con las funciones en tiempo de ejecución.

### <a name="logger"></a>Registrador

Use `getLogger`, que está definido en `ExecutionContext`, para escribir registros desde el código de función.

Ejemplo:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Ver registros y seguimiento

Puede usar la CLI de Azure para hacer streaming del registro stdout y stderr de Java, así como de otros registros de la aplicación. 

Configure la aplicación de función para escribir el registro de aplicación con la CLI de Azure:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Para transmitir en secuencias la salida para la aplicación de función con la CLI de Azure, abra una nueva sesión del símbolo del sistema, Bash o Terminal y escriba el siguiente comando:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
El comando [az webapp log tail](/cli/azure/webapp/log) tiene opciones para filtrar la salida usando la opción `--provider`. 

Para descargar los archivos de registro como un solo archivo ZIP mediante la CLI de Azure, abra una sesión nueva del símbolo del sistema, Bash o Terminal y escriba el siguiente comando:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Debe haber habilitado el registro del sistema de archivos en Azure Portal o la CLI de Azure antes de ejecutar este comando.

## <a name="environment-variables"></a>Variables de entorno

En Functions, [la configuración de la aplicación](https://docs.microsoft.com/en-us/azure/azure-functions/functions-app-settings), como las cadenas de conexión del servicio, se exponen como variables de entorno durante la ejecución. Puede obtener acceso a estas opciones mediante, `System.getenv("AzureWebJobsStorage")`.

Ejemplo:

Agregue [AppSetting](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings) con el nombre testAppSetting y el valor testAppSettingValue.

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("testAppSetting "+ System.getenv("testAppSettingValue"));
        return String.format(req);
    }
}

```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca del desarrollo de Java de Azure Fuctions, consulte los siguientes recursos:

* [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
* [Referencia para desarrolladores de Azure Functions](functions-reference.md)
* [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
- Desarrollo y depuración local con [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) y [Eclipse](functions-create-maven-eclipse.md). 
* [Depuración remota de Azure Functions en Java con Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Complemento Maven para Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md): optimización de la creación de funciones a través del objetivo `azure-functions:add` y preparación de un directorio de almacenamiento provisional para la [implementación del archivo ZIP](deployment-zip-push.md).
