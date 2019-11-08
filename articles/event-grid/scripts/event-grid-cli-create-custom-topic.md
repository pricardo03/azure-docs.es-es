---
title: 'Ejemplo de script de la CLI de Azure: creación de un tema personalizado | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: creación de un tema personalizado'
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: tomfitz
ms.openlocfilehash: 16c6586939a6dad248cef3abdabd78faf04a2381
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580381"
---
# <a name="create-event-grid-custom-topic-with-azure-cli"></a>Creación de un tema personalizado de Event Grid con la CLI de Azure

Este script crea un tema personalizado de Event Grid.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli[main](../../../cli_scripts/event-grid/create-custom-topic/create-custom-topic.sh "Create custom topic")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear el tema personalizado. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az eventgrid topic create](https://docs.microsoft.com/cli/azure/eventgrid/topic#az-eventgrid-topic-create) | Permite crear un tema personalizado de Event Grid. |


## <a name="next-steps"></a>Pasos siguientes

* Para información sobre la consulta de suscripciones, consulte [Consulta de suscripciones de Event Grid](../query-event-subscriptions.md).
* Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).
