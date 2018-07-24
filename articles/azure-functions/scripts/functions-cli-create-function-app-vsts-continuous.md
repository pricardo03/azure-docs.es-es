---
title: Creación de una función en Azure que se implemente desde Visual Studio Team Services | Microsoft Docs
description: Creación de una instancia de Function App e implementación de código de función desde Visual Studio Team Services
services: functions
keywords: ''
author: syntaxc4
ms.author: glenga
ms.date: 07/03/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 1b54cfebd3ae36fc8025aeb4ea9c91d336bc5343
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988959"
---
# <a name="create-a-function-app-and-deploy-function-code-from-visual-studio-team-services"></a>Creación de una aplicación de función e implementación de código de función desde Visual Studio Team Services

En este tema se explica cómo usar Azure Functions para crear una aplicación de función [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) con el [plan de consumo](../functions-scale.md#consumption-plan). La aplicación de función, que es un contenedor para las funciones, se implementa continuamente desde un repositorio de Visual Studio Team Services (VSTS). 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

Para realizar este tema, necesita lo siguiente:

* Un repositorio de VSTS que contenga el proyecto de aplicación de función y para el que disponga de permisos administrativos.
* Un [token de acceso personal (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) para acceder al repositorio de VSTS.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si prefiere usar la CLI de Azure de forma local, debe instalar y utilizar la versión 2.0 o posterior. Para determinar la versión de la CLI de Azure, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de ejemplo

Este ejemplo crea una instancia de Azure Function App e implementa código de la función desde Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una cuenta de almacenamiento, una aplicación de función y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Crea la cuenta de almacenamiento necesaria para la aplicación de función. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Crea una aplicación de función en un [plan de consumo](../functions-scale.md#consumption-plan) sin servidor. |
| [az functionapp deployment source config](https://docs.microsoft.com/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Asocia una aplicación de función con un repositorio GIT o Mercurial. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de CLI para Azure Functions en la [documentación de Azure Functions](../functions-cli-samples.md).
