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
ms.date: 08/10/2018
ms.author: routlaw
ms.openlocfilehash: f0dc471e8875ad0d738fce10421c3586752148b9
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092316"
---
# <a name="azure-functions-java-developer-guide"></a>Guía de Azure Functions para desarrolladores de Java

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Modelo de programación 

La instancia de Azure Functions debe ser un método de clase sin estado que procese entradas y produzca salidas. Aunque puede escribir métodos de instancia, la función no debe depender de ningún campo de instancia de la clase. Todos los métodos de la función deben tener un modificador de acceso `public`.

Puede colocar más de una función en un proyecto. Evite colocar las funciones en archivos JAR independientes.

## <a name="triggers-and-annotations"></a>Desencadenadores y anotaciones

 Las funciones de Azure se invocan mediante un desencadenador, como una solicitud HTTP, un temporizador o una actualización de datos. La función debe procesar ese desencadenador y las demás entradas para generar una o más salidas.

Utilice las anotaciones de Java incluidas en el paquete [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) para enlazar las entradas y salidas a los métodos. Está disponible código de ejemplo con las anotaciones en los [documentos de referencia de Java](/java/api/com.microsoft.azure.functions.annotation) para cada anotación y en la documentación de referencia de enlace de Azure Functions, como la correspondiente a los [desencadenadores HTTP](/azure/azure-functions/functions-bindings-http-webhook).

Las entradas y salidas de los desencadenadores también se definen en [function.json](/azure/azure-functions/functions-reference#function-code) para la función en lugar de a través de anotaciones. No se recomienda usar `function.json` en lugar de las anotaciones de este modo.

> [!IMPORTANT] 
> Debe configurar una cuenta de Azure Storage en [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) para ejecutar los desencadenadores de Azure Blob/Queue/Table Storage de manera local.

Ejemplo con anotaciones:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

La misma función escrita sin anotaciones:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

con el archivo `function.json` correspondiente:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
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

## <a name="third-party-libraries"></a>Bibliotecas de terceros 

Azure Functions admite el uso de bibliotecas de terceros. De forma predeterminada, todas las dependencias especificadas en el archivo `pom.xml` del proyecto se incluirán automáticamente durante el objetivo de `mvn package`. Para las bibliotecas no especificadas como dependencias en el archivo `pom.xml`, colóquelas en un directorio `lib` del directorio raíz de la función. Las dependencias colocadas en el directorio `lib` se agregarán al cargador de clases del sistema en tiempo de ejecución.

## <a name="data-type-support"></a>Compatibilidad con los tipos de datos

Puede utilizar todos los tipos de datos de Java para la entrada y salida, incluidos los tipos nativos; los personalizados de Java y los tipos especializados de Azure que se definen en el paquete `azure-functions-java-library`. Los intentos en tiempo de ejecución de Azure Functions convierten la entrada recibida en el tipo que solicita el código.

### <a name="strings"></a>Cadenas

Los valores pasados a métodos de función se convierten a cadenas si el tipo de parámetro de entrada correspondiente para la función es `String`. 

### <a name="plain-old-java-objects-pojos"></a>Objetos POJO

Las cadenas con formato JSON se convierten a tipos de Java si la firma de la entrada de la función espera ese tipo de Java. Esta conversión permite pasar JSON y trabajar con tipos de Java.

Los tipos POJO que se utilizan como entradas para las funciones deben tener el mismo modificador de acceso `public` que los métodos de la función donde se utilizan. No tiene que declarar los campos de la clase POJO `public`. Por ejemplo, una cadena JSON `{ "x": 3 }` puede convertirse al tipo POJO siguiente:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>Datos binarios

Los datos binarios se representan como `byte[]` en el código de las funciones de Azure. Enlace entradas o salidas binarias a las funciones mediante la configuración del campo `dataType` de function.json en `binary`:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

A continuación, utilícelo en el código de la función:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Los valores de entrada vacíos pueden ser `null` como argumento de las funciones, pero una manera recomendada de tratar los posibles valores vacíos consiste en usar `Optional<T>`.


## <a name="function-method-overloading"></a>Sobrecarga de los métodos de función

Se pueden sobrecargar los métodos de función del mismo nombre, pero de distintos tipos. Por ejemplo, puede haber `String echo(String s)` y `String echo(MyType s)` en una clase. Azure Functions decide qué método que se invoca según el tipo de entrada (para la entrada HTTP, el tipo MIME `text/plain` da lugar a `String`, mientras que `application/json` representa `MyType`).

## <a name="inputs"></a>Entradas

Las entradas se dividen en dos categorías Azure Functions: una es la entrada del desencadenador y la otra es una entrada adicional. Aunque son diferentes en `function.json`, se usan igual en el código de Java. Tomemos el siguiente fragmento de código a modo de ejemplo:

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String in,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") MyObject obj
    ) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    public static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Cuando se desencadena esta función, el elemento `String in` pasa la solicitud HTTP a la función. Se recuperará una entrada de Azure Table Storage basada en la id. de la dirección URL de la ruta y estará disponible como `obj` en el cuerpo de la función.

## <a name="outputs"></a>Salidas

Las salidas se expresan como valores devueltos o como parámetros de salida. Si hay una sola salida, se recomienda utilizar el valor devuelto. Para varias salidas, deberá utilizar parámetros de salida.

El valor devuelto es la forma más sencilla de salida, solo devuelve el valor de cualquier tipo y el sistema en tiempo de ejecución de Azure Functions intentará serializar el tipo real (por ejemplo, una respuesta HTTP).  Puede aplicar cualquier anotación de salida al método de función (la propiedad de nombre de la anotación tiene que ser $return) para definir la salida de valor devuelto.

Para generar varios valores de salida, use el tipo `OutputBinding<T>` que se define en el paquete `azure-functions-java-library`. Si necesita realizar una respuesta HTTP e insertar un mensaje en una cola, puede escribir algo similar a lo siguiente:

Por ejemplo, una función de copia de contenido de blob podría definirse como el código siguiente. La anotación `@StorageAccount` se usa aquí para evitar la duplicación de la propiedad de conexión para `@BlobTrigger` y `@BlobOutput`.

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Use `OutputBinding<byte[]`> para crear un valor de salida binario (para los parámetros); para devolver valores, simplemente use `byte[]`.

## <a name="specialized-types"></a>Tipos especializados

En ocasiones, es posible que una función deba controlar minuciosamente las entradas y salidas. Los tipos especializados del paquete `azure-functions-java-core` sirven para manipular la información de la solicitud y adaptar el estado de retorno de un desencadenador HTTP:

| Tipo especializado      |       Destino        | Uso típico                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    Desencadenador HTTP     | Método get, encabezados o consultas |
| `HttpResponseMessage<T>` | Enlace de salida HTTP | Estado de retorno distinto de 200   |

> [!NOTE] 
> También puede usar la anotación `@BindingName` para obtener encabezados y consultas HTTP. Por ejemplo, `@BindingName("name") String query` recorre en iteración los encabezados de solicitud y las consultas HTTP, y pasa ese valor al método. Por ejemplo, `query` será `"test"` si la dirección URL de solicitud es `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Metadatos

Los metadatos proceden de orígenes diferentes, como los encabezados HTTP, las consultas HTTP, y los [metadatos de desencadenador](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Use la anotación `@BindingName` junto con el nombre de metadatos para obtener el valor.

Por ejemplo, `queryValue` en el siguiente fragmento de código será `"test"` si la dirección URL solicitada es `http://{example.host}/api/metadata?name=test`.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="execution-context"></a>Contexto de ejecución

Se interactúa con el entorno de ejecución de Azure Functions a través del objeto `ExecutionContext` que se define en el paquete `azure-functions-java-library`. Utilice el objeto `ExecutionContext` para utilizar la información de invocación y de tiempo de ejecución de las funciones en el código.

### <a name="custom-logging"></a>Registro personalizado

Al registro del tiempo de ejecución de las funciones se accede a través del objeto `ExecutionContext`. Este registrador está asociado al monitor de Azure y permite marcar las advertencias y los errores detectados durante la ejecución de la función.

El código de ejemplo siguiente registra un mensaje de advertencia al recibir un cuerpo de solicitud en blanco.

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

Puede usar la CLI de Azure para transmitir en secuencias el estándar de Java y el registro de errores, así como otros registros de la aplicación. En primer lugar, configure la aplicación de función para escribir el registro de aplicación con la CLI de Azure:

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

Mantenga la información secreta como las claves o los tokens fuera de su código fuente por motivos de seguridad. Para usar claves y tokens en el código de función, puede leerlos de las variables de entorno.

Para establecer las variables de entorno al ejecutar Azure Functions localmente, puede optar por agregar estas variables al archivo local.settings.json. Si no tiene uno en el directorio raíz del proyecto de la función, puede crear uno. Este es el aspecto que debería tener el archivo:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Cada asignación de clave y valor en el mapa `values` se pondrá a disposición en tiempo de ejecución como variable de entorno, a la que podrá tener acceso mediante una llamada a `System.getenv("<keyname>")`, por ejemplo, `System.getenv("AzureWebJobsStorage")`. Agregar pares adicionales de clave/valor es una práctica aceptada y recomendada.

> [!NOTE]
> Si se sigue este enfoque, asegúrese de agregar el archivo local.settings.json al archivo de omisión de repositorio, para que no se confirme.

Ahora que el código depende de estas variables de entorno, puede iniciar sesión en Azure Portal para establecer el mismo par de clave/valor en la configuración de aplicación de la función, para que el código funcione de forma equivalente al probarse localmente y al implementarse en Azure.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca del desarrollo de Java de Azure Fuctions, consulte los siguientes recursos:

* [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
* [Referencia para desarrolladores de Azure Functions](functions-reference.md)
* [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
- Desarrollo y depuración local con [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) y [Eclipse](functions-create-maven-eclipse.md). 
* [Depuración remota de Azure Functions en Java con Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Complemento Maven para Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md): optimización de la creación de funciones a través del objetivo `azure-functions:add` y preparación de un directorio de almacenamiento provisional para la [implementación del archivo ZIP](deployment-zip-push.md).
