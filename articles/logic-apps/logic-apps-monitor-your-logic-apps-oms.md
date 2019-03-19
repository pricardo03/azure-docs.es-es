---
title: 'Supervisar aplicaciones lógicas con registros de Azure Monitor: Azure Logic Apps | Microsoft Docs'
description: Obtenga información y datos de depuración para solucionar problemas y diagnosticar las ejecuciones de aplicaciones lógicas con Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 3f890e6cabd757fdd38374befaaccd1a10c9bd96
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192469"
---
# <a name="monitor-logic-apps-with-azure-monitor-logs"></a>Supervisión de aplicaciones lógicas con los registros de Azure Monitor

Para supervisar y obtener detalles de depuración más completos acerca de las aplicaciones lógicas, activar [registros de Azure Monitor](../log-analytics/log-analytics-overview.md) al crear la aplicación lógica. Registros de Azure Monitor proporciona registro de diagnóstico y supervisión de las aplicaciones lógicas cuando se instala la solución Logic Apps Management en Azure portal. Esta solución también proporciona información agregada para las ejecuciones de aplicaciones lógicas con detalles específicos, como el estado, el tiempo de ejecución, el estado de reenvío y los identificadores de correlación. En este artículo se muestra cómo activar los registros de Azure Monitor, por lo que puede ver los eventos en tiempo de ejecución y datos de la aplicación lógica se ejecutan.

Para activar los registros de Azure Monitor para las aplicaciones lógicas existentes, siga estos pasos para [activar el registro de diagnóstico y enviar datos de tiempo de ejecución de aplicación lógica a los registros de Azure Monitor](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

> [!NOTE]
> En esta página ya se describieron los pasos para realizar estas tareas con Microsoft Operations Management Suite (OMS), que [se retirará en enero de 2019](../azure-monitor/platform/oms-portal-transition.md). Reemplace esos pasos por Azure Log Analytics. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, necesita un área de trabajo de Log Analytics. Aprenda a [crear un área de trabajo de Log Analytics](../azure-monitor/learn/quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Activación del registro de diagnóstico al crear aplicaciones lógicas

1. En [Azure Portal](https://portal.azure.com), cree una aplicación lógica. Seleccione **Crear un recurso** > **Integración** > **Aplicación lógica**.

   ![Creación de una aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

1. En **Crear aplicación lógica**, realice estas tareas:

   1. Asigne un nombre a la aplicación lógica y seleccione su suscripción de Azure. 

   1. Cree o seleccione un grupo de recursos de Azure.

   1. Establezca **Log Analytics** en **Activado**. 

   1. En la lista de áreas de trabajo de Log Analytics, seleccione el área de trabajo donde quiera enviar los datos de las ejecuciones de aplicación lógica. 

      ![Creación de la aplicación lógica](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Después de realizar este paso, Azure crea la aplicación lógica, que ahora está asociada al área de trabajo de Log Analytics. 
      Además, este paso también instala automáticamente la solución Logic Apps Management en el área de trabajo.

   1. Cuando termine, seleccione **Crear**.

1. Para ver las ejecuciones de aplicación lógica, [continúe con estos pasos](#view-logic-app-runs-oms).

## <a name="install-logic-apps-management-solution"></a>Instalación de la solución Logic Apps Management

Si ya activó de registros de Azure Monitor cuando creó la aplicación lógica, omita este paso. Ya tiene instalada la solución Logic Apps Management.

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los servicios**. En el cuadro de búsqueda, busque "log analytics" y seleccione **Log Analytics**.

   ![Selección de “Log Analytics”](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. En **Log Analytics**, busque y seleccione el área de trabajo de Log Analytics. 

   ![Selección del área de trabajo de Log Analytics](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. En **Introducción a Log Analytics** > **Configurar soluciones de supervisión**, elija **Ver soluciones**.

   ![Selección de “Ver soluciones”](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. En la página Información general, seleccione **Agregar** para abrir la lista **Soluciones de administración**. En la lista, seleccione **Logic Apps Management**. 

   ![Selección de “Logic Apps Management”](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Si no encuentra la solución, en la parte inferior de la lista, seleccione **Cargar más** hasta que se muestre la solución.

1. Elija **Crear**, confirme el área de trabajo de Log Analytics donde quiere instalar la solución y vuelva a elegir **Crear**.   

   ![Selección de "Crear" para "Logic Apps Management"](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

   Si no quiere usar un área de trabajo existente, también puede crear un área de trabajo nueva en este momento.

   Cuando termine, la solución Logic Apps Management aparecerá en la página de información general. 

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-run-information"></a>Visualización de información de ejecución de la aplicación lógica

Una vez que se ejecuta la aplicación lógica, puede ver el estado y el recuento de esas ejecuciones en el icono **Logic Apps Management**. 

1. Vaya al área de trabajo de Log Analytics y abra la página de información general. Elija **Logic Apps Management**. 

   ![Estado y recuento de ejecuciones de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   En este caso, las ejecuciones de aplicación lógica se agrupan por nombre y estado de ejecución. 
   En esta página, también se muestran detalles sobre errores en acciones o desencadenadores de las ejecuciones de aplicación lógica.

   ![Resumen de estado de las ejecuciones de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
1. Para ver todas las ejecuciones de una aplicación lógica específica o el estado, seleccione la fila correspondiente a la aplicación lógica o el estado.

   A continuación se muestra un ejemplo con todas las ejecuciones de una aplicación lógica específica:

   ![Visualización de las ejecuciones de una aplicación lógica o el estado](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Esta página contiene las siguientes opciones avanzadas:

   * **Propiedades controladas:**

     En esta columna, se muestran propiedades controladas agrupadas por acciones para la aplicación lógica. Para ver las propiedades controladas, elija **ver**. 
     Para buscar las propiedades controladas, use el filtro de columna.
   
     ![Ver propiedades controladas de una aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Todas las propiedades controladas recién agregadas pueden tardar entre 10 y 15 minutos en aparecer la primera vez. Aprenda a [agregar propiedades controladas a la aplicación lógica](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Reenviar**: se puede volver a enviar una o varias ejecuciones de aplicaciones lógicas que no se pudieron realizar correctamente, de aquellas que sí lo hicieron o de aquellas que aún están en ejecución. Seleccione las casillas de las ejecuciones que desea volver a enviar y elija **Reenviar**. 

     ![Reenviar ejecuciones de aplicaciones lógicas](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Para filtrar estos resultados, puede realizar un filtrado en el cliente y en el servidor.

   * **Filtro del lado cliente**: por cada columna, seleccione los filtros que prefiera, por ejemplo:

     ![Ejemplo de filtros de columna](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Filtro del lado servidor**: para seleccionar un intervalo de tiempo específico o limitar el número de ejecuciones que se muestran, use el control de ámbito de la parte superior de la página. De forma predeterminada, solo aparecen 1000 registros a la vez.
   
     ![Cambio de la ventana de tiempo](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
1. Para ver todas las acciones y sus detalles de una ejecución concreta, seleccione una fila de una ejecución de aplicación lógica.

   A continuación se muestra un ejemplo que muestra todas las acciones de una ejecución de aplicación lógica específica:

   ![Ver acciones de una ejecución de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
1. En cualquier página de resultados, para ver la consulta subyacente a los resultados o para ver todos los resultados, elija **Ver todo**, que abre la página de búsqueda de registros.
   
   ![Ver todo en las páginas de resultados](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   En la página de búsqueda de registros,

   * Para ver los resultados de consulta en una tabla, elija **Tabla**.

   * Para cambiar la consulta, puede modificar la cadena de consulta en la barra de búsqueda. 
   Para una mejor experiencia, elija **Análisis avanzado**.

     ![Visualización de las acciones y los detalles de una ejecución de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     En la página de log analytics, puede actualizar las consultas y ver los resultados de la tabla. En esta consulta se usa el [lenguaje de consulta de Kusto](https://aka.ms/LogAnalyticsLanguageReference), que puede modificar si quiere ver resultados diferentes. 

     ![log analytics: vista de consulta](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de mensajes B2B](../logic-apps/logic-apps-monitor-b2b-message.md)