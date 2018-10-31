---
title: 'Instalación de la extensión Durable Functions y ejemplos: Azure'
description: Obtenga información acerca de cómo instalar la extensión Durable Functions para Azure Functions, para el desarrollo de Azure Portal o para el desarrollo de Visual Studio.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 6bbf232fc17b9acfd4e8cd84a0cb1346ab8ea9b5
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986828"
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Instalación de la extensión Durable Functions y ejemplos (Azure Functions)

La extensión [Durable Functions](durable-functions-overview.md) para Azure Functions se proporciona en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Este artículo muestra cómo instalar el paquete y un conjunto de ejemplos para los entornos de desarrollo siguientes:

* Visual Studio 2017 (recomendado para C#) 
* Visual Studio Code (recomendado para JavaScript)
* Azure Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Actualmente, Visual Studio proporciona la mejor experiencia para el desarrollo de aplicaciones que usan Durable Functions.  Las funciones se pueden ejecutar localmente y también se pueden publicar en Azure. Puede empezar con un proyecto vacío o con un conjunto de funciones de ejemplo.

### <a name="prerequisites"></a>Requisitos previos

* Instale la [versión más reciente de Visual Studio](https://www.visualstudio.com/downloads/) (15.6 o posterior). Incluya la carga de trabajo del **desarrollo Azure** en las opciones de configuración.

### <a name="start-with-sample-functions"></a>Empezar con funciones de ejemplo 

1. Descargue el [archivo .zip de la aplicación de ejemplo para Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). No es necesario agregar la referencia de NuGet porque el proyecto de ejemplo ya la tiene.
2. Instale y ejecute la versión 5.6 o posterior del [Emulador de Azure Storage](https://docs.microsoft.com/azure/storage/storage-use-emulator). Como alternativa, puede actualizar el archivo *local.settings.json* con cadenas de conexión reales de Azure Storage.
3. Abra el proyecto en Visual Studio 2017. 
4. Para obtener instrucciones sobre cómo ejecutar el ejemplo, empiece con [Encadenamiento de funciones: Hello Sequence de ejemplo](durable-functions-sequence.md). El ejemplo se puede ejecutar localmente o publicarse en Azure.

### <a name="start-with-an-empty-project"></a>Empezar con un proyecto vacío
 
Siga las mismas instrucciones que para empezar con el ejemplo, pero, en lugar de descargar el archivo *.zip*, realice estos pasos:

1. Cree un proyecto de aplicación de función.
2. Busque la siguiente referencia del paquete NuGet mediante *Administrar paquetes NuGet* y agréguela al proyecto: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.6.2.
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code proporciona una experiencia de desarrollo local que abarca todas las principales plataformas: Windows, Mac OS y Linux.  Las funciones se pueden ejecutar localmente y también se pueden publicar en Azure. Puede empezar con un proyecto vacío o con un conjunto de funciones de ejemplo.

### <a name="prerequisites"></a>requisitos previos

* Instale la [versión más reciente de Visual Studio Code](https://code.visualstudio.com/Download). 

* Siga las instrucciones que aparecen en "Instalación de Azure Functions Core Tools" en [Codificación y comprobación de las funciones de Azure Functions en un entorno local](https://docs.microsoft.com/azure/azure-functions/functions-run-local).

    >[!IMPORTANT]
    > Si ya tiene las herramientas multiplataforma de Azure Functions, actualícelas a la última versión disponible.

    >[!IMPORTANT]
    >En JavaScript, Durable Functions requiere la versión 2.x de Azure Functions Core Tools.

*  En una máquina Windows, instale y ejecute la versión 5.6 o posterior del [Emulador de Azure Storage](https://docs.microsoft.com/azure/storage/storage-use-emulator). Como alternativa, puede actualizar el archivo *local.settings.json* con cadenas de conexión reales de Azure Storage. 


### <a name="start-with-sample-functions"></a>Empezar con funciones de ejemplo

#### <a name="c"></a>C#

1. Clone el [repositorio de Durable Functions](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navegue hasta el equipo en la [carpeta de ejemplos de scripts de C#](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Instale la extensión Durable Functions de Azure ejecutando lo siguiente en una ventana del terminal o del símbolo del sistema:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.6.2
    ```
4. Instale la extensión Twilio de Azure Functions. Para ello, ejecute lo siguiente en una ventana del terminal o del símbolo del sistema:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0
    ```
5. Ejecute el Emulador de Azure Storage o actualice el archivo *local.settings.json* con cadenas de conexión reales de Azure Storage.
6. Abra el proyecto en Visual Studio Code. 
7. Para obtener instrucciones sobre cómo ejecutar el ejemplo, empiece con [Encadenamiento de funciones: Hello Sequence de ejemplo](durable-functions-sequence.md). El ejemplo se puede ejecutar localmente o publicarse en Azure.
8. Inicie el proyecto ejecutando en el terminal o el símbolo del sistema el siguiente comando:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (solo Functions v2)

1. Clone el [repositorio de Durable Functions](https://github.com/Azure/azure-functions-durable-extension.git).
2. En la máquina, vaya a la [carpeta de ejemplos de scripts de JavaScript](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Instalación de la extensión Durable Functions de Azure mediante la ejecución del comando siguiente en una ventana de terminal o en un símbolo del sistema

    ```bash
    func extensions install
    ```
    > [!NOTE] 
    > Es necesario que el [SDK de .NET Core](https://www.microsoft.com/net/download) esté instalado en la máquina.
4. Ejecute lo siguiente en una ventana del terminal o del símbolo del sistema para restaurar los paquetes npm:
    
    ```bash
    npm install
    ``` 
5. Actualice el archivo *local.settings.json* con una cadena de conexión de una cuenta de Azure Storage para `AzureWebJobsStorage`.  Esta cuenta de almacenamiento se usará para el estado de la función durable.
6. Abra el proyecto en un editor como Visual Studio Code. 
7. Para obtener instrucciones sobre cómo ejecutar el ejemplo, empiece con [Encadenamiento de funciones: Hello Sequence de ejemplo](durable-functions-sequence.md). El ejemplo se puede ejecutar localmente o publicarse en Azure.
8. Inicie el proyecto ejecutando en el terminal o el símbolo del sistema el siguiente comando:
    ```
    func start
    ```

### <a name="start-with-an-empty-project"></a>Empezar con un proyecto vacío
 
1. En el terminal o el símbolo del sistema, navegue hasta la carpeta que va a hospedar la aplicación de función.
3. Para ir al proyecto de Function App, ejecute el siguiente comando:

    ```bash
    func init
    ``` 
4. Ejecute el Emulador de Azure Storage (solo Windows) o actualice el archivo *local.settings.json* con cadenas de conexión reales de Azure Storage para `AzureWebJobsStorage`.
5. Después, cree una nueva función ejecutando el comando siguiente y siga los pasos del asistente:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Actualmente no está disponible la plantilla de función duradera, pero puede empezar con una de las opciones admitidas y, luego, modificar el código. Use para la referencia los ejemplos de [cliente de orquestación](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [desencadenador de orquestación](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence) y [desencadenador de actividad](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).
2. Instale la extensión Durable Functions de Azure mediante la ejecución del siguiente comando en una ventana de terminal o en un símbolo del sistema:

    ```
    func extensions install
    ```

6. Abra la carpeta de proyecto de Visual Studio Code y continúe modificando el código de plantilla. 
7. Inicie el proyecto ejecutando en el terminal o el símbolo del sistema el siguiente comando:
    ```
    func start
    ```

## <a name="azure-portal"></a>Azure Portal

Si lo prefiere, puede usar [Azure Portal](https://portal.azure.com) para el desarrollo con Durable Functions.

   > [!NOTE]
   > En el portal aún no está disponible Durable Functions con JavaScript.

### <a name="create-an-orchestrator-function"></a>Creación de una función de orquestador

1. Cree una nueva aplicación de función en el portal, tal como se muestra en el [artículo de inicio rápido de funciones](functions-create-first-azure-function.md#create-a-function-app).

2. Configure la aplicación de la función para [usar la versión de tiempo de ejecución 2.0](set-runtime-version.md).

   La extensión Durable Functions funciona tanto en el entorno de tiempo de ejecución 1.X como 2.0, pero las plantillas de Azure Portal solo están disponibles cuando se usa el entorno de tiempo de ejecución 2.0.

3. Cree una nueva función seleccionando **"Crear su propia función personalizada"**.

4. Cambie el **lenguaje** a **C#**, el **escenario** a **Durable Functions** y seleccione la plantilla **Durable Functions Http Starter - C#**.

5. En **Extensiones no instaladas**, haga clic en **Instalar** para descargar la extensión de NuGet.org. 

6. Una vez completada la instalación, continúe con la creación de una función de cliente de orquestación, **"HttpStart"**, que se crea mediante la plantilla **Durable Functions Http Starter - C#**.

7. Después, cree una función de orquestación **"HelloSequence"** a partir de la plantilla **Durable Functions Orchestrator - C#**.

8. Y se llamará a la última función **"Hello"** de la plantilla **Durable Functions Activity - C#**.

9. Vaya a la función **"HttpStart"** y copie su dirección URL.

10. Use Postman o CURL para llamar a la función duradera. Antes de realizar la prueba, reemplace en la dirección URL **{functionName}** con el nombre de la función de orquestación **HelloSequence**.  No se requiere ningún dato; use el verbo POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Después, llame al punto de conexión **"statusQueryGetUri"** y vea el estado actual de la función duradera.

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Continuar con la llamada al punto de conexión **"statusQueryGetUri"** hasta que el estado cambie a **"Completado"**. 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Felicidades. La primera función duradera está en funcionamiento en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejecutar el ejemplo de encadenamiento de funciones](durable-functions-sequence.md)
