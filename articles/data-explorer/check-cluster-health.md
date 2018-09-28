---
title: Comprobar el estado de un clúster del Explorador de datos de Azure
description: En este artículo se describen los pasos para determinar si el estado del clúster del Explorador de datos de Azure es correcto.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0746247d2c912ba66e81b95f45b168e32b522130
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988434"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Comprobar el estado de un clúster del Explorador de datos de Azure

Hay varios factores que afectan al estado de un clúster del Explorador de datos de Azure, incluidos la CPU, la memoria y el subsistema de disco. En este artículo se muestran algunos pasos básicos que puede seguir para valorar el estado de un clúster.

1. Inicie sesión en [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. En el panel izquierdo, seleccione el clúster y ejecute el siguiente comando.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Un resultado de 1 indica que el estado es correcto; un resultado de 0 indica que el clúster está en mal estado.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al clúster.

1. En **Supervisión**, seleccione primero **Métricas** y, a continuación, **Keep Alive**, tal y como se muestra en la siguiente imagen. Un resultado cercano a 1 indica que el estado del clúster es correcto.

    ![Métrica Keep Alive para clústeres](media/check-cluster-health/portal-metrics.png)

1. Agregue otras métricas, como la CPU y el almacenamiento en memoria caché, para medir los recursos que el clúster utiliza.

1. Si necesita ayuda para diagnosticar problemas relacionados con el estado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com).