---
title: Escalado vertical del clúster de Azure Data Explorer para ajustarse a los cambios en la demanda
description: En este artículo se describen los pasos para escalar y reducir verticalmente un clúster de Azure Data Explorer en función de los cambios en la demanda.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: dc9ca8bb592e699d19835efeafb91e81408ae297
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571270"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Administración del escalado vertical de clúster para ajustarse a los cambios en la demanda

Existen dos flujos de trabajo de escalado para un clúster de Azure Data Explorer:
1. [Escalado horizontal](manage-cluster-horizontal-scaling.md), también denominado reducción horizontal.
2. Escalado vertical, también denominado reducción vertical.

En este artículo se muestra cómo administrar el escalado vertical de los clústeres.

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal. Un enfoque mejor es *escalar* un clúster, es decir, agregar y eliminar capacidad y recursos de CPU con los cambios en la demanda. 

## <a name="steps-to-configure-vertical-scaling"></a>Pasos para configurar el escalado vertical

1. Vaya al clúster. En **Configuración**, seleccione **Escalar verticalmente**.

    Se le muestra una lista de las SKU disponibles. Por ejemplo, en la ilustración siguiente, solo cuatro SKU están disponibles.

    ![Escalado vertical](media/manage-cluster-vertical-scaling/scale-up.png)

    Las SKU están deshabilitadas porque son la SKU actual o no están disponibles en la región donde se encuentra el clúster.

1. Para cambiar la SKU, seleccione la que quiera y presione el botón **Seleccionar**.

> [!NOTE]
> El proceso de escalado vertical puede tardar unos minutos y, durante ese tiempo, se suspenderá el clúster. Tenga en cuenta que la reducción vertical puede perjudicar el rendimiento del clúster.

Ha realizado una operación de escalado o reducción verticales para el clúster de Azure Data Explorer.

Si necesita ayuda por problemas relacionados con el escalado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Pasos siguientes

* [Administración del escalado horizontal de clústeres](manage-cluster-horizontal-scaling.md) para escalar horizontalmente de forma dinámica el recuento de instancias en función de las métricas que especifique.

* Supervise el uso de recursos con este artículo: [Supervisión del rendimiento, el mantenimiento y el uso de Azure Data Explorer con métricas](using-metrics.md)

