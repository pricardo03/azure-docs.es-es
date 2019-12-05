---
title: Seguimiento de mensajes B2B con los registros de Azure Monitor
description: Seguimiento de la comunicación B2B de las cuentas de integración y Azure Logic Apps con Azure Log Analytics
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 3726b0c8c22614d2acc797295543e69f9358d69c
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792933"
---
# <a name="track-b2b-messages-with-azure-monitor-logs"></a>Seguimiento de mensajes B2B con los registros de Azure Monitor

Después de establecer la comunicación B2B entre entidades en la cuenta de integración, los asociados pueden intercambiar mensajes con protocolos como AS2, X12 y EDIFACT. Para comprobar si estos mensajes se procesan correctamente, puede realizar un seguimiento de ellos con los [registros de Azure Monitor](../log-analytics/log-analytics-overview.md). Por ejemplo, puede usar estas capacidades de seguimiento basado en web para el seguimiento de mensajes:

* Número y estado de los mensajes
* Del estado de las confirmaciones
* Correlación de mensajes con confirmaciones
* Descripción detallada de errores
* De las funcionalidades de búsqueda

> [!NOTE]
> En esta página ya se describieron los pasos para realizar estas tareas con Microsoft Operations Management Suite (OMS), que [se retirará en enero de 2019](../azure-monitor/platform/oms-portal-transition.md). Reemplace esos pasos por Azure Log Analytics. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

* Una aplicación lógica configurada con registro de diagnósticos. Obtenga información sobre [cómo crear una aplicación lógica](quickstart-create-first-logic-app-workflow.md) y [cómo configurar el registro de esa aplicación lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Una cuenta de integración configurada con supervisión y registro. Obtenga información sobre [cómo crear una cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) y [cómo configurar la supervisión y el registro de esa cuenta](../logic-apps/logic-apps-monitor-b2b-message.md).

* Si aún no lo ha hecho, [publique los datos de diagnóstico en los registros de Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

* Una vez que cumpla con los requisitos anteriores, también necesitará un área de trabajo de Log Analytics que usará para realizar el seguimiento de la comunicación B2B mediante Log Analytics. Si no tiene un área de trabajo de Log Analytics, aprenda a [crear un área de trabajo de Log Analytics](../azure-monitor/learn/quick-create-workspace.md).

## <a name="install-logic-apps-b2b-solution"></a>Instalación de una solución Logic Apps B2B

Para que los registros de Azure Monitor realicen el seguimiento de los mensajes B2B de la aplicación lógica, agregue la solución **Logic Apps B2B** a los registros de Azure Monitor. Obtenga más información sobre la [incorporación de soluciones a los registros de Azure Monitor](../azure-monitor/learn/quick-create-workspace.md).

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los servicios**. En el cuadro de búsqueda, busque "log analytics" y seleccione **Log Analytics**.

   ![Selección de “Log Analytics”](media/logic-apps-track-b2b-messages-omsportal/find-log-analytics.png)

1. En **Log Analytics**, busque y seleccione el área de trabajo de Log Analytics. 

   ![Selección de un área de trabajo de Log Analytics](media/logic-apps-track-b2b-messages-omsportal/select-log-analytics-workspace.png)

1. En **Introducción a Log Analytics** > **Configurar soluciones de supervisión**, elija **Ver soluciones**.

   ![Selección de “Ver soluciones”](media/logic-apps-track-b2b-messages-omsportal/log-analytics-workspace.png)

1. En la página Información general, seleccione **Agregar** para abrir la lista **Soluciones de administración**. En la lista, seleccione **Logic Apps B2B**. 

   ![Seleccionar solución Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

   Si no encuentra la solución, en la parte inferior de la lista, seleccione **Cargar más** hasta que se muestre la solución.

1. Elija **Crear**, confirme el área de trabajo de Log Analytics donde quiere instalar la solución y vuelva a elegir **Crear**.   

   ![Seleccionar "Crear" para Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   Si no quiere usar un área de trabajo existente, también puede crear un área de trabajo nueva en este momento.

1. Cuando haya terminado, vuelva a la página de **información general** del área de trabajo. 

   Ahora, la solución Logic Apps B2B aparece en esta página. 
   Una vez procesados los mensajes B2B, aparece en esta página el número de mensajes actualizado.

<a name="message-status-details"></a>

## <a name="view-b2b-message-information"></a>Visualización de información de los mensajes B2B

Una vez procesados los mensajes B2B, puede ver el estado y los detalles de esos mensajes en el icono **Logic Apps B2B**.

1. Vaya al área de trabajo de Log Analytics y abra la página de información general. Seleccione **Logic Apps B2B**.

   ![Recuento de mensajes actualizado](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > De forma predeterminada, el icono de **Logic Apps B2B** muestra datos basados en un solo día. Para cambiar el ámbito de datos a otro intervalo, elija el control de ámbito de la parte superior de la página:
   > 
   > ![Cambiar intervalo](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)

1. Después de que aparezca el panel de estado de los mensajes, puede ver más detalles sobre un tipo de mensaje concreto, lo que muestra los datos basados en un solo día. Seleccione el icono de **AS2**, **X12** o **EDIFACT**.

   ![Visualización del estado de los mensajes](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Aparece una lista de mensajes para el icono seleccionado. 
   Para más información sobre las propiedades de cada tipo de mensaje, vea estas descripciones de las propiedades de los mensajes:

   * [Propiedades de mensajes AS2](#as2-message-properties)
   * [Propiedades de mensajes X12](#x12-message-properties)
   * [Propiedades de mensajes EDIFACT](#EDIFACT-message-properties)

   Por ejemplo, este podría ser el aspecto de una lista de mensajes AS2:

   ![Visualización de mensajes AS2](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Para ver o exportar las entradas y salidas de mensajes concretos, seleccione esos mensajes y elija **Descargar**. Cuando se le pida, guarde el archivo .zip en el equipo local y luego extraiga ese archivo. 

   La carpeta extraída incluye una carpeta para cada mensaje seleccionado. 
   Si configura confirmaciones, la carpeta de mensajes incluye además archivos con detalles de confirmación. 
   Cada carpeta de mensaje tiene como mínimo estos archivos: 
   
   * Archivos de lenguaje natural con los detalles de carga de entrada y carga de salida
   * Archivos codificados con las entradas y salidas

   Aquí encontrará los formatos de nombre de carpeta y archivo de cada tipo de mensaje:

   * [Formatos de nombre de carpeta y archivo AS2](#as2-folder-file-names)
   * [Formatos de nombre de carpeta y archivo X12](#x12-folder-file-names)
   * [Formatos de nombre de carpeta y archivo EDIFACT](#edifact-folder-file-names)

   ![Descarga de archivos de mensajes](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Para ver todas las acciones que tienen el mismo identificador de ejecución, en la página **Búsqueda de registros**, elija un mensaje de la lista de mensajes.

   Puede ordenar estas acciones por columna o buscar resultados concretos.

   ![Acciones con el mismo identificador de ejecución](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Para buscar resultados con consultas predeterminadas, elija **Favoritos**.

   * Aprenda [cómo generar consultas mediante la adición de filtros](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   O bien aprenda [cómo buscar datos con búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md).

   * Para cambiar la consulta del cuadro de búsqueda, actualícela con las columnas y los valores que quiera usar como filtros.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Descripciones de propiedades y formatos de nombre de los mensajes AS2, X12 y EDIFACT

Estas son las descripciones de propiedades y los formatos de nombre de los archivos de mensajes descargados por tipo de mensaje.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Descripciones de propiedades de los mensajes AS2

Estas son las descripciones de propiedades de cada mensaje AS2.

| Propiedad | DESCRIPCIÓN |
| --- | --- |
| Remitente | Asociado invitado especificado en **Configuración de recepción** o asociado del host especificado en **Configuración de envío** para un acuerdo de AS2 |
| Receptor | Asociado del host especificado en **Configuración de recepción** o asociado invitado especificado en **Configuración de envío** para un acuerdo de AS2 |
| Aplicación lógica | Aplicación lógica donde se configuran las acciones AS2 |
| Status | Estado del mensaje AS2 <br>Correcto = recibido o enviado un mensaje AS2 válido. No se configura ninguna MDN. <br>Correcto = recibido o enviado un mensaje AS2 válido. Se configura y se recibe una MDN, o se envía. <br>Error = recibido un mensaje AS2 no válido. No se configura ninguna MDN. <br>Pendiente = recibido o enviado un mensaje AS2 válido. Se configura una MDN y se espera una MDN. |
| Ack | Estado del mensaje MDN <br>Aceptado = recibida o enviada una MDN positiva. <br>Pendiente = esperando a recibir o enviar una MDN. <br>Rechazado = recibida o enviada una MDN negativa. <br>No necesario = no configurada ninguna MDN en el acuerdo. |
| Dirección | Dirección del mensaje AS2 |
| Id. de correlación | Identificador que correlaciona todos los desencadenadores y las acciones de una aplicación lógica |
| Id. de mensaje | Identificador del mensaje AS2 a partir de los encabezados de mensajes AS2 |
| Timestamp | Hora a la que la acción AS2 procesó el mensaje |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>Formatos de nombre AS2 para archivos de mensajes descargados

Estos son los formatos de nombre de cada carpeta y archivo de mensajes AS2 descargados.

| Archivo o carpeta | Formato de nombre |
| :------------- | :---------- |
| Carpeta de mensajes | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Archivos de entrada, salida y, si se ha configurado, de confirmación | **Carga de entrada**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_input_payload.txt </p>**Carga de salida**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_output\_payload.txt </p></p>**Entradas**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_inputs.txt </p></p>**Salidas**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Descripciones de propiedades de los mensajes X12

Estas son las descripciones de propiedades de cada mensaje X12.

| Propiedad | DESCRIPCIÓN |
| --- | --- |
| Remitente | Asociado invitado especificado en **Configuración de recepción** o asociado del host especificado en **Configuración de envío** para un acuerdo de X12 |
| Receptor | Asociado del host especificado en **Configuración de recepción** o asociado invitado especificado en **Configuración de envío** para un acuerdo de X12 |
| Aplicación lógica | Aplicación lógica donde se configuran las acciones X12 |
| Status | Estado del mensaje X12 <br>Correcto = recibido o enviado un mensaje X12 válido. No se configura ninguna confirmación funcional. <br>Correcto = recibido o enviado un mensaje X12 válido. Se configura y se recibe una confirmación funcional, o se envía una confirmación funcional. <br>Error = recibido o enviado un mensaje X12 no válido. <br>Pendiente = recibido o enviado un mensaje X12 válido. Se configura una confirmación funcional y se espera una confirmación funcional. |
| Ack | Estado de confirmación funcional (997) <br>Aceptado = recibida o enviada una confirmación funcional positiva. <br>Rechazado = recibida o enviada una confirmación funcional negativa. <br>Pendiente = se espera una confirmación funcional, pero no se ha recibido. <br>Pendiente = generada una confirmación funcional, pero no se puede enviar al asociado. <br>No necesario = confirmación funcional no configurada. |
| Dirección | Dirección del mensaje X12 |
| Id. de correlación | Identificador que correlaciona todos los desencadenadores y las acciones de una aplicación lógica |
| Tipo de mensaje | Tipo de mensaje X12 de EDI |
| ICN | Número de control de intercambio del mensaje X12 |
| TSCN | Número de control de conjuntos de transacciones del mensaje X12 |
| Timestamp | Hora a la que la acción X12 procesó el mensaje |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>Formatos de nombre X12 para archivos de mensajes descargados

Estos son los formatos de nombre de cada carpeta y archivo de mensajes X12 descargados.

| Archivo o carpeta | Formato de nombre |
| :------------- | :---------- |
| Carpeta de mensajes | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Archivos de entrada, salida y, si se ha configurado, de confirmación | **Carga de entrada**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_input_payload.txt </p>**Carga de salida**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_output\_payload.txt </p></p>**Entradas**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_inputs.txt </p></p>**Salidas**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Descripciones de propiedades de los mensajes EDIFACT

Estas son las descripciones de propiedades de cada mensaje EDIFACT.

| Propiedad | DESCRIPCIÓN |
| --- | --- |
| Remitente | Asociado invitado especificado en **Configuración de recepción** o asociado del host especificado en **Configuración de envío** para un acuerdo de EDIFACT |
| Receptor | Asociado del host especificado en **Configuración de recepción** o asociado invitado especificado en **Configuración de envío** para un acuerdo de EDIFACT |
| Aplicación lógica | Aplicación lógica donde se configuran las acciones EDIFACT |
| Status | Estado del mensaje EDIFACT <br>Correcto = recibido o enviado un mensaje EDIFACT válido. No se configura ninguna confirmación funcional. <br>Correcto = recibido o enviado un mensaje EDIFACT válido. Se configura y se recibe una confirmación funcional, o se envía una confirmación funcional. <br>Error = recibido o enviado un mensaje EDIFACT no válido <br>Pendiente = recibido o enviado un mensaje EDIFACT válido. Se configura una confirmación funcional y se espera una confirmación funcional. |
| Ack | Estado de confirmación funcional (CONTRL) <br>Aceptado = recibida o enviada una confirmación funcional positiva. <br>Rechazado = recibida o enviada una confirmación funcional negativa. <br>Pendiente = se espera una confirmación funcional, pero no se ha recibido. <br>Pendiente = generada una confirmación funcional, pero no se puede enviar al asociado. <br>No necesario = confirmación funcional no configurada. |
| Dirección | Dirección del mensaje EDIFACT |
| Id. de correlación | Identificador que correlaciona todos los desencadenadores y las acciones de una aplicación lógica |
| Tipo de mensaje | Tipo del mensaje EDIFACT |
| ICN | Número de control de intercambio del mensaje EDIFACT |
| TSCN | Número de control de conjuntos de transacciones del mensaje EDIFACT |
| Timestamp | Hora a la que la acción EDIFACT procesó el mensaje |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>Formatos de nombre EDIFACT para archivos de mensajes descargados

Estos son los formatos de nombre de cada carpeta y archivo de mensajes EDIFACT descargados.

| Archivo o carpeta | Formato de nombre |
| :------------- | :---------- |
| Carpeta de mensajes | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Archivos de entrada, salida y, si se ha configurado, de confirmación | **Carga de entrada**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_input_payload.txt </p>**Carga de salida**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_output\_payload.txt </p></p>**Entradas**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_inputs.txt </p></p>**Salidas**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Pasos siguientes

* [Consultar mensajes B2B en los registros de Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Esquemas de seguimiento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de seguimiento de X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de seguimiento personalizados](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)
