---
title: Creación de su primera función en Linux en Azure
description: Aprenda a crear su primera función hospedada en Linux en Azure mediante Azure Functions Core Tools y la CLI de Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 09/12/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: multiple
manager: jeconnoc
ms.openlocfilehash: 1045e0cc0d114bb8b35e6136a2054b3642eac7e8
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249884"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Creación de su primera función hospedada en Linux mediante Core Tools y la CLI de Azure (versión preliminar)

Azure Functions permite ejecutar el código en un entorno de Linux [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) sin necesidad de crear una máquina virtual o publicar una aplicación web. El hospedaje en Linux está actualmente en versión preliminar y requiere [el entorno de ejecución Functions 2.0](functions-versions.md).

Esta guía de inicio rápido le explica cómo usar la CLI de Azure para crear su primera aplicación de función para que se ejecute en Linux. El código de función se crea localmente y, a continuación, se implementa en Azure mediante [Azure Functions Core Tools](functions-run-local.md).

Los pasos siguientes se admiten en equipos Mac, Windows o Linux. En este artículo se muestra cómo crear funciones en JavaScript o C#.

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

## <a name="create-a-linux-function-app-in-azure"></a>Creación de una aplicación de función de Linux en Azure

Debe tener una aplicación de función para hospedar la ejecución de las funciones en Linux. La aplicación de función proporciona un entorno sin servidor para la ejecución de su código de función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. Cree una aplicación de función que se ejecute en Linux con el comando [az functionapp create](/cli/azure/functionapp#az_functionapp_create).

En el siguiente comando, use un nombre de aplicación de función único donde vea el marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. `<app_name>` también es el dominio DNS predeterminado de la aplicación de función. Este nombre debe ser único entre todas las aplicaciones de Azure.

```azurecli
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--location "westus" --is-linux
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
