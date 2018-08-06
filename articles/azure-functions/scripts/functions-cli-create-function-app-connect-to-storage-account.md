---
title: Creación de una función de Azure que se conecta a una instancia de Azure Storage | Microsoft Docs
description: 'Ejemplo de script de la CLI de Azure: creación de una función de Azure que se conecta a una instancia de Azure Storage'
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: ''
tags: functions
ms.assetid: ''
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 8c878625b01ae926eb8e584365810071e84be66b
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397465"
---
# <a name="create-a-function-app-that-connects-to-an-azure-storage-account"></a>Creación de una aplicación de función que se conecta a una cuenta de Azure Storage

Este script de ejemplo de Azure Functions crea una aplicación de función y conecta la función a una cuenta de Azure Storage. La configuración de la aplicación creada que contiene la conexión se puede utilizar con un [desencadenador o enlace de almacenamiento](..\functions-bindings-storage-blob.md). 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si usa la CLI localmente, asegúrese de que ejecuta la versión 2.0 de la CLI de Azure o posterior. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de ejemplo

Este ejemplo crea una aplicación de Azure Function App y agrega la cadena de conexión de almacenamiento a una configuración de aplicación.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, ejecute el comando siguiente para quitar el grupo de recursos y todos los recursos relacionados:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Crea un grupo de recursos con una ubicación. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Cree una cuenta de almacenamiento. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Crea una aplicación de función en un [plan de consumo](../functions-scale.md#consumption-plan) sin servidor. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de CLI para Azure Functions en la [documentación de Azure Functions](../functions-cli-samples.md).
