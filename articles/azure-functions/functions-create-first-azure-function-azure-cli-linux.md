---
title: Creación de su primera función en Linux en Azure
description: Aprenda a crear su primera función hospedada en Linux en Azure mediante Azure Functions Core Tools y la CLI de Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: javascript
manager: jeconnoc
ms.openlocfilehash: b6df653f89f05a9b253ecea102ed8310ff2a53b7
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438297"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Creación de su primera función hospedada en Linux mediante Core Tools y la CLI de Azure (versión preliminar)

Azure Functions permite ejecutar el código en un entorno de Linux [sin servidor](https://azure.com/serverless) sin necesidad de crear una máquina virtual o publicar una aplicación web. El hospedaje en Linux está actualmente en versión preliminar y requiere [el entorno de ejecución Functions 2.0](functions-versions.md). Para más información acerca de las consideraciones de versión preliminar para ejecutar las aplicaciones de función en Linux, consulte [este artículo sobre funciones en Linux](https://aka.ms/funclinux).

Esta guía de inicio rápido le explica cómo usar la CLI de Azure para crear su primera aplicación de función para que se ejecute en Linux. El código de función se crea localmente y, a continuación, se implementa en Azure mediante [Azure Functions Core Tools](functions-run-local.md).

Los pasos siguientes se admiten en equipos Mac, Windows o Linux. En este artículo se muestra cómo crear funciones en JavaScript o C#. Para más información acerca de cómo crear funciones de Python, consulte el artículo sobre la [Creación de la primera función de Python con Core Tools y la CLI de Azure (versión preliminar)](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de ejecutar este ejemplo, debe tener lo siguiente:

+ Instale [la versión 2.x de Azure Core Tools](functions-run-local.md#v2).

+ Instale la [CLI de Azure]( /cli/azure/install-azure-cli). En este artículo se necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. También puede usar [Azure Cloud Shell](https://shell.azure.com/bash).

+ Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Creación del proyecto de aplicación de función local

Ejecute el comando siguiente desde la línea de comandos para crear un proyecto de aplicación de función en la carpeta `MyFunctionProj` del directorio local actual. También se crea un repositorio GitHub en `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Cuando se le pida, use las teclas de dirección para seleccionar un entorno de ejecución de trabajo de las siguientes opciones de lenguaje:

+ `dotnet`: crea un proyecto de biblioteca de clases .NET (.csproj).
+ `node`: crea un proyecto JavaScript.
+ `python`: crea un proyecto de Python. Para las funciones de Python, consulte la [guía de inicio rápido de Python](functions-create-first-function-python.md).

Cuando se ejecute el comando, verá una salida similar a la siguiente:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Creación de un plan de App Service de Linux

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Creación de una aplicación de función de Linux en Azure

Debe tener una aplicación de función para hospedar la ejecución de las funciones en Linux. La aplicación de función proporciona un entorno sin servidor para la ejecución de su código de función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. Cree una aplicación de función que se ejecute en Linux con el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

En el siguiente comando, use un nombre de aplicación de función único donde vea el marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. `<app_name>` también es el dominio DNS predeterminado de la aplicación de función. Este nombre debe ser único entre todas las aplicaciones de Azure. También debe establecer el entorno de ejecución de `<language>` para la aplicación de función de `dotnet` (C#), `node` (JavaScript), o `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

> [!NOTE]
> Si tiene un grupo de recursos existente llamado `myResourceGroup` con cualquier aplicación de App Service que no sea de Linux, deberá usar un grupo de recursos diferente. No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos.  

Una vez creada la aplicación de función, verá el siguiente mensaje:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Ahora, puede publicar el proyecto en la nueva aplicación de función en Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Este artículo le indica cómo hospedar la aplicación de función en un contenedor de Azure App Service predeterminado. También puede hospedar sus funciones en Linux en su propio contenedor personalizado.

> [!div class="nextstepaction"]
> [Creación de una función en Linux con una imagen personalizada](functions-create-function-linux-custom-image.md)
