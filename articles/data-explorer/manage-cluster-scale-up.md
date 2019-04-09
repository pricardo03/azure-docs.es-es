---
title: Escalar verticalmente un clúster del explorador de datos de Azure para dar cabida a cambios en la demanda
description: Este artículo describe los pasos para escalar verticalmente y reducir verticalmente un clúster de explorador de datos de Azure basado en la cambiante demanda.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: 1f130f79b6b6924559e1693e1eef8ced2972b3d5
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59279404"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Administración del escalado vertical de clúster para ajustarse a los cambios en la demanda

Hay dos flujos de trabajo para escalar un clúster del explorador de datos de Azure: escalado vertical y [escalada](manage-cluster-scale-out.md). En este artículo se muestra cómo administrar el escalado del clúster.

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. Un tamaño de clúster estático puede provocar infrautilización o sobreutilización, ninguno de los cuales es ideal. Un enfoque mejor es *escala* un clúster, agregar y quitar la capacidad y recursos de CPU con el cambio a petición. 

## <a name="steps-to-scale-up"></a>Pasos para escalar verticalmente
1. Vaya a su clúster. En **configuración**, seleccione **escalar verticalmente**.

    Se muestra una lista de las SKU disponibles. Por ejemplo, en la ilustración siguiente, solo cuatro SKU están disponibles.

    ![Escalado vertical](media/manage-cluster-scale-up/scale-up.png)

    SKU están deshabilitadas porque son la SKU actual, o que no están disponibles en la región donde se encuentra el clúster.

1. Para cambiar la SKU, seleccione la SKU que desee y elija el **seleccione** botón.

> [!NOTE]
> El proceso de escalado puede tardar unos minutos y, durante ese tiempo se suspenderá el clúster. Tenga en cuenta que la reducción vertical puede perjudicar el rendimiento del clúster.

Ahora ya ha terminado una operación de escalado o reducción vertical para el clúster del explorador de datos de Azure.

## <a name="next-steps"></a>Pasos siguientes
También puede [administrar el clúster de escalabilidad horizontal](manage-cluster-scale-out.md) a dinámicamente escalar horizontalmente el recuento de instancias basado en métricas que especifique.

Si necesita ayuda con problemas de escalado de clústeres, [abrir una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) en Azure portal.

Supervisar el uso de recursos al seguir este artículo: [Supervisar el rendimiento, mantenimiento y uso con métricas de explorador de Azure Data](using-metrics.md).
