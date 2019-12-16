---
title: Creación de su primera función en Linux en Azure
description: Aprenda a crear su primera función hospedada en Linux en Azure mediante las herramientas de la línea de comandos, Azure Functions Core Tools y la CLI de Azure.
ms.date: 03/12/2019
ms.topic: quickstart
ms.custom: mvc, fasttrack-edit
ms.openlocfilehash: 57c1b4a0ef5e8f030360bca6d69f66e8752f3da1
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74964147"
---
# <a name="quickstart-create-your-first-function-hosted-on-linux-using-command-line-tools"></a>Inicio rápido: Creación de su primera función hospedada en Linux mediante las herramientas de la línea de comandos

Azure Functions permite ejecutar el código en un entorno de Linux [sin servidor](https://azure.com/serverless) sin necesidad de crear una máquina virtual o publicar una aplicación web. El hospedaje de Linux requiere [la versión 2.x, y posteriores, del runtime de Functions](functions-versions.md). Las funciones sin servidor se ejecutan en el [plan de consumo](functions-scale.md#consumption-plan).

Esta guía de inicio rápido le explica cómo usar la CLI de Azure para crear su primera aplicación de función para que se ejecute en Linux. El código de función se crea localmente y, a continuación, se implementa en Azure mediante [Azure Functions Core Tools](functions-run-local.md).

Los pasos siguientes se admiten en equipos Mac, Windows o Linux. En este artículo se muestra cómo crear funciones en JavaScript o C#. Para más información acerca de cómo crear funciones de Python, consulte el artículo sobre la [Creación de la primera función de Python con Core Tools y la CLI de Azure](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de ejecutar este ejemplo, debe tener lo siguiente:

+ Instale [Azure Functions Core Tools](./functions-run-local.md#v2), versión 2.6.666 u otra posterior.

+ Instale la [CLI de Azure]( /cli/azure/install-azure-cli). En este artículo se necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. También puede usar [Azure Cloud Shell](https://shell.azure.com/bash).

+ Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-create-function-app-cli](../../includes/functions-create-function-app-cli.md)]

## <a name="enable-extension-bundles"></a>Habilitación de conjuntos de extensiones

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Creación de una aplicación de función de Linux en Azure

Debe tener una aplicación de función para hospedar la ejecución de las funciones en Linux. La aplicación de función proporciona un entorno sin servidor para la ejecución de su código de función. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. Cree una aplicación de función que se ejecute en Linux con el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

En el siguiente comando, use un nombre de aplicación de función único donde vea el marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. `<app_name>` también es el dominio DNS predeterminado de la aplicación de función. Este nombre debe ser único entre todas las aplicaciones de Azure. También debe establecer el entorno de ejecución de `<language>` para la aplicación de función de `dotnet` (C#), `node` (JavaScript/TypeScript) o `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Una vez creada la aplicación de función, verá el siguiente mensaje:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Ahora, puede publicar el proyecto en la nueva aplicación de función en Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]