---
title: Creación de consultas de seguimiento de mensajes B2B
description: Creación de consultas que realizan el seguimiento de mensajes AS2, X12 y EDIFACT en Azure Log Analytics para Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 36cf45aa0f7d46b62caa586d1939ec52e67b1a3e
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792863"
---
# <a name="create-tracking-queries-for-b2b-messages-in-azure-monitor-logs-for-azure-logic-apps"></a>Creación de consultas de seguimiento de mensajes B2B en registros de Azure Monitor para Azure Logic Apps

Para encontrar los mensajes AS2, X12 o EDIFACT en seguimiento con [registros de Azure Monitor](../log-analytics/log-analytics-overview.md), puede crear consultas que filtren las acciones según criterios específicos. Por ejemplo, puede encontrar mensajes según un número de control de intercambio específico.

> [!NOTE]
> En esta página ya se describieron los pasos para realizar estas tareas con Microsoft Operations Management Suite (OMS), que [se retirará en enero de 2019](../azure-monitor/platform/oms-portal-transition.md). Reemplace esos pasos por Azure Log Analytics. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

* Una aplicación lógica configurada con registro de diagnósticos. Obtenga información sobre [cómo crear una aplicación lógica](quickstart-create-first-logic-app-workflow.md) y [cómo configurar el registro de esa aplicación lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Una cuenta de integración configurada con supervisión y registro. Obtenga información sobre [cómo crear una cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) y [cómo configurar la supervisión y el registro de esa cuenta](../logic-apps/logic-apps-monitor-b2b-message.md).

* Si aún no lo ha hecho, [publique datos de diagnóstico para registros de Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) y [establezca el seguimiento de mensajes en los registros de Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="create-queries-with-filters"></a>Creación de consultas con filtros

Para buscar mensajes según determinadas propiedades o valores, puede crear consultas que utilicen filtros. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los servicios**. En el cuadro de búsqueda, busque "log analytics" y seleccione **Log Analytics**.

   ![Selección de “Log Analytics”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/find-log-analytics.png)

1. En **Log Analytics**, busque y seleccione el área de trabajo de Log Analytics. 

   ![Selección de un área de trabajo de Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/select-log-analytics-workspace.png)

1. En el menú del área de trabajo, en **General**, seleccione **Registros (clásico)** o **Registros**. 

   En este ejemplo se muestra cómo utilizar la vista de registros clásica. 
   Si elige **Ver registros** en la sección **Maximice su experiencia de Log Analytics**, en **Buscar y analizar los registros**, obtendrá la **vista Registros (clásico)** . 

   ![Ver registros clásicos](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/view-classic-logs.png)

1. En el cuadro de edición de consultas, empiece a escribir el nombre del campo que desea buscar. Cuando empiece a escribir, el editor de consultas le mostrará posibles coincidencias y operaciones que puede usar. Después de crear la consulta, elija **Ejecutar** o presione la tecla ENTRAR.

   Este ejemplo busca coincidencias en **LogicAppB2B**. 
   Obtenga más información sobre [cómo buscar datos en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md).

   ![Empezar a escribir la cadena de consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/create-query.png)

1. Para cambiar el período de tiempo que desea ver, en el panel izquierdo seleccione el tiempo en la lista de duración o arrastre el control deslizante. 

   ![Cambiar período de tiempo](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/change-timeframe.png)

1. Para agregar un filtro a la consulta, elija **Agregar**. 

   ![Agregar filtro a consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/add-filter.png)

1. En **Agregar filtros**, escriba el nombre del filtro que desea buscar. Si encuentra el filtro, selecciónelo. En el panel izquierdo, elija **Agregar** de nuevo.

   Por ejemplo, esta es una consulta diferente que busca en eventos de **Type=="AzureDiagnostics"** y busca resultados según el número de control de intercambio mediante el filtro **event_record_messageProperties_interchangeControlNumber_s**.

   ![Selección de valor de filtro](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/filter-example.png)

   Después de elegir **Agregar**, la consulta se actualizó con el valor y el evento de filtro seleccionados. 
   Los resultados anteriores también se filtraron. 

   Por ejemplo, esta consulta busca en eventos de **Type=="AzureDiagnostics"** y busca resultados según el número de control de intercambio mediante el filtro **event_record_messageProperties_interchangeControlNumber_s**.

   ![Resultados filtrados](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-query"></a>Guardar consulta

Para guardar la consulta en la vista **Registros (clásico)** , siga estos pasos:

1. En la consulta de la página **Registros (clásico)** , elija **Analytics**. 

   ![Elegir "Analytics"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-analytics.png)

1. En la barra de herramientas de la consulta, elija **Guardar**.

   ![Elija "Save" (Guardar).](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/save-query.png)

1. Proporcione los detalles acerca de la consulta, por ejemplo, asígnele un nombre, seleccione **Consulta** y proporcione un nombre de categoría. Cuando termine, seleccione **Guardar**.

   ![Elija "Save" (Guardar).](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query-details.png)

1. Para ver las consultas guardadas, vuelva a la página de consulta. En la barra de herramientas de la consulta, elija **Búsquedas guardadas**.

   ![Elegir "Búsquedas guardadas"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-saved-searches.png)

1. En **Búsquedas guardadas**, seleccione la consulta para poder ver los resultados. 

   ![Selección de la consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/saved-query-results.png)

   Edite la consulta para actualizarla y poder encontrar distintos resultados.

## <a name="find-and-run-saved-queries"></a>Búsqueda y ejecución de consultas guardadas

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los servicios**. En el cuadro de búsqueda, busque "log analytics" y seleccione **Log Analytics**.

   ![Selección de “Log Analytics”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/find-log-analytics.png)

1. En **Log Analytics**, busque y seleccione el área de trabajo de Log Analytics. 

   ![Selección de un área de trabajo de Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/select-log-analytics-workspace.png)

1. En el menú del área de trabajo, en **General**, seleccione **Registros (clásico)** o **Registros**. 

   En este ejemplo se muestra cómo utilizar la vista de registros clásica. 

1. Una vez que se abra la página de consulta, en la barra de herramientas de consulta, elija **Búsquedas guardadas**.

   ![Elegir "Búsquedas guardadas"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-saved-searches.png)

1. En **Búsquedas guardadas**, seleccione la consulta para poder ver los resultados. 

   ![Selección de la consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/saved-query-results.png) 

   La consulta se ejecuta automáticamente, pero si, por cualquier motivo, esto no sucede así, en el editor de consultas elija **Ejecutar**.

## <a name="next-steps"></a>Pasos siguientes

* [Esquemas de seguimiento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de seguimiento de X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de seguimiento personalizados](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)