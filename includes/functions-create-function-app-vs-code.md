---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 79dbee33928fbc7560d0ea27be3af25cc510e996
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641898"
---
## <a name="create-an-azure-functions-project"></a>Creación de un proyecto de Azure Functions

La plantilla del proyecto de Azure Functions de Visual Studio Code crea un proyecto que se puede publicar en una aplicación de función en Azure. Una aplicación de función permite agrupar funciones como una unidad lógica para la administración, la implementación y el uso compartido de recursos.

1. En Visual Studio Code, seleccione el logotipo de Azure para que se muestre el área **Azure: Functions** y, a continuación, seleccione el icono Crear nuevo proyecto.

    ![Creación de un proyecto de aplicación de función](./media/functions-create-function-app-vs-code/create-function-app-project.png)

1. Elija una ubicación para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Este artículo se ha diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Seleccione el lenguaje para el proyecto de la aplicación de función. En este artículo, se usa JavaScript.
    ![Elegir el lenguaje del proyecto](./media/functions-create-function-app-vs-code/create-function-app-project-language.png)

1. Cuando se le solicite, elija **Agregar al área de trabajo**.

Visual Studio Code crea el proyecto de la aplicación de función en una nueva área de trabajo. Este proyecto contiene los archivos de configuración [host.json](../articles/azure-functions/functions-host-json.md) y [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), además de los archivos de proyecto específicos del lenguaje. También obtendrá un nuevo repositorio de Git en la carpeta del proyecto.