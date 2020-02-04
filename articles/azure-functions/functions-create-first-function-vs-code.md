---
title: Creación de la primera función en Azure mediante Visual Studio Code
description: Cree y publique en Azure una función simple desencadenada por HTTP mediante la extensión de Azure Functions de Visual Studio Code.
ms.topic: quickstart
ms.date: 01/10/2020
ms.custom: mvc, devcenter
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 0540c7b01d693975f34515c7d13f0477ac74d4a1
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842263"
---
# <a name="quickstart-create-an-azure-functions-project-using-visual-studio-code"></a>Inicio rápido: Creación de un proyecto de Azure Functions en Visual Studio Code

En este artículo se usa Visual Studio Code para crear una función que responda a solicitudes HTTP. Después de probar el código localmente, se implementa en el entorno sin servidor de Azure Functions. Este inicio rápido supone un pequeño costo en su cuenta de Azure. 

También hay una [versión basada en la CLI](functions-create-first-azure-function-azure-cli.md) de este artículo.

## <a name="prerequisites"></a>Prerequisites

+ [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas admitidas](https://code.visualstudio.com/docs/supporting/requirements#_platforms). 
::: zone pivot="programming-language-csharp"
+ La [extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) para Visual Studio Code.
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ Versiones de [Node.js](https://nodejs.org/), Active LTS y Maintenance LTS (se recomiendan 8.11.1 y 10.14.1).
::: zone-end
::: zone pivot="programming-language-python"
+ [Python 3.7](https://www.python.org/downloads/release/python-375/) o [Python 3.6](https://www.python.org/downloads/release/python-368/), que admite Azure Functions. Python 3.8 todavía no se admite. 

+ La [extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) para Visual Studio Code
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ El [SDK de .NET Core 2.2+](https://www.microsoft.com/net/download)

+ La [extensión de PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) para Visual Studio Code 
::: zone-end

+ La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code. 

+ Una [cuenta de Azure](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) con una suscripción activa. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

## <a name="create-an-azure-functions-project"></a>Creación del proyecto local 

En esta sección se usa Visual Studio Code para crear un proyecto local en Azure Functions en su lenguaje preferido. Más adelante en este artículo, publicará el código de función en Azure. 

1. Seleccione el icono de Azure en la barra de actividades y después en el área **Azure: Functions**, seleccione el icono **Crear un proyecto**.

    ![Elija Crear un proyecto.](media/functions-create-first-function-vs-code/create-new-project.png)

1. Elija una ubicación de directorio para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Estos pasos se han diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Escriba la siguiente información cuando se le indique:

    ::: zone pivot="programming-language-csharp"

    | Prompt | Value | 
    | ------ | ----- | 
    | Seleccione un lenguaje para el proyecto de funciones. | C# |
    | Seleccione una versión. | Azure Functions v2 | 
    | Seleccionar una plantilla para la primera función de su proyecto | Desencadenador HTTP | 
    | Proporcionar un nombre de función | HttpExample | 
    | Proporcionar un espacio de nombres | My.Functions | 
    | Nivel de autorización | Anónimas | 
    | Seleccionar cómo desea que se abra el proyecto | Agregar a área de trabajo |

    ::: zone-end

    ::: zone pivot="programming-language-javascript"

    | Prompt | Value | 
    | ------ | ----- | 
    | Seleccione un lenguaje para el proyecto de funciones. | JavaScript | 
    | Seleccione una versión. | Azure Functions v2 | 
    | Seleccionar una plantilla para la primera función de su proyecto | Desencadenador HTTP | 
    | Proporcionar un nombre de función | HttpExample | 
    | Nivel de autorización | Anónimas | 
    | Seleccionar cómo desea que se abra el proyecto | Agregar a área de trabajo |

    ::: zone-end

    ::: zone pivot="programming-language-typescript"

    | Prompt | Value | 
    | ------ | ----- | 
    | Seleccione un lenguaje para el proyecto de funciones. | TypeScript | 
    | Seleccione una versión. | Azure Functions v2 | 
    | Seleccionar una plantilla para la primera función de su proyecto | Desencadenador HTTP | 
    | Proporcionar un nombre de función | HttpExample | 
    | Nivel de autorización | Anónimas | 
    | Seleccionar cómo desea que se abra el proyecto | Agregar a área de trabajo |

    ::: zone-end

    ::: zone pivot="programming-language-powershell"

    | Prompt | Value | 
    | ------ | ----- | 
    | Seleccione un lenguaje para el proyecto de funciones. | PowerShell | 
    | Seleccione una versión. | Azure Functions v2 | 
    | Seleccionar una plantilla para la primera función de su proyecto | Desencadenador HTTP | 
    | Proporcionar un nombre de función | HttpExample | 
    | Nivel de autorización | Anónimas | 
    | Seleccionar cómo desea que se abra el proyecto | Agregar a área de trabajo |

    ::: zone-end

    ::: zone pivot="programming-language-python"

    | Prompt | Value | 
    | ------ | ----- | 
    | Seleccione un lenguaje para el proyecto de funciones. | Python | 
    | Seleccione una versión. | Azure Functions v2 | 
    | Seleccione un alias de Python para crear un entorno virtual. | Alias de Python | 
    | Seleccionar una plantilla para la primera función de su proyecto | Desencadenador HTTP | 
    | Proporcionar un nombre de función | HttpExample | 
    | Nivel de autorización | Anónimas | 
    | Seleccionar cómo desea que se abra el proyecto | Agregar a área de trabajo | 

    ::: zone-end

1. Visual Studio Code hace lo siguiente:

    + Crea un proyecto de Azure Functions en una nueva área de trabajo, que contiene los archivos de configuración [host.json](functions-host-json.md) y [local.settings.json](functions-run-local.md#local-settings-file). 

    ::: zone pivot="programming-language-csharp"

    + Crea un [archivo de biblioteca de clases de HttpExample.cs](functions-dotnet-class-library.md#functions-class-library-project) que implementa la función.

    ::: zone-end
        
    ::: zone pivot="programming-language-javascript"
    
    + Crea un archivo package.json en la carpeta raíz.

    + Crea una carpeta HttpExample que contiene el [archivo de definición function.json](functions-reference-node.md#folder-structure) y el [archivo index.js](functions-reference-node.md#exporting-a-function), un archivo Node.js que contiene el código de función.
    
    ::: zone-end
    
    ::: zone pivot="programming-language-typescript"
    
    + Crea un archivo package.json y un archivo tsconfig.json en la carpeta raíz.

    + Crea una carpeta HttpExample que contiene el [archivo de definición function.json](functions-reference-node.md#folder-structure) y el [archivo index.js](functions-reference-node.md#typescript), un archivo TypeScript que contiene el código de función.
    
    ::: zone-end
    
    ::: zone pivot="programming-language-powershell"
    
    + Crea una carpeta HttpExample que contiene el [archivo de definición function.json](functions-reference-python.md#programming-model) y el archivo run.ps1 que contiene el código de función.
    
    ::: zone-end
    
    ::: zone pivot="programming-language-python"
    
    + Crea un archivo requirements.txt de nivel de proyecto que muestra los paquetes que requieren las funciones.
    
    + Crea una carpeta HttpExample que contiene el [archivo de definición function.json](functions-reference-python.md#programming-model) y el archivo \_\_init\_\_.py que contiene el código de función.
    
    ::: zone-end

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-python"

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end

::: zone pivot="programming-language-powershell"

[!INCLUDE [functions-run-function-test-local-vs-code-ps](../../includes/functions-run-function-test-local-vs-code-ps.md)]

::: zone-end

Después de comprobar que la función se ejecuta correctamente en el equipo local, es el momento de usar Visual Studio Code para publicar el proyecto directamente en Azure. 

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>Ejecución de la función en Azure

1. Copie la dirección URL del desencadenador HTTP del panel **Salida**. De nuevo, agregue la cadena de consulta `name` como `?name=<yourname>` al final de esta dirección URL y ejecute la solicitud.

    La dirección URL que llama a la función desencadenada por HTTP debería tener el formato siguiente:

        http://<functionappname>.azurewebsites.net/api/httpexample?name=<yourname> 

1. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. En el siguiente ejemplo se muestra la respuesta en el explorador para la solicitud GET remota devuelta por la función: 

    ![Respuesta de la función en el explorador](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando continúa con el paso siguiente, [Adición de un enlace de cola de Azure Storage a una función](functions-add-output-binding-storage-queue-vs-code.md), tendrá que conservar todos los recursos intactos para crear a partir de lo que ya ha hecho.

De lo contrario, puede usar los pasos siguientes para eliminar la aplicación de funciones y sus recursos relacionados para evitar incurrir en costos adicionales.

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

Para más información sobre los costos de Functions, consulte [Estimación de los costos según el plan de consumo](functions-consumption-costs.md).

## <a name="next-steps"></a>Pasos siguientes

Ha usado Visual Studio Code para crear una aplicación de función con una función simple desencadenada por HTTP. En el siguiente artículo, esa función se expande mediante la adición de un enlace de salida. Este enlace escribe la cadena de la solicitud HTTP en un mensaje en una cola de Azure Queue Storage. 

> [!div class="nextstepaction"]
> [Adición de un enlace de cola de Azure Storage a una función](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
