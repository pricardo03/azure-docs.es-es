---
title: Escalado del clúster del Explorador de datos de Azure para ajustarse a los cambios en la demanda
description: Este artículo describe los pasos para escalar verticalmente y reducir verticalmente un clúster de explorador de datos de Azure basado en la cambiante demanda.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 71f1d197b4a3fb437607f876ea524e582e62c0fd
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756326"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Administración del escalado vertical de clúster para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. Un tamaño de clúster estático puede provocar infrautilización o sobreutilización, ninguno de los cuales es ideal.

Un enfoque mejor es *escala* un clúster, agregar y quitar la capacidad y recursos de CPU con el cambio a petición. Hay dos flujos de trabajo para el escalado: escalado vertical y horizontal. En este artículo se muestra cómo administrar el escalado del clúster.

1. Vaya a su clúster. En **configuración**, seleccione **escalar verticalmente**.

    Se muestra una lista de las SKU disponibles. Por ejemplo, en la ilustración siguiente, solo cuatro SKU están disponibles.

    ![Escalado vertical](media/manage-cluster-scale-up/scale-up.png)

    SKU están deshabilitadas porque son la SKU actual, o que no están disponibles en la región donde se encuentra el clúster.

1. Para cambiar la SKU, seleccione la SKU que desee y elija el **seleccione** botón.

> [!NOTE]
> El proceso de escalado puede tardar unos minutos y, durante ese tiempo se suspenderá el clúster. Tenga en cuenta que la reducción vertical puede perjudicar el rendimiento del clúster.

Ahora ya ha terminado una operación de escalado o reducción vertical para el clúster del explorador de datos de Azure. También puede [administrar el clúster de escalabilidad horizontal](manage-cluster-scale-out.md) a dinámicamente escalar horizontalmente el recuento de instancias basado en métricas que especifique.

Si necesita ayuda con problemas de escalado de clústeres, [abrir una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) en Azure portal.
