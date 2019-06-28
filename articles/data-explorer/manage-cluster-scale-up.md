---
title: Escalado vertical del clúster de Azure Data Explorer para ajustarse a los cambios en la demanda
description: En este artículo se describen los pasos para escalar y reducir verticalmente un clúster de Azure Data Explorer en función de los cambios en la demanda.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: 1f130f79b6b6924559e1693e1eef8ced2972b3d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60758708"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Administración del escalado vertical de clúster para ajustarse a los cambios en la demanda

Existen dos flujos de trabajo de escalado para un clúster de Azure Data Explorer: escalado vertical y [escalado horizontal](manage-cluster-scale-out.md). En este artículo se muestra cómo administrar el escalado vertical de los clústeres.

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal. Un enfoque mejor es *escalar* un clúster, es decir, agregar y eliminar capacidad y recursos de CPU con los cambios en la demanda. 

## <a name="steps-to-scale-up"></a>Pasos para el escalado vertical
1. Vaya al clúster. En **Configuración**, seleccione **Escalar verticalmente**.

    Se le muestra una lista de las SKU disponibles. Por ejemplo, en la ilustración siguiente, solo cuatro SKU están disponibles.

    ![Escalado vertical](media/manage-cluster-scale-up/scale-up.png)

    Las SKU están deshabilitadas porque son la SKU actual o no están disponibles en la región donde se encuentra el clúster.

1. Para cambiar la SKU, seleccione la que quiera y presione el botón **Seleccionar**.

> [!NOTE]
> El escalado vertical del proceso puede tardar unos minutos y, durante ese tiempo se suspenderá el clúster. Tenga en cuenta que la reducción vertical puede perjudicar el rendimiento del clúster.

Ha realizado una operación de escalado o reducción verticales para el clúster de Azure Data Explorer.

## <a name="next-steps"></a>Pasos siguientes
También puede [administrar el escalado horizontal del clúster](manage-cluster-scale-out.md) para escalar dinámicamente de forma horizontal el recuento de instancias en función de las métricas que especifique.

Si necesita ayuda por problemas relacionados con el escalado de un clúster, [abra una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) en Azure Portal.

Supervise el uso de recursos con este artículo: [Supervisión del rendimiento, el mantenimiento y el uso de Azure Data Explorer con métricas](using-metrics.md)
