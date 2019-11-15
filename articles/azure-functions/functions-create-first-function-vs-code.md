---
title: Creación de la primera función en Azure mediante Visual Studio Code
description: Cree y publique en Azure una función simple desencadenada por HTTP mediante la extensión de Azure Functions de Visual Studio Code.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso, arquitectura sin servidor
ms.service: azure-functions
ms.topic: quickstart
ms.date: 06/25/2019
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: fbde3c939a23e4023c0403c27433717a11762b08
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622052"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Creación de la primera función mediante Visual Studio Code

Azure Functions permite ejecutar el código en un entorno [sin servidor](https://azure.microsoft.com/solutions/serverless/) sin necesidad de crear una máquina virtual o publicar una aplicación web.

En este artículo aprenderá a usar la [extensión de Azure Functions para Visual Studio Code] para crear y probar una función "hola mundo" en su equipo local mediante Microsoft Visual Studio Code. Posteriormente, puede publicar el código de la función en Azure desde Visual Studio Code.

![Código Azure Functions en un proyecto de Visual Studio](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

La extensión admite actualmente funciones de C#, JavaScript, Java y Python. Los pasos descritos en este artículo y en el artículo que sigue admiten solo funciones de JavaScript y C#. Para obtener información sobre cómo usar Visual Studio Code para crear y publicar las funciones de Python, consulte [Creación e implementación de Azure Functions sin servidor en Python con Visual Studio Code](/azure/python/tutorial-vs-code-serverless-python-01). Para obtener información sobre cómo usar Visual Studio Code para crear y publicar las funciones de PowerShell, consulte [Cree su primera función de PowerShell en Azure](functions-create-first-function-powershell.md). 

Actualmente, la extensión se encuentra en versión preliminar. Para más información, consulte la página de la [extensión de Azure Functions para Visual Studio Code].

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido:

* Instale [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas compatibles](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Instale la versión 2.x de [Azure Functions Core Tools](functions-run-local.md#v2).

* Instale los requisitos específicos para el lenguaje elegido:

    | Idioma | Requisito |
    | -------- | --------- |
    | **C#** | [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)  |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> | 
 
    <sup>*</sup>Versiones Active LTS y Maintenance LTS (se recomiendan 8.11.1 y 10.14.1).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../includes/functions-create-function-app-vs-code.md)]

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Después de comprobar que la función se ejecuta correctamente en el equipo local es el momento de publicar el proyecto en Azure.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>Ejecución de la función en Azure

1. Copie la dirección URL del desencadenador HTTP del panel **Salida**. Esta dirección URL incluye la clave de función, que se pasa al parámetro de consulta `code`. Como antes, asegúrese de que agrega la cadena de consulta `?name=<yourname>` al final de esta dirección URL y ejecute la solicitud.

    La dirección URL que llama a la función desencadenada por HTTP debería tener el formato siguiente:

        http://<functionappname>.azurewebsites.net/api/<functionname>?code=<function_key>&name=<yourname> 

1. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. A continuación se muestra la respuesta en el explorador para la solicitud GET remota devuelta por la función: 

    ![Respuesta de la función en el explorador](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>Pasos siguientes

Ha usado Visual Studio Code para crear una aplicación de función con una función simple desencadenada por HTTP. En el siguiente artículo, esa función se expande mediante la adición de un enlace de salida. Este enlace escribe la cadena de la solicitud HTTP en un mensaje en una cola de Azure Queue Storage. El siguiente artículo también muestra cómo limpiar estos nuevos recursos de Azure mediante la eliminación del grupo de recursos que ha creado.

> [!div class="nextstepaction"]
> [Adición de un enlace de cola de Azure Storage a una función](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Extensión de Azure Functions para Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
