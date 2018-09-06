---
title: 'Creación de consultas de mensajes B2B en Log Analytics: Azure Logic Apps | Microsoft Docs'
description: Creación de consultas que realizan el seguimiento de mensajes AS2, X12 y EDIFACT con Log Analytics para Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: baccd255fc2812eae0de3a98dfcef3dcbc7e1b46
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124277"
---
# <a name="create-queries-for-tracking-as2-x12-and-edifact-messages-in-log-analytics-for-azure-logic-apps"></a>Creación de consultas para el seguimiento de mensajes AS2, X12 y EDIFACT en Log Analytics para Azure Logic Apps

Para encontrar los mensajes AS2, X12 o EDIFACT en seguimiento con [Azure Log Analytics](../log-analytics/log-analytics-overview.md), puede crear consultas que filtren las acciones según criterios específicos. Por ejemplo, puede encontrar mensajes según un número de control de intercambio específico.

## <a name="requirements"></a>Requisitos

* Una aplicación lógica configurada con registro de diagnósticos. Obtenga información sobre [cómo crear una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) y [cómo configurar el registro de esa aplicación lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Una cuenta de integración configurada con supervisión y registro. Obtenga información sobre [cómo crear una cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) y [cómo configurar la supervisión y el registro de esa cuenta](../logic-apps/logic-apps-monitor-b2b-message.md).

* Si aún no lo ha hecho, [publique datos de diagnóstico para Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) y [establezca el seguimiento de mensajes en Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Una vez satisfechos los requisitos previos, debería tener un área de trabajo en Log Analytics. Use la misma área de trabajo para realizar el seguimiento de la comunicación B2B en Log Analytics. 
>  
> Si no tiene un área de trabajo de Log Analytics, aprenda a [crear un área de trabajo de Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Creación de consultas de mensajes con filtros en Log Analytics

En este ejemplo se muestra cómo puede buscar mensajes según su número de control de intercambio.

> [!TIP] 
> Si conoce el nombre del área de trabajo de Log Analytics, vaya a la página principal del área de trabajo (`https://{your-workspace-name}.portal.mms.microsoft.com`) y empiece en el paso 4. De lo contrario, empiece en el paso 1.

1. En [Azure Portal](https://portal.azure.com), elija **Todos los servicios**. Busque "log analytics" y luego elija **Log Analytics** como se muestra aquí:

   ![Búsqueda de Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. En **Log Analytics**, busque y seleccione el área de trabajo de Log Analytics.

   ![Selección del área de trabajo de Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. En **Administración**, elija **Búsqueda de registros**.

   ![Elija Búsqueda de registros](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/azure-portal-page.png)

4. En el cuadro de búsqueda, especifique un campo que quiera buscar y pulse **Entrar**. Cuando empiece a escribir, Log Analytics le mostrará posibles coincidencias y operaciones que puede usar. Más información sobre [cómo buscar datos en Log Analytics](../log-analytics/log-analytics-log-searches.md).

   En este ejemplo se buscan eventos con **Type=AzureDiagnostics**.

   ![Empezar a escribir la cadena de consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

5. En la barra de la izquierda, elija el período de tiempo que desea consultar. Para agregar un filtro a la consulta, elija **+Agregar**.

   ![Agregar filtro a consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

6. En **Agregar filtros**, escriba el nombre del filtro para poder encontrar el que quiere. Seleccione el filtro y elija **+Agregar**.

   Para buscar el número de control de intercambio, este ejemplo busca la palabra "intercambio" y selecciona **event_record_messageProperties_interchangeControlNumber_s** como filtro.

   ![Selección de filtro](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

7. En la barra de la izquierda, seleccione el valor de filtro que quiere usar y elija **Aplicar**.

   En este ejemplo se selecciona el número de control de intercambio para los mensajes que deseamos.

   ![Selección de valor de filtro](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

8. Ahora vuelva a la consulta que está creando. La consulta se actualizó con el valor y el evento de filtro seleccionados. Los resultados anteriores también se filtraron.

    ![Consulta con los resultados filtrados](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Guardar la consulta para futuro uso

1. Desde la consulta en la página **Búsqueda de registros**, elija **Guardar**. Asigne un nombre a la consulta, seleccione una categoría y elija **Guardar**.

   ![Asignación de nombre y categoría a la consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Para ver la consulta, elija **Favoritos**.

   ![Elegir "Favoritos"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. En **Búsquedas guardadas**, seleccione la consulta para poder ver los resultados. Edite la consulta para actualizarla y poder encontrar distintos resultados.

   ![Selección de la consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Búsqueda y ejecución de consultas guardadas en Log Analytics

1. Abra la página principal del área de trabajo de Log Analytics (`https://{your-workspace-name}.portal.mms.microsoft.com`) y elija **Búsqueda de registros**.

   ![Abra la página principal de Log Analytics y elija "Búsqueda de registros".](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   O bien

   ![Seleccione "Búsqueda de registros" en el menú.](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. En la página principal de **Búsqueda de registros**, elija **Favoritos**.

   ![Elegir "Favoritos"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. En **Búsquedas guardadas**, seleccione la consulta para poder ver los resultados. Edite la consulta para actualizarla y poder encontrar distintos resultados.

   ![Selección de la consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Pasos siguientes

* [Esquemas de seguimiento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de seguimiento de X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de seguimiento personalizados](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)