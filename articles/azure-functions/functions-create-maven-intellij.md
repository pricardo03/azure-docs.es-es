---
title: Creación de una aplicación de función de Azure con Java e IntelliJ | Microsoft Docs
description: Guía de procedimientos para crear y publicar una aplicación sencilla sin servidor desencadenada por HTTP con Java e IntelliJ en Azure Functions.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: azure functions, functions, procesamiento de eventos, proceso, arquitectura sin servidor, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: e926bfb023fe3edfd564aa6389e21f6594bec169
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117065"
---
# <a name="create-your-first-function-with-java-and-intellij-preview"></a>Creación de la primera función con Java e IntelliJ (versión preliminar)

> [!NOTE] 
> Java para Azure Functions actualmente se encuentra disponible en versión preliminar.

En este artículo se muestra cómo crear un proyecto de función [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) con IntelliJ IDEA y Apache Maven, probarlo y depurarlo en su propio equipo desde el IDE e implementarlo en Azure Functions. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Configuración de su entorno de desarrollo

Para desarrollar una aplicación de Functions con Java e IntelliJ, debe tener instalado lo siguiente:

-  [Kit para desarrolladores de Java](https://www.azul.com/downloads/zulu/), versión 8.
-  [Apache Maven](https://maven.apache.org), versión 3.0 o posterior.
-  [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Community o Ultimate con herramientas de Maven.
-  [CLI de Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar esta guía de inicio rápido.

Se recomienda también instalar [Azure Functions Core Tools, versión 2](functions-run-local.md#v2), que proporcionan un entorno de desarrollo local para escribir, ejecutar y depurar Azure Functions. 


## <a name="create-a-functions-project"></a>Creación de un proyecto de Functions

1. En IntelliJ IDEA, haga clic en **Create New Project** (Crear nuevo proyecto).  
1. Seleccione la creación desde **Maven**
1. Active la casilla para **Create from archetype** (Crear desde arquetipo) y **Add Archetype** (Agregar arquetipo) para [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - GroupId: com.microsoft.azure
    - ArtifactId: azure-functions-archetype
    - Version: use la versión más reciente del [repositorio central](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![creación de IntelliJ Maven](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Haga clic en **Next** (Siguiente) y escriba los detalles para el proyecto actual y, por último, en **Finish** (Finalizar).

Maven crea los archivos del proyecto en una carpeta nueva con el nombre _artifactId_. El código generado en el proyecto es una función [desencadenada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) simple que devuelve el cuerpo de la solicitud HTTP desencadenadora.

## <a name="run-functions-locally-in-the-ide"></a>Ejecución local de funciones en el IDE

> [!NOTE]
> [Azure Functions Core Tools, versión 2](functions-run-local.md#v2), debe estar instalado para ejecutar y depurar localmente las funciones.

1. Seleccione la opción para importar los cambios o asegúrese de que la opción de [importación automática](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html) esté habilitada.
1. Abra la barra de herramientas **Maven Projects** (Proyectos de Maven)
1. En Lifecycle (Ciclo de vida), haga doble clic en **package** para empaquetar y compilar la solución y crear un directorio de destino.
1. Under Plugins (Complementos) -> azure-functions, haga doble clic en **azure-functions:run** para iniciar el runtime local de Azure Functions.  
  ![Barra de herramientas de Maven para Azure Functions](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

Cierre el cuadro de diálogo de ejecución cuando termine de probar la función. Solo puede haber un host de función activo y en ejecución en el entorno local a la vez.

### <a name="debug-the-function-in-intellij"></a>Depuración de la función en IntelliJ

Puede depurar las funciones en IntelliJ si lo adjunta al host de la función después del inicio.  Ejecute localmente Azure Functions con los pasos mencionados y, en el menú **Run** (Ejecutar), seleccione **Attach to local process** (Adjuntar al proceso local).  Debería ver un proceso en el puerto 5005 disponible.  Después de adjuntar, puede tener puntos de interrupción que se ejecutan y depuran dentro de la aplicación de función.

Cuando termine, detenga el depurador y el proceso en ejecución. Solo puede haber un host de función activo y en ejecución en el entorno local a la vez.

## <a name="deploy-the-function-to-azure"></a>Implementación de la función en Azure

El proceso de implementación en Azure Functions usa las credenciales de cuenta desde la CLI de Azure. [Inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de seguir usando el símbolo del sistema de su equipo.

```azurecli
az login
```

Implemente el código en una aplicación de Function nueva con el destino de Maven `azure-functions:deploy` o seleccione la opción azure-functions:deploy en la ventana Maven Projects (Proyectos de Maven).

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

## <a name="next-steps"></a>Pasos siguientes

- Revise la [guía para desarrolladores de Java Functions](functions-reference-java.md) para más información sobre cómo desarrollar funciones de Java.
- Agregue funciones adicionales con desencadenadores distintos en el proyecto con el destino de Maven `azure-functions:add`.
