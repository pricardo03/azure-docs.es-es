---
title: Creación de una aplicación de función en Linux en un plan de Azure App Service
description: Obtenga información sobre cómo crear una aplicación de función que se ejecute en Linux en un plan de App Service mediante la CLI de Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: ec7b71c7da19ecefc14696c029e63a074b498ec8
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696760"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan-preview"></a>Creación de una aplicación de función en Linux en un plan de Azure App Service (versión preliminar)

Azure Functions permite hospedar sus funciones en Linux en un contenedor de Azure App Service predeterminado. En este artículo se ofrece orientación sobre cómo usar la CLI de Azure para crear una aplicación de función hospedada en Linux en Azure que se ejecute en un [plan de App Service](functions-scale.md#app-service-plan). También puede [traer su propio contenedor personalizado](functions-create-function-linux-custom-image.md). El hospedaje en Linux actualmente se encuentra en versión preliminar.

En un plan de App Service, el usuario es responsable de escalar la aplicación de función. Para aprovechar las funcionalidades de Azure Functions sin servidor, también puede hospedar sus funciones en Linux en un [plan de consumo](functions-scale.md#consumption-plan).

Puede seguir los pasos siguientes con un equipo Mac, Windows o Linux.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

+ Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI de forma local, para este tema es preciso la CLI de Azure versión 2.0.21 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Creación de un plan de App Service de Linux

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Creación de aplicación de función en Linux

Debe tener una aplicación de función para hospedar la ejecución de las funciones en Linux. La aplicación de función proporciona un entorno para la ejecución del código de su aplicación. Le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. Cree una aplicación de función usando el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create) con un plan de App Service de Linux.

En el siguiente comando, sustituya un nombre de aplicación de función único donde vea el marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. El `<app_name>` se usa como el dominio DNS predeterminado para la Function App y, por ello, el nombre debe ser único en todas las aplicaciones de Azure. También debe establecer el entorno de ejecución de `<language>` para la aplicación de función de `dotnet` (C#), `node` (JavaScript), o `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Después de que se haya creado la aplicación de función, la CLI de Azure muestra información similar al siguiente ejemplo:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Dado que `myAppServicePlan` es un plan de Linux, la imagen de Docker integrada se utiliza para crear el contenedor que ejecuta la aplicación de función en Linux.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources-simple.md)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo crear una aplicación de función hospedada en Linux en Azure. Ahora puede [implementar un proyecto de función](https://docs.microsoft.com/cli/azure/functionapp/deployment/source?view=azure-cli-latest) en esta aplicación de función. Puede usar Azure Functions Core Tools para [crear un proyecto de Functions](functions-run-local.md) en el equipo local e implementarlo en la nueva aplicación de función de Linux.  

> [!div class="nextstepaction"] 
> [Codificación y comprobación de Azure Functions en un entorno local](functions-run-local.md)
