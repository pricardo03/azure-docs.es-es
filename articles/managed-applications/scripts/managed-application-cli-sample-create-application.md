---
title: 'Ejemplo de script de la CLI de Azure: implementación de una aplicación administrada | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: implementación de una aplicación administrada'
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: f09d9dcb60370c2cc51f5652def92e5ad9c9b512
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61226402"
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>Implementación de una aplicación administrada para el catálogo de servicios con la CLI de Azure

Este script implementa una definición de aplicación administrada desde el catálogo de servicios. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-application/create-application.sh "Create application")]


## <a name="script-explanation"></a>Explicación del script

Este script usa el siguiente comando para implementar la aplicación administrada. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az managedapp create](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-create) | Cree una aplicación administrada. Proporcione el identificador de definición y los parámetros para la plantilla. |


## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a las aplicaciones administradas, consulte la [introducción a las aplicaciones administradas de Azure](../overview.md).
* Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).
