---
title: 'Obtención de grupos de recursos administrados y cambio de tamaño de las máquinas virtuales: CLI de Azure'
description: Proporciona un script de ejemplo de la CLI de Azure que obtiene un grupo de recursos administrado en una aplicación administrada de Azure. El script cambia el tamaño de las máquinas virtuales.
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
ms.openlocfilehash: ccf853f33eea054b450563747646f3d9ec560aa3
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72528907"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Obtención de recursos de un grupo de recursos administrados y cambio de tamaño de las máquinas virtuales con la CLI de Azure

Este script recupera los recursos de un grupo de recursos administrados y cambia el tamaño de las máquinas virtuales de ese grupo de recursos.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para implementar la aplicación administrada. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-list) | Enumera las aplicaciones administradas. Proporciona valores de consulta que se centran en los resultados. |
| [az resource list](https://docs.microsoft.com/cli/azure/resource#az-resource-list) | Enumera los recursos. Proporciona un grupo de recursos y valores de consulta que se centran en el resultado. |
| [az vm resize](https://docs.microsoft.com/cli/azure/vm#az-vm-resize) | Actualiza el tamaño de una máquina virtual. |


## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a las aplicaciones administradas, consulte la [introducción a las aplicaciones administradas de Azure](../overview.md).
* Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).
