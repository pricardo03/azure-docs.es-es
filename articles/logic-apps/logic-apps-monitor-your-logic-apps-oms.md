---
title: 'Supervisión de aplicaciones lógicas con Azure Monitor: Azure Logic Apps'
description: Obtención de información y datos de depuración para solucionar problemas y diagnosticar las ejecuciones de aplicaciones lógicas con registros de Azure Monitor
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 08/29/2019
ms.openlocfilehash: a038a05f03ce7a209ae82203441750749bc6c4c4
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70138805"
---
# <a name="get-insights-and-debugging-data-for-logic-apps-by-using-azure-monitor-logs"></a>Obtención de información y datos de depuración de aplicaciones lógicas mediante el uso de registros de Azure Monitor

Para supervisar y obtener información más detallada sobre la depuración de las aplicaciones lógicas, active los [registros de Azure Monitor](../log-analytics/log-analytics-overview.md) cuando cree la aplicación lógica. Los registros de Azure Monitor proporcionan registros de diagnóstico y supervisión para las aplicaciones lógicas cuando se instala la solución Logic Apps Management en Azure Portal. Esta solución también proporciona información agregada para las ejecuciones de aplicaciones lógicas con detalles específicos, como el estado, el tiempo de ejecución, el estado de reenvío y los identificadores de correlación. En este artículo se muestra cómo activar los registros de Azure Monitor para poder ver eventos y datos del runtime de las ejecuciones de su aplicación lógica.

En este tema se muestra cómo configurar los registros de Azure Monitor al crear la aplicación lógica. Para activar los registros de Azure Monitor para las aplicaciones lógicas existentes, siga estos pasos para [activar el registro de diagnóstico y enviar datos de tiempo de ejecución de aplicaciones lógicas a los registros de Azure Monitor](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

> [!NOTE]
> En esta página ya se describieron los pasos para realizar estas tareas con Microsoft Operations Management Suite (OMS), que [se retiró en enero de 2019](../azure-monitor/platform/oms-portal-transition.md), y se reemplazan esos pasos por [registros de Azure Monitor](../azure-monitor/platform/data-platform-logs.md), término que reemplazó al de Log Analytics. Los datos de registro siguen almacenándose en un área de trabajo de Log Analytics y siguen recopilándose y analizándose por el mismo servicio de Log Analytics. Para más información, consulte [Cambios de nomenclatura y terminología de Azure Monitor](../azure-monitor/terminology.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, necesita un área de trabajo de Log Analytics. Aprenda a [crear un área de trabajo de Log Analytics](../azure-monitor/learn/quick-create-workspace.md).

## <a name="turn-on-logging-for-new-logic-apps"></a>Activación del registro de nuevas aplicaciones lógicas

1. En [Azure Portal](https://portal.azure.com), cree una aplicación lógica. En el menú principal de Azure, seleccione **Crear un recurso** > **Integración** > **Aplicación lógica**.

   ![Creación de una nueva aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/create-new-logic-app.png)

1. En **Aplicación lógica**, siga estos pasos:

   1. Asigne un nombre a la aplicación lógica y seleccione su suscripción de Azure.

   1. Cree o seleccione un grupo de recursos de Azure. Seleccione la ubicación de la aplicación lógica.

   1. En **Log Analytics**, seleccione **Activado**.

   1. En la lista **Área de trabajo de Log Analytics**, seleccione el área de trabajo donde quiera enviar los datos de las ejecuciones de aplicación lógica.

      ![Especificación de información de la aplicación lógica](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app-details.png)

      Después de realizar este paso, Azure crea la aplicación lógica, que ahora está asociada al área de trabajo de Log Analytics. Además, este paso instala automáticamente la solución Logic Apps Management en el área de trabajo.

   1. Seleccione **Crear** cuando haya terminado.

1. Para ver las ejecuciones de aplicación lógica, [continúe con estos pasos](#view-logic-app-runs-oms).

## <a name="install-logic-apps-management-solution"></a>Instalación de la solución Logic Apps Management

Si ya configuró los registros de Azure Monitor cuando creó la aplicación lógica, puede saltarse este paso. Ya tiene instalada la solución Logic Apps Management.

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los servicios**. En el cuadro de búsqueda, busque "áreas de trabajo de log analytics" y seleccione **Áreas de trabajo de Log Analytics**.

   ![Selección de "Áreas de trabajo de Log Analytics"](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. En **Áreas de trabajo de Log Analytics**, seleccione su área de trabajo.

   ![Selección del área de trabajo de Log Analytics](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. En el panel Información general, en **Introducción a Log Analytics** > **Configurar soluciones de supervisión**, elija **Ver soluciones**.

   ![Selección de "Ver soluciones"](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. En **Información general**, seleccione **Agregar**.

   ![Seleccione "Agregar".](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

1. Una vez que se abra **Marketplace**, en el cuadro de búsqueda, escriba "logic apps management" y seleccione **Logic Apps Management**.

   ![Selección de “Logic Apps Management”](./media/logic-apps-monitor-your-logic-apps-oms/select-logic-apps-management.png)

1. En el panel de descripción de la solución, seleccione **Crear**.

   ![Selección de "Crear" para "Logic Apps Management"](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

1. Revise y confirme el área de trabajo de Log Analytics donde quiere instalar la solución y vuelva a seleccionar **Crear**.

   ![Selección de "Crear" para "Logic Apps Management"](./media/logic-apps-monitor-your-logic-apps-oms/confirm-log-analytics-workspace.png)

   Una vez que Azure implemente la solución en el grupo de recursos de Azure que contiene el área de trabajo de Log Analytics, la solución aparecerá en el panel de resumen del área de trabajo.

   ![Panel de resumen del área de trabajo](./media/logic-apps-monitor-your-logic-apps-oms/workspace-summary-pane-logic-apps-management.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-run-information"></a>Visualización de información de ejecución de la aplicación lógica

Una vez que se ejecuta la aplicación lógica, puede ver el estado y el recuento de esas ejecuciones en el icono **Logic Apps Management**.

1. Vaya al área de trabajo de Log Analytics y seleccione **Resumen del área de trabajo** > **Logic Apps Management**.

   ![Estado y recuento de ejecuciones de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-runs-summary.png)

   En este caso, las ejecuciones de aplicación lógica se agrupan por nombre y estado de ejecución. En esta página, también se muestran detalles sobre errores en acciones o desencadenadores de las ejecuciones de aplicación lógica.

   ![Resumen de estado de las ejecuciones de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-runs-summary-details.png)

1. Para ver todas las ejecuciones de una aplicación lógica específica o el estado, seleccione la fila correspondiente a la aplicación lógica o el estado.

   A continuación se muestra un ejemplo con todas las ejecuciones de una aplicación lógica específica:

   ![Vista de las ejecuciones y el estado de la aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Esta página tiene opciones avanzadas: 

   * Columna **Propiedades controladas**: en una aplicación lógica en la que estén configuradas las propiedades controladas, que se agrupan por acciones, se pueden ver las propiedades en esta columna. Para ver las propiedades controladas, seleccione **Ver**. Para buscar las propiedades controladas, use el filtro de columna.

      ![Ver propiedades controladas de una aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

      Todas las propiedades controladas recién agregadas pueden tardar entre 10 y 15 minutos en aparecer la primera vez. Aprenda a [agregar propiedades controladas a la aplicación lógica](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Volver a enviar**: puede volver a enviar una o varias ejecuciones de la aplicación lógica que registraron errores, que se realizaron correctamente o que siguen en ejecución. Active las casillas de las ejecuciones que desea volver a enviar y seleccione **Volver a enviar**.

     ![Reenviar ejecuciones de aplicaciones lógicas](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Para filtrar los resultados, puede aplicar un filtro en el cliente y en el servidor.

   * **Filtro del lado cliente**: por cada columna, seleccione los filtros que prefiera, por ejemplo:

     ![Ejemplo de filtros de columna](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Filtro del lado servidor**: para seleccionar un intervalo de tiempo específico o limitar el número de ejecuciones que se muestran, use el control de ámbito de la parte superior de la página. De forma predeterminada, solo aparecen 1000 registros a la vez.

     ![Cambio de la ventana de tiempo](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)

1. Para ver todas las acciones de una ejecución específica y sus detalles, seleccione la fila de una ejecución de aplicación lógica.

   A continuación, se muestra un ejemplo de todas las acciones y desencadenadores de una ejecución de aplicación lógica específica:

   ![Ver acciones de una ejecución de aplicación lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)

1. En cualquier página de resultados, para ver la consulta subyacente a los resultados o para ver todos los resultados, seleccione **Ver todo**, que abre la página **Registros**.

   ![Ver todos los resultados](media/logic-apps-monitor-your-logic-apps-oms/logic-app-see-all.png)

   En la página **Registros**, puede seleccionar estas opciones:

   * Para ver los resultados de consulta en una tabla, seleccione **Tabla**.

   * Las consultas utilizan el [lenguaje de consulta Kusto](https://aka.ms/LogAnalyticsLanguageReference), que puede modificar si desea ver otros resultados. Si quiere modificar la consulta, actualice la cadena de consulta y seleccione **Ejecutar** para ver los resultados en la tabla. 

     ![Log Analytics: vista de consultas](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de mensajes B2B](../logic-apps/logic-apps-monitor-b2b-message.md)