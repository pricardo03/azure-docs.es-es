---
title: Creación de la primera función en Azure con Java y Maven | Microsoft Docs
description: Cree y publique una función simple desencadenada por HTTP en Azure con Java y Maven.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure functions, funciones, procesamiento de eventos, proceso, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: a72d6b180db35f3e0f0e0527e8ae0f544a585b25
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822980"
---
# <a name="create-your-first-function-with-java-and-maven"></a>Creación de la primera función con Java y Maven

Este artículo le guiará con el uso de la herramienta de la línea de comandos de Maven para compilar y publicar una función de Java en Azure Functions. Cuando haya terminado, el código de la función se ejecuta en el [Plan de consumo](functions-scale.md#consumption-plan) en Azure y puede activarse mediante una solicitud HTTP.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para desarrollar funciones con Java, debe tener instalado lo siguiente:

- [Kit para desarrolladores de Java](https://www.azul.com/downloads/zulu/), versión 8.
- [Apache Maven](https://maven.apache.org), versión 3.0 o posterior.
- [CLI de Azure](https://docs.microsoft.com/cli/azure)
- [Azure Functions Core Tools](functions-run-local.md#v2) (requiere el **SDK de .NET Core 2.x**)

> [!IMPORTANT]
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar esta guía de inicio rápido.

## <a name="generate-a-new-functions-project"></a>Generación de un nuevo proyecto de Functions

En una carpeta vacía, ejecute el comando siguiente para generar el proyecto de Functions desde un [arquetipo Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven le pedirá los valores necesarios para acabar de generar el proyecto. Para conocer los valores _groupId_, _artifactId_ y _version_, consulte la referencia sobre las [convenciones de nomenclatura de Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html). El valor _appName_ debe ser único en todo Azure, por lo que Maven genera un nombre de aplicación en función del valor _artifactId_ escrito anteriormente como valor predeterminado. El valor _packageName_ determina el paquete de Java para el código de función generado.

Los identificadores `com.fabrikam.functions` y `fabrikam-functions` siguientes se utilizan como ejemplo y para facilitar la lectura de los pasos posteriores de esta guía de inicio rápido. En este paso, se recomienda especificar los valores propios en Maven.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

Maven crea los archivos del proyecto en una carpeta nueva llamada _artifactId_, en este ejemplo `fabrikam-functions`. El código generado listo para ejecutarse del proyecto es una sencilla función [desencadenada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) que devuelve el cuerpo de la solicitud:

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage<String> hello(
            @HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponse(400, "Please pass a name on the query string or in the request body");
        } else {
            return request.createResponse(200, "Hello, " + name);
        }
    }
}

```

## <a name="run-the-function-locally"></a>Ejecución local de la función

Cambie de directorio a la carpeta de proyecto que acaba de crear y compile y ejecute la función con Maven:

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Si experimenta esta excepción: `javax.xml.bind.JAXBException` con Java 9, consulte la solución alternativa en [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

Esta salida se ve cuando la función se ejecuta localmente en el sistema y está lista para responder a las solicitudes HTTP:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Desencadene la función desde la línea de comandos, para lo que debe usar curl en una ventana de terminal nueva:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

Use `Ctrl-C` en el terminal para detener el código de función.

## <a name="deploy-the-function-to-azure"></a>Implementación de la función en Azure

El proceso de implementación en Azure Functions usa las credenciales de cuenta desde la CLI de Azure. [Inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de continuar.

```azurecli
az login
```

Implemente el código en una aplicación de función nueva `azure-functions:deploy` con el destino de Maven.

```
mvn azure-functions:deploy
```

Cuando se complete la implementación, verá la dirección URL que puede usar para acceder a la aplicación de función de Azure:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Pruebe la aplicación de función que se ejecuta en Azure mediante `cURL`. Es preciso que cambie dirección URL del ejemplo siguiente para coincida con la implementada para su propia aplicación de función en el paso anterior.

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

Edite el archivo de código fuente `src/main.../Function.java` en el proyecto generado para modificar el texto que devuelve la aplicación de función. Cambie esta línea:

```java
return request.createResponse(200, "Hello, " + name);
```

Por lo siguiente:

```java
return request.createResponse(200, "Hi, " + name);
```

Guarde los cambios y repita la implementación mediante la ejecución de `azure-functions:deploy` desde el terminal como antes. La aplicación de función se actualizará y esta solicitud:

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

tendrá una salida actualizada:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Pasos siguientes

Creó una aplicación de función de Java con un desencadenador HTTP simple y la implementó en Azure Functions.

- Revise la [guía para desarrolladores de Java Functions](functions-reference-java.md) para más información sobre cómo desarrollar funciones de Java.
- Agregue funciones adicionales con desencadenadores distintos en el proyecto con el destino de Maven `azure-functions:add`.
- Escriba y depure localmente las funciones con [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) y [Eclipse](functions-create-maven-eclipse.md). 
- Depure las funciones implementadas en Azure con Visual Studio Code. Consulte la documentación sobre las [aplicaciones Java sin servidor](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) de Visual Studio Code para instrucciones.
