---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: f9b853f0e86fc298ab35421928492ba813d85827
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444612"
---
## <a name="create-an-azure-functions-project"></a>Creación de su proyecto de Functions con una función 

La plantilla del proyecto de Azure Functions de Visual Studio Code crea un proyecto que se puede publicar en una aplicación de función en Azure. Una aplicación de función permite agrupar funciones como una unidad lógica para la administración, la implementación y el uso compartido de recursos.

1. En Visual Studio Code, presione F1 para abrir la paleta de comandos. En la paleta de comandos, busque y seleccione `Azure Functions: Create new project...`.

1. Elija una ubicación de directorio para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Estos pasos se han diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Siguiendo las indicaciones, proporcione la información siguiente:

    | Prompt | Valor | DESCRIPCIÓN |
    | ------ | ----- | ----------- |
    | Seleccionar el lenguaje para el proyecto de la aplicación de funciones | C# o JavaScript | En este artículo se admite C# y JavaScript. Para Python, consulte [este artículo de Python](https://code.visualstudio.com/docs/python/tutorial-azure-functions) y, para PowerShell, consulte [este artículo de PowerShell](../articles/azure-functions/functions-create-first-function-powershell.md).  |
    | Seleccionar una plantilla para la primera función de su proyecto | Desencadenador HTTP | Cree una función desencadenada por HTTP en la nueva aplicación de funciones. |
    | Proporcionar un nombre de función | HttpTrigger | Presione ENTRAR para usar el nombre predeterminado. |
    | Proporcionar un espacio de nombres | My.Functions | (Solo C#) Las bibliotecas de clases de C# deben tener un espacio de nombres.  |
    | Nivel de autorización | Función | Requiere una [clave de función](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) para llamar al punto de conexión HTTP de la función. |
    | Seleccionar cómo desea que se abra el proyecto | Agregar a área de trabajo | Crea la aplicación de funciones en el área de trabajo actual. |

Visual Studio Code crea el proyecto de la aplicación de función en una nueva área de trabajo. Este proyecto contiene los archivos de configuración [host.json](../articles/azure-functions/functions-host-json.md) y [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), además de los archivos de proyecto específicos del lenguaje. 

También se crea una nueva función desencadenada por HTTP en la carpeta HttpTrigger del proyecto de la aplicación de funciones.