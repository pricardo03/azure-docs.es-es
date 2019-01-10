---
title: Creación de la primera función durable en Azure con JavaScript
description: Cree y publique una instancia de Azure Durable Functions con Visual Studio Code.
services: functions
documentationcenter: na
author: ColbyTresness
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 11/07/2018
ms.author: azfuncdf, cotresne, glenga
ms.openlocfilehash: 0db76e80b2ff7400e7c2914efbd2bbcb24d9b005
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034325"
---
# <a name="create-your-first-durable-function-in-javascript"></a>Creación de su primera función durable en JavaScript

*Durable Functions* es una extensión de [Azure Functions](../functions-overview.md) que le permite escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre.

En este artículo, aprenderá a usar la extensión de Azure Functions para Visual Studio Code con el fin de crear y probar localmente una función durable "Hola mundo".  Esta función organizará y encadenará llamadas a otras funciones. Luego, publicará el código de función en Azure.

![Ejecución de una función durable en Azure](./media/quickstart-js-vscode/functions-vs-code-complete.png)

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

* Instale [Visual Studio Code](https://code.visualstudio.com/download).

* Asegúrese de que tiene las [herramientas de Azure Functions más recientes](../functions-develop-vs.md#check-your-tools-version).

* En un equipo Windows, compruebe que tiene el [Emulador de Azure Storage](../../storage/common/storage-use-emulator.md) instalado y ejecutándose. En un equipo Mac o Linux, debe usar una cuenta de Azure Storage real.

* Asegúrese de que tiene instalada la versión 8.0 o una versión posterior de [Node.js](https://nodejs.org/).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-create-function-app-vs-code.md)]

## <a name="install-the-durable-functions-npm-package"></a>Instalación del paquete npm de Durable Functions

1. Instale el paquete npm `durable-functions` mediante la ejecución de `npm install durable-functions` en el directorio raíz de la aplicación de función.

## <a name="create-a-starter-function"></a>Creación de una función de inicio

En primer lugar, cree una función desencadenada por HTTP que inicie una orquestación de función durable.

1. En **Azure: Functions**, elija el icono Crear función.

    ![Creación de una función](./media/quickstart-js-vscode/create-function.png)

2. Seleccione la carpeta con el proyecto de la aplicación de función y seleccione la plantilla de función del **desencadenador HTTP**.

    ![Elegir la plantilla del desencadenador HTTP](./media/quickstart-js-vscode/create-function-choose-template.png)

3. Escriba `HttpStart` como nombre de la función y presione ENTRAR y, a continuación, seleccione la opción de autenticación **Anónimo**.

    ![Elegir autenticación anónima](./media/quickstart-js-vscode/create-function-anonymous-auth.png)

    Se crea una función en el lenguaje elegido mediante la plantilla para una función desencadenada por HTTP.

4. Reemplace index.js por el siguiente código JavaScript:

    [!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

5. Reemplace function.json por el siguiente código JSON:

    [!code-json[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json)]

Ahora hemos creado un punto de entrada en nuestra función durable. Vamos a agregar un orquestador.

## <a name="create-an-orchestrator-function"></a>Creación de una función de orquestador

A continuación, cree otra función para que sea el orquestador. Para mayor comodidad, usamos la plantilla de función de desencadenador HTTP. El código de la propia función se reemplaza por el código del orquestador.

1. Repita los pasos de la sección anterior para crear una segunda función mediante la plantilla de desencadenador HTTP. Esta vez, llame a la función `OrchestratorFunction`.

2. Abra el archivo index.js de la nueva función y reemplace el contenido por el código siguiente:

    [!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

3. Abra el archivo function.json y reemplácelo por el siguiente código JSON:

    [!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/function.json)]

Hemos agregado un orquestador para coordinar las funciones de actividad. Ahora agregaremos la función de actividad a la que se hace referencia.

## <a name="create-an-activity-function"></a>Creación de una función de actividad

1. Repita los pasos de la sección anterior para crear una tercera función mediante la plantilla de desencadenador HTTP. Esta vez, llame a la función `SayHello`.

2. Abra el archivo index.js de la nueva función y reemplace el contenido por el código siguiente:

    [!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

3. Reemplace function.json por el siguiente código JSON:

    [!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

Ya hemos agregado todos los componentes necesarios para iniciar una orquestación y encadenar funciones de actividad.

## <a name="test-the-function-locally"></a>Prueba local de la función

Azure Functions Core Tools le permite ejecutar un proyecto de Azure Functions en el equipo de desarrollo local. Se le solicita que instale estas herramientas la primera vez que inicie una función de Visual Studio Code.  

1. En un equipo Windows, inicie el Emulador de Azure Storage y asegúrese de que la propiedad **AzureWebJobsStorage** de local.settings.json está establecida en `UseDevelopmentStorage=true`. En un equipo Mac o Linux, debe establecer la propiedad **AzureWebJobsStorage** en la cadena de conexión de una cuenta de Azure Storage existente. Más adelante en este artículo creará una cuenta de almacenamiento.

2. Para probar la función, establezca un punto de interrupción en el código de la función y presione F5 para iniciar el proyecto de la aplicación de función. La salida de Core Tools aparece en el panel **Terminal**. Si se trata de la primera vez que usa Durable Functions, se instalará la extensión Durable Functions y es posible que la compilación tarde unos segundos.

    > [!NOTE]
    > JavaScript Durable Functions requiere la versión **1.7.0** o posterior de la extensión **Microsoft.Azure.WebJobs.Extensions.DurableTask**. Compruebe que la versión de la extensión de Durable Functions del archivo `extensions.csproj` cumple con este requisito. Si no es así, detenga la aplicación de función, cambie la versión y presione F5 para reiniciar la aplicación de función.

3. En el panel **Terminal**, copie el punto de conexión de la dirección URL de la función desencadenada por HTTP.

    ![Salida local de Azure](../media/functions-create-first-function-vs-code/functions-vscode-f5.png)

4. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador y observe el estado de la orquestación.

5. Para detener la depuración, presione Mayús + F1.

Después de comprobar que la función se ejecuta correctamente en el equipo local es el momento de publicar el proyecto en Azure.

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../../includes/functions-publish-project-vscode.md)]

## <a name="test-your-function-in-azure"></a>Prueba de una función en Azure

1. Copie la dirección URL del desencadenador HTTP del panel **Salida**. La dirección URL que llama a la función desencadenada por HTTP debería tener el formato siguiente:

        http://<functionappname>.azurewebsites.net/orchestrators/<functionname>

2. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Debe obtener la misma respuesta de estado que antes cuando se usó la aplicación publicada.

## <a name="next-steps"></a>Pasos siguientes

Ha usado Visual Studio Code para crear y publicar una aplicación de función durable con JavaScript.

> [!div class="nextstepaction"]
> [Más información sobre los patrones comunes de funciones durables](durable-functions-concepts.md)