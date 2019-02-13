---
title: Introducción a Azure Advisor | Microsoft Docs
description: Introducción a Azure Advisor.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: kasparks
ms.openlocfilehash: aabb316cf564520e6ea5e8689f1c18c98b94ab3c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488482"
---
# <a name="get-started-with-azure-advisor"></a>Introducción a Azure Advisor

Obtenga información acerca de cómo acceder a Advisor mediante Azure Portal y obtener e implementar recomendaciones.

> [!NOTE]
> Azure Advisor se ejecuta automáticamente en segundo plano para buscar recursos recién creados. Puede tardar hasta 24 horas en poder proporcionar recomendaciones sobre esos recursos.

## <a name="get-recommendations"></a>Obtención de recomendaciones

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. En el panel izquierdo, haga clic en **Advisor**.  Si no ve Asesor en el panel izquierdo, haga clic en **Todos los servicios**.  En el panel de menú de servicio, en **Supervisión y administración**, haga clic en **Advisor**. Se muestra el panel de Advisor.

   ![Acceso a Azure Advisor mediante Azure Portal](./media/advisor-get-started/advisor-portal-menu.png) 

1. El panel de Advisor mostrará un resumen de las recomendaciones para todas las suscripciones seleccionadas.  Puede elegir las suscripciones que desee que muestren las recomendaciones para usar el menú desplegable del filtro de suscripción.

1. Para obtener recomendaciones para una categoría específica, haga clic en una de las pestañas: **Alta disponibilidad**, **Seguridad**, **Rendimiento** o **Costo**. 

  ![Panel de Azure Advisor](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-recommendation-details-and-implement-a-solution"></a>Obtención de detalles de recomendaciones e implementación de una solución

Puede seleccionar una recomendación en Advisor para ver detalles adicionales, como las acciones de recomendación y los recursos afectados, y para implementar la solución para la recomendación.  

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

1. Seleccione una categoría de recomendación para mostrar la lista de recomendaciones dentro de esa categoría o seleccionar la pestaña **Todas** para ver todas las recomendaciones.

1. Haga clic en una recomendación que desee revisar en detalle.

1. Revise la información acerca de la recomendación y los recursos que se aplican a la recomendación.

1. Haga clic en **Acción recomendada** para implementar la recomendación.

## <a name="filter-recommendations"></a>Filtrar recomendaciones

Puede filtrar las recomendaciones para explorar en profundidad lo que es más importante para usted.  Puede filtrar por suscripción, tipo de recurso o estado de recomendación.  

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

1. Use los menús desplegables en el panel de Advisor para filtrar por suscripción, tipo de recurso o estado de recomendación.

    ![Criterios de filtro de búsqueda de Advisor](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-recommendations"></a>Posponer o descartar recomendaciones

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

1. Vaya hasta la recomendación que desee posponer o descartar.

1. Haga clic en la recomendación.

1. Haga clic en **Postpone** (Posponer). 

1. Puede especificar el tiempo que se pospondrá o seleccionar **Never** (Nunca) para descartar la recomendación.

## <a name="exclude-subscriptions-or-resource-groups"></a>Exclusión de suscripciones o grupos de recursos

Puede que tenga grupos de recursos o suscripciones para los que no desee recibir recomendaciones de Advisor: como recursos de prueba.  Puede configurar Advisor para que solo genere recomendaciones para grupos de recursos y suscripciones específicos.

> [!NOTE]
> Para incluir o excluir una suscripción o grupo de recursos de Advisor, debe ser propietario de una suscripción.  Si no tiene los permisos necesarios para una suscripción o grupo de recursos, la opción de incluir o incluir está deshabilitada en la interfaz de usuario.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

1. Haga clic en **Configurar** en la barra de acción.

1. Desactive las suscripciones o grupos de recursos para las que no desee recibir recomendaciones de Advisor.

    ![Ejemplo de recursos de configuración de Advisor](./media/advisor-get-started/advisor-configure-resources.png)

1. Haga clic en el botón **Apply** (Aplicar).

## <a name="configure-low-usage-vm-recommendation"></a>Configuración de recomendaciones sobre máquinas virtuales con poco uso

Este procedimiento configura la regla de uso promedio de CPU para las recomendaciones sobre una máquina virtual con poco uso.

Advisor supervisa la utilización de las máquinas virtuales durante 14 días e identifica aquellas con una utilización escasa. Se considera que una máquina virtual tiene una utilización escasa si su uso promedio de CPU es del 5 % o menos y el de la red es de 7 MB o menos durante cuatro o más días.

Si le gustaría que fuera más exigente en la identificación de las máquinas virtuales con poco uso, puede ajustar la regla de uso promedio de la CPU según la suscripción.  La regla de uso promedio de CPU puede establecerse en 5 %, 10 %, 15 % o 20 %.

> [!NOTE]
> Para ajustar la regla de uso promedio de CPU para identificar las máquinas virtuales de poco uso, debe ser *Propietario* de una suscripción.  Si no tiene los permisos necesarios para una suscripción o grupo de recursos, la opción de incluir o incluir estará deshabilitada en la interfaz de usuario. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

1. Haga clic en **Configurar** en la barra de acción.

1. Haga clic en la pestaña **Reglas**.

1. Seleccione las suscripciones para las que les gustaría ajustar la regla de uso promedio de CPU y, luego, haga clic en **Editar**.

1. Seleccione el valor de uso promedio de CPU y, luego, haga clic en **Aplicar**.

1. Haga clic en **Actualizar recomendaciones** para actualizar las recomendaciones existentes para usar la nueva regla de uso promedio de CPU. 

   ![Ejemplo de reglas de recomendación de configuración de Advisor](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-recommendations"></a>Descarga de recomendaciones

Advisor le permite descargar un resumen de sus recomendaciones.  Puede descargar las recomendaciones como un archivo PDF o un archivo CSV.  La descarga de recomendaciones permite el uso compartido sencillo con sus colegas o la realización de su propio análisis en la parte superior de los datos de recomendación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

1. Haga clic en **Descargar como CSV** o **Descargar como PDF** en la barra de acción.

La opción de descarga respeta los filtros aplicados en el panel de Advisor.  Si selecciona la opción de descarga mientras visualiza una recomendación o categoría de recomendación específicas, el resumen descargado solo incluye información para esa categoría o recomendación. 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Advisor, consulte:

- [Introducción a Azure Advisor](advisor-overview.md)
- [Recomendaciones sobre alta disponibilidad de Advisor](advisor-high-availability-recommendations.md)
- [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
- [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
- [Recomendaciones sobre el costo de Advisor](advisor-performance-recommendations.md)
