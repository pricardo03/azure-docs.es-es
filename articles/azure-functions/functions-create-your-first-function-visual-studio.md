---
title: Creación de la primera función de Azure mediante Visual Studio
description: Cree y publique una función de Azure desencadenada mediante HTTP con Visual Studio.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso, arquitectura sin servidor
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 10/17/2018
ms.author: glenga
ms.custom: mvc, devcenter, vs-azure, 23113853-34f2-4f
ms.openlocfilehash: 51f6fb8c0dc1450a39173fafb0d71efc9a59a536
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649982"
---
# <a name="create-your-first-function-using-visual-studio"></a>Creación de la primera función mediante Visual Studio

Azure Functions permite ejecutar el código en un entorno [sin servidor](https://azure.microsoft.com/solutions/serverless/) sin necesidad de crear una máquina virtual o publicar una aplicación web.

En este artículo, aprenderá a usar las herramientas de Visual Studio 2017 para Azure Functions con el fin de crear y probar localmente una función "Hola mundo". Luego, publicará el código de función en Azure. Estas herramientas están disponibles como parte de la carga de trabajo de desarrollo de Azure en Visual Studio 2017.

![Respuesta de localhost de la función en el explorador](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local-final.png)

Este tema incluye [un vídeo](#watch-the-video) que muestra los mismos pasos básicos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

* Instale [Visual Studio 2017](https://azure.microsoft.com/downloads/) y asegúrese de que la carga de trabajo de **desarrollo de Azure** también está instalada.

* Asegúrese de que tiene las [herramientas de Azure Functions más recientes](functions-develop-vs.md#check-your-tools-version).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-function-app-project"></a>Creación de un proyecto de aplicación de función

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio crea un proyecto y dentro de él una clase que contiene código reutilizable para el tipo de función seleccionada. El atributo **FunctionName** en el método establece el nombre de la función. El atributo **HttpTrigger** especifica que una solicitud HTTP desencadena la función. El código reutilizable envía una respuesta HTTP que incluye un valor del cuerpo de la solicitud o de la cadena de consulta. Puede agregar enlaces de entrada y de salida a una función mediante la aplicación de los atributos adecuados al método. Para más información, consulte la sección [Desencadenadores y enlaces](functions-dotnet-class-library.md#triggers-and-bindings) de la [Referencia para desarrolladores de C# de Azure Functions](functions-dotnet-class-library.md).

Ahora que ha creado un proyecto de función y una función desencadenada por HTTP, puede probarla en el equipo local.

## <a name="test-the-function-locally"></a>Prueba local de la función

Azure Functions Core Tools le permite ejecutar un proyecto de Azure Functions en el equipo de desarrollo local. Se le solicita que instale estas herramientas la primera vez que inicie una función de Visual Studio.

1. Para probar la función, presione F5. Si se le solicita, acepte la solicitud de Visual Studio para descargar e instalar las herramientas de Azure Functions Core (CLI). También es preciso que habilite una excepción de firewall para que las herramientas para controlen las solicitudes de HTTP.

2. Copie la dirección URL de la función de los resultados del runtime de Azure Functions.

    ![Runtime local de Azure](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-debugging.png)

3. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<YOUR_NAME>` a esta dirección URL y ejecute la solicitud. A continuación se muestra la respuesta en el explorador para la solicitud GET local devuelta por la función: 

    ![Respuesta de localhost de la función en el explorador](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local.png)

4. Para detener la depuración, presione **Mayús + F5**.

Después de comprobar que la función se ejecuta correctamente en el equipo local es el momento de publicar el proyecto en Azure.

## <a name="publish-the-project-to-azure"></a>Publicar el proyecto en Azure

Debe tener una aplicación de función en la suscripción de Azure para poder publicar el proyecto. Las aplicaciones de función se pueden crear directamente desde Visual Studio.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Prueba de una función en Azure

1. Copie la URL base de la aplicación de función de la página de perfil de publicación. Reemplace la parte `localhost:port` de la dirección URL que usó al probar la función localmente por la nueva URL base. Como antes, asegúrese de que agrega la cadena de consulta `?name=<YOUR_NAME>` a esta dirección URL y ejecute la solicitud.

    La dirección URL que llama a la función desencadenada por HTTP debería tener el formato siguiente:

        http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?name=<YOUR_NAME> 

2. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. A continuación se muestra la respuesta en el explorador para la solicitud GET remota devuelta por la función:

    ![Respuesta de la función en el explorador](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png)

## <a name="watch-the-video"></a>Visualización del vídeo

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

## <a name="next-steps"></a>Pasos siguientes

Ha usado Visual Studio para crear y publicar una aplicación de función en C# con una función simple desencadenada por HTTP.

* [Aprenda a agregar enlaces de entrada y salida que se integren con otros servicios.](functions-develop-vs.md#add-bindings)
* [Más información acerca de las funciones de desarrollo como bibliotecas de clase .NET](functions-dotnet-class-library.md).
