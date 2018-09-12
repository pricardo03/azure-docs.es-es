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
ms.openlocfilehash: 16d6dd6a589256ad98a37465e64e847778d0cc7e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092600"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Creación de la primera función con Java y Maven (versión preliminar)

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

Este tutorial de inicio rápido lo guía para crear un proyecto de función [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) con Maven, probarlo localmente e implementarlo en Azure Functions. Cuando termine, tendrá una aplicación de función desencadenada por HTTP que se ejecuta en Azure.

![Acceso a una función Hola mundo desde la línea de comandos con cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos
Para desarrollar una aplicación de funciones con Java, debe tener instalado lo siguiente:

-  [Kit para desarrolladores de Java](https://www.azul.com/downloads/zulu/), versión 8.
-  [Apache Maven](https://maven.apache.org), versión 3.0 o posterior.
-  [CLI de Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar esta guía de inicio rápido.

## <a name="install-the-azure-functions-core-tools"></a>Instalación de Azure Functions Core Tools

Azure Functions Core Tools proporciona un entorno de desarrollo local para escribir, ejecutar y depurar Azure Functions desde el terminal o el símbolo del sistema. 

Instale la [versión 2 de Core Tools](functions-run-local.md#v2) en el equipo local antes de continuar.

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

El valor `appRegion` especifica en qué [región de Azure](https://azure.microsoft.com/global-infrastructure/regions/) desea ejecutar la aplicación de función implementada. Puede obtener una lista de valores de nombre de regiones mediante el comando `az account list-locations` de la CLI de Azure. El valor `resourceGroup` especifica en qué grupo de recursos de Azure se creará la aplicación de función.

Los identificadores `com.fabrikam.functions` y `fabrikam-functions` siguientes se utilizan como ejemplo y para facilitar la lectura de los pasos posteriores de esta guía de inicio rápido. En este paso, se recomienda especificar los valores propios en Maven.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus : 
Define value for property 'resourceGroup' java-functions-group: 
Confirm properties configuration: Y
```

Maven crea los archivos del proyecto en una carpeta nueva llamada _artifactId_, en este ejemplo `fabrikam-functions`. El código generado listo para ejecutarse del proyecto es una sencilla función [desencadenada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) que devuelve el cuerpo de la solicitud después de una cadena "Hola".

```java
public class Function {
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage HttpTriggerJava(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,final ExecutionContext context) {
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

## <a name="run-the-function-locally"></a>Ejecución local de la función

Cambie de directorio a la carpeta de proyecto que acaba de crear y compile y ejecute la función con Maven:

```
cd fabrikam-functions
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Si experimenta esta excepción: `javax.xml.bind.JAXBException` con Java 9, consulte la solución alternativa en [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

Esta salida se ve cuando la función se ejecuta localmente en el sistema y está lista para responder a las solicitudes HTTP:

```Output
Listening on http://0.0.0.0:7071/
Hit CTRL-C to exit...

Http Functions:

        HttpTrigger-Java: http://localhost:7071/api/HttpTrigger-Java
```

Desencadene la función desde la línea de comandos, para lo que debe usar curl en una ventana de terminal nueva:

```
curl -w '\n' -d LocalFunctionTest http://localhost:7071/api/HttpTrigger-Java
```

```Output
Hello, LocalFunctionTest
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
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

```Output
Hello, AzureFunctionsTest
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

Ha creado una aplicación de función de Java con un desencadenador HTTP simple y la ha implementado en Azure Functions.

- Revise la [guía para desarrolladores de Java Functions](functions-reference-java.md) para más información sobre cómo desarrollar funciones de Java.
- Agregue funciones adicionales con desencadenadores distintos en el proyecto con el destino de Maven `azure-functions:add`.
- Escriba y depure localmente las funciones con [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) y [Eclipse](functions-create-maven-eclipse.md). 
- Depure las funciones implementadas en Azure con Visual Studio Code. Consulte la documentación sobre las [aplicaciones Java sin servidor](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) de Visual Studio Code para instrucciones.
