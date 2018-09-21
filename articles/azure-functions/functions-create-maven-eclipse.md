---
title: Creación de una aplicación de función de Azure con Java y Eclipse | Microsoft Docs
description: Guía de procedimientos para crear y publicar una aplicación sencilla sin servidor desencadenada por HTTP con Java y Eclipse en Azure Functions.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: azure functions, functions, procesamiento de eventos, proceso, arquitectura sin servidor, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 8b0681165fe84dd54f3b81f610c6698998e881de
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125500"
---
# <a name="create-your-first-function-with-java-and-eclipse-preview"></a>Creación de la primera función con Java y Eclipse (versión preliminar)

> [!NOTE] 
> Java para Azure Functions actualmente se encuentra disponible en versión preliminar.

En este artículo se muestra cómo crear un proyecto de función [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) con el IDE de Eclipse y Apache Maven, probarlo y depurarlo, e implementarlo en Azure Functions. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Configuración de su entorno de desarrollo

Para desarrollar una aplicación de función con Java y Eclipse, debe tener instalado lo siguiente:

-  [Kit para desarrolladores de Java](https://www.azul.com/downloads/zulu/), versión 8.
-  [Apache Maven](https://maven.apache.org), versión 3.0 o posterior.
-  [Eclipse](https://www.eclipse.org/downloads/packages/), con compatibilidad con Java y Maven.
-  [CLI de Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar esta guía de inicio rápido.

Se recomienda encarecidamente instalar también [Azure Functions Core Tools, versión 2](functions-run-local.md#v2), que proporciona un entorno de desarrollo local para ejecutar y depurar Azure Functions. 

## <a name="create-a-functions-project"></a>Creación de un proyecto de Functions

1. En Eclipse, seleccione el menú **File** (Archivo) y, luego, elija **Project** (Proyecto). 
1. Abra la carpeta **Java Project** (Proyecto de Java) en la ventana **New Project** (Nuevo proyecto), seleccione **Maven Project** (Proyecto de Maven) y, a continuación, elija **Next** (Siguiente).
1. Acepte los valores predeterminados en el cuadro de diálogo **New Maven Project** (Nuevo proyecto de Maven) y seleccione **Next** (Siguiente).
1. Seleccione **Add Archetype** (Agregar arquetipo) y agregue las entradas para [azure-functions-arquetipo](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - Identificador de grupo de arquetipo: com.microsoft.azure
    - Identificador de artefacto de arquetipo: azure-functions-arquetipo
    - Versión: use la versión más reciente del [repositorio central](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![creación de Eclipse Maven](media/functions-create-first-java-eclipse/functions-create-eclipse.png)  
1. Haga clic en **OK** (Aceptar), escriba los detalles del proyecto actual y, por último, seleccione **Finish** (Finalizar).

Maven crea los archivos del proyecto en una carpeta nueva con el nombre _artifactId_. El código generado en el proyecto es una función [desencadenada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) simple que devuelve el cuerpo de la solicitud HTTP desencadenadora.

## <a name="run-functions-locally-in-the-ide"></a>Ejecución local de funciones en el IDE

> [!NOTE]
> [Azure Functions Core Tools, versión 2](functions-run-local.md#v2), debe estar instalado para ejecutar y depurar localmente las funciones.

1. Haga clic con el botón derecho en el proyecto generado y, luego, elija **Ejecutar como** y **Maven build** (compilación de Maven).
1. En el cuadro de diálogo **Edit Configuration** (Editar configuración), escriba `package` en los campos **Goals** (Objetivos) y **Name** (Nombres); luego, seleccione **Run** (Ejecutar). Con esto, se compila y empaqueta el código de función.
1. Una vez completada la compilación, cree otra configuración de ejecución como las anteriores utilizando `azure-functions:run` como el objetivo y el nombre. Seleccione **Run** (Ejecutar) para ejecutar la función en el IDE.

Finalice el tiempo de ejecución en la ventana de consola cuando haya terminado de probar la función. Solo puede haber un host de función activo y en ejecución en el entorno local a la vez.

### <a name="debug-the-function-in-eclipse"></a>Depuración de la función en Eclipse

En la configuración de **Run As** (Ejecutar como) establecida en el paso anterior, cambie `azure-functions:run` a `mvn azure-functions:run -DenableDebug` y ejecute la configuración actualizada para iniciar la aplicación de función en el modo de depuración.

Seleccione el menú **Run** (Ejecutar) y abra **Debug Configurations** (Configuraciones de depuración). Elija **Remote Java Application** (Aplicación de Java remota) y cree una. Asigne un nombre a la configuración y complete la configuración. El puerto debe ser el mismo que el del puerto de depuración que abrió el host de función, cuyo valor predeterminado es `5005`. Después de la instalación, haga clic en `Debug` para iniciar la depuración.

![Depuración de funciones en Eclipse](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

Establezca puntos de interrupción e inspeccione los objetos de la función mediante el IDE. Cuando termine, detenga el depurador y el host de la función en ejecución. Solo puede haber un host de función activo y en ejecución en el entorno local a la vez.

## <a name="deploy-the-function-to-azure"></a>Implementación de la función en Azure

El proceso de implementación en Azure Functions usa las credenciales de cuenta desde la CLI de Azure. [Inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de seguir usando el símbolo del sistema de su equipo.

```azurecli
az login
```

Implemente el código en una nueva aplicación de función con el objetivo de Maven `azure-functions:deploy` en una nueva configuración de **Run As** (Ejecutar como).

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
