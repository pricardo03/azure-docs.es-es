---
title: 'Uso de Java y Maven para publicar una función: Azure Functions'
description: Cree y publique una función simple desencadenada por HTTP en Azure con Java y Maven.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure functions, funciones, procesamiento de eventos, proceso, arquitectura sin servidor
ms.service: azure-functions
ms.topic: quickstart
ms.devlang: java
ms.date: 08/10/2018
ms.author: routlaw
ms.reviewer: glenga
ms.custom: mvc, devcenter, seo-java-july2019, seo-java-august2019
ms.openlocfilehash: 5c5a0285a827a7990a11eb6ef6445e7d84189767
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70096853"
---
# <a name="quickstart-use-java-to-create-and-publish-a-function-to-azure-functions"></a>Inicio rápido: Uso de Java para crear y publicar una función en Azure Functions

Este artículo le guiará con el uso de la herramienta de la línea de comandos de Maven para compilar y publicar una función de Java en Azure Functions. Cuando haya terminado, el código de la función se ejecuta en el [Plan de consumo](functions-scale.md#consumption-plan) en Azure y puede activarse mediante una solicitud HTTP.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para desarrollar funciones con Java, debe tener instalado lo siguiente:

- [Kit para desarrolladores de Java](https://aka.ms/azure-jdks), versión 8
- [Apache Maven](https://maven.apache.org), versión 3.0 o posterior
- [CLI de Azure](https://docs.microsoft.com/cli/azure)
- [Azure Functions Core Tools](./functions-run-local.md#v2), versión 2.6.666 u otra posterior

> [!IMPORTANT]
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar esta guía de inicio rápido.

## <a name="generate-a-new-functions-project"></a>Generación de un nuevo proyecto de Functions

En una carpeta vacía, ejecute el comando siguiente para generar el proyecto de Functions desde un [arquetipo Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/macOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

> [!NOTE]
> Si experimenta problemas al ejecutar el comando, eche un vistazo a qué versión de `maven-archetype-plugin` se usa. Dado que se ejecuta el comando en un directorio vacío sin ningún archivo `.pom`, podría estar intentando usar un complemento de la versión anterior de `~/.m2/repository/org/apache/maven/plugins/maven-archetype-plugin` si actualizó desde una versión anterior de Maven. Si es así, intente eliminar el directorio `maven-archetype-plugin` y vuelva a ejecutar el comando.

### <a name="windows"></a>Windows

```powershell
mvn archetype:generate `
    "-DarchetypeGroupId=com.microsoft.azure" `
    "-DarchetypeArtifactId=azure-functions-archetype"
```

```cmd
mvn archetype:generate ^
    "-DarchetypeGroupId=com.microsoft.azure" ^
    "-DarchetypeArtifactId=azure-functions-archetype"
```

Maven le pedirá los valores necesarios para acabar de generar el proyecto. Para conocer los valores _groupId_, _artifactId_ y _version_, consulte la referencia sobre las [convenciones de nomenclatura de Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html). El valor _appName_ debe ser único en todo Azure, por lo que Maven genera un nombre de aplicación en función del valor _artifactId_ escrito anteriormente como valor predeterminado. El valor _packageName_ determina el paquete de Java para el código de función generado.

Los identificadores `com.fabrikam.functions` y `fabrikam-functions` siguientes se utilizan como ejemplo y para facilitar la lectura de los pasos posteriores de esta guía de inicio rápido. En este paso, se recomienda especificar los valores propios en Maven.

```Output
Define value for property 'groupId' (should match expression '[A-Za-z0-9_\-\.]+'): com.fabrikam.functions
Define value for property 'artifactId' (should match expression '[A-Za-z0-9_\-\.]+'): fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus: :
Define value for property 'resourceGroup' java-functions-group: :
Confirm properties configuration: Y
```

Maven crea los archivos del proyecto en una carpeta nueva llamada _artifactId_, en este ejemplo `fabrikam-functions`. El código generado listo para ejecutarse del proyecto es una función [desencadenada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) que devuelve el cuerpo de la solicitud. Reemplace *src/main/java/com/fabrikam/functions/Function.java* con el código siguiente: 

```java
package com.fabrikam.functions;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class Function {
    /**
     * This function listens at endpoint "/api/HttpTrigger-Java". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/HttpTrigger-Java
     * 2. curl {your host}/api/HttpTrigger-Java?name=HTTP%20Query
     */
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = { HttpMethod.GET, HttpMethod.POST }, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
        } else {
            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
}

```

## <a name="enable-extension-bundles"></a>Habilitación de conjuntos de extensiones

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

## <a name="run-the-function-locally"></a>Ejecución local de la función

Cambie de directorio a la carpeta de proyecto que acaba de crear (la que contiene los archivos host.json y pom.xml) y compile y ejecute la función con Maven:

```CMD
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

   hello: http://localhost:7071/api/HttpTrigger-Java
```

Desencadene la función desde la línea de comandos, para lo que debe usar curl en una ventana de terminal nueva:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java -d LocalFunction
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

Implemente el código en una aplicación de función nueva `azure-functions:deploy` con el destino de Maven. Esto realiza una [implementación comprimida con el modo de ejecutar desde paquete](functions-deployment-technologies.md#zip-deploy) habilitado.

> [!NOTE]
> Cuando se usa Visual Studio Code para implementar la aplicación de función, recuerde que debe elegir una suscripción de pago o se producirá un error. Puede ver la suscripción en el lado izquierdo del IDE.

```azurecli
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

> [!NOTE]
> Asegúrese de establecer los **derechos de acceso** en `Anonymous`. Al elegir el nivel predeterminado de `Function`, tiene que presentar la [tecla de función](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) en las solicitudes para acceder al punto de conexión de la función.

```azurecli
curl -w "\n" https://fabrikam-function-20170920120101928.azurewebsites.net/api/HttpTrigger-Java -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

Edite el archivo de código fuente `src/main.../Function.java` en el proyecto generado para modificar el texto que devuelve la aplicación de función. Cambie esta línea:

```java
return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
```

Por lo siguiente:

```java
return request.createResponseBuilder(HttpStatus.OK).body("Hi, " + name).build();
```

Guarde los cambios. Ejecute el paquete mvn clean y repita la implementación mediante la ejecución de `azure-functions:deploy` desde el terminal como antes. La aplicación de función se actualizará y esta solicitud:

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
