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
ms.openlocfilehash: d07873b34a41ff20b5007a88743f6b150d4d8a3d
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212835"
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

1. Es posible agregar otras métricas al gráfico. Seleccione el gráfico y, luego, **Agregar métrica**. Seleccione otra métrica; en este ejemplo se muestra **CPU**.

    ![Adición de métrica](media/check-cluster-health/add-metric.png)

1. Si necesita ayuda para diagnosticar problemas relacionados con el estado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).