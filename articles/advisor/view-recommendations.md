---
title: Ver recomendaciones del Asesor de actualizaciones de Azure que le interesen
description: Ver y filtrar las recomendaciones de Azure Advisor para reducir el ruido.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 04/03/2019
ms.author: kasparks
ms.openlocfilehash: 9f599a63fd5f52420f1b79e769d4f7bca9683b32
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467913"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Ver recomendaciones del Asesor de actualizaciones de Azure que le interesen

Azure Advisor proporciona recomendaciones para ayudarle a optimizar las implementaciones de Azure. Dentro de Advisor, tener acceso a algunas características que le ayudan a reducir las recomendaciones a solo aquellos que le interesan.

## <a name="configure-subscriptions-and-resource-groups"></a>Configurar las suscripciones y grupos de recursos

Advisor le permite seleccionar las suscripciones y grupos de recursos que son importantes para usted y su organización. Solo ver las recomendaciones para las suscripciones y grupos de recursos que seleccionó. De forma predeterminada, todas están seleccionadas. Opciones de configuración se aplican a la suscripción o grupo de recursos, por lo que la misma configuración que se aplica a todos los usuarios que tiene acceso a ese grupo de recursos o suscripción. Opciones de configuración pueden cambiarse en el portal de Azure o mediante programación.

Para realizar cambios en el portal de Azure:

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure portal.

1. Seleccione **configuración** en el menú.

   ![Menú de configuración del Asesor de actualizaciones](./media/view-recommendations/configuration.png)

1. Active la casilla de la **Include** columna para las suscripciones o grupos de recursos para recibir las recomendaciones de Advisor. Si la casilla está deshabilitada, puede que no tenga permiso para cambiar una configuración en ese grupo de recursos o suscripción. Obtenga más información sobre [permisos en Azure Advisor](permissions.md).

1. Haga clic en **aplicar** en la parte inferior después de realizar un cambio.

## <a name="filtering-your-view-in-the-azure-portal"></a>Filtrar la vista mediante el portal de Azure

Opciones de configuración permanecen activas hasta que se puede cambiar. Si desea limitar la vista de las recomendaciones para una visualización única, puede usar las listas desplegables proporcionadas en la parte superior del panel de Advisor. Desde los paneles de información general, alta disponibilidad, seguridad, rendimiento, costo y todas las recomendaciones, puede seleccionar las suscripciones, tipos de recursos y estado de la recomendación que desee ver.

   ![Menú de filtrado del Asesor de actualizaciones](./media/view-recommendations/filtering.png)

## <a name="dismissing-and-postponing-recommendations"></a>Descartar y posponer las recomendaciones

Azure Advisor le permite anular o posponer las recomendaciones en un único recurso. Si descartar una recomendación, no verá lo nuevo a menos que activa manualmente. Sin embargo, posponer una recomendación permite especificar una duración después de que la recomendación es automáticamente vuelve a activar. Posponer puede realizarse en Azure portal o mediante programación.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Posponer una recomendación solo en el portal de Azure 

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure portal.
1. Seleccione una categoría de recomendación para ver las recomendaciones
1. Seleccione una recomendación en la lista de recomendaciones
1. Seleccione posponer o descartar para la recomendación que desee posponer o descartar

     ![Menú de filtrado del Asesor de actualizaciones](./media/view-recommendations/postpone-dismiss.png)

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Posponer o descartar una varias recomendaciones en Azure portal

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure portal.
1. Seleccione una categoría de recomendación para ver las recomendaciones.
1. Seleccione una recomendación en la lista de recomendaciones.
1. Seleccione la casilla de verificación a la izquierda de la fila para todos los recursos que desee posponer o descartar la recomendación.
1. Seleccione **posponer** o **descartar** en la parte superior izquierda de la tabla.

     ![Menú de filtrado del Asesor de actualizaciones](./media/view-recommendations/postpone-dismiss-multiple.png)

> [!NOTE]
> Necesita permiso de colaborador o propietario para descartar o posponer la recomendación. Más información acerca de los permisos en Azure Advisor.

> [!NOTE]
> Si se deshabilitan los cuadros de selección, todavía se pueden cargar recomendaciones. Espere a que todas las recomendaciones cargar antes de intentar posponer o descartar.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Reactivar una recomendación pospuesta o descartada

Puede activar una recomendación que se ha pospuesto o descartar. Esta acción puede realizarse en Azure portal o mediante programación. En Azure Portal:

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure portal.

1. Cambiar el filtro en el panel de información general para **pospuesto**. Asistente para, a continuación, muestra recomendaciones pospuestas o descartadas.

    ![Menú de filtrado del Asesor de actualizaciones](./media/view-recommendations/activate-postponed.png)

1. Seleccione una categoría para ver **pospuesto** y **descartadas** recomendaciones.

1. Seleccione una recomendación en la lista de recomendaciones. Se abrirá recomendaciones con el **pospuesto & Descartar** ficha ya seleccionado para mostrar los recursos para el que se ha pospuesto o descartar esta recomendación.

1. Haga clic en **activar** al final de la fila. Una vez que se hace clic en, la recomendación está activa para ese recurso y por lo que se quitó de esta tabla. Ahora es visible en la recomendación del **Active** ficha.
 
     ![Menú de filtrado del Asesor de actualizaciones](./media/view-recommendations/activate-postponed-2.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo puede ver las recomendaciones que le interesan en Azure Advisor. Para más información sobre Advisor, consulte: 

- [¿Qué es Azure Advisor?](advisor-overview.md)
- [Introducción a Advisor](advisor-get-started.md)
- [Permisos en Azure Advisor](permissions.md)



