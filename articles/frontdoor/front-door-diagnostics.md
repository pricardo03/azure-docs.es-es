---
title: 'Azure Front Door Service: métricas y registro | Microsoft Docs'
description: Este artículo le ayudará a entender las diferentes métricas y registros de acceso que Azure Front Door Service admite.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 5d4d591c465cae91c59e8f86ea9d3e421db0f952
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58112116"
---
# <a name="monitoring-metrics-for-front-door"></a>Supervisión de métricas para Front Door

Mediante el servicio de puerta de entrada de Azure, puede supervisar los recursos de las maneras siguientes:
* [Registros](#diagnostic-logging): los registros permiten que un recurso guarde o consuma datos de rendimiento, acceso o de otro tipo con fines de supervisión.

* [Métricas](#metrics): Application Gateway actualmente tiene siete métricas para ver los contadores de rendimiento.

## <a name="metrics"></a>Métricas

Las métricas son una característica de determinados recursos de Azure en los que puede ver contadores de rendimiento en el portal. Las métricas siguientes están disponibles en Front Door:

| Métrica | Nombre de métrica para mostrar | Unidad | Dimensiones | DESCRIPCIÓN |
| --- | --- | --- | --- | --- |
| RequestCount | Recuento de solicitudes | Número | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de solicitudes de cliente que atiende Front Door.  |
| RequestSize | Tamaño de la solicitud | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como solicitudes de clientes a Front Door. |
| ResponseSize | Tamaño de la respuesta | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como respuestas de Front Door a los clientes. |
| TotalLatency | Latencia total | Milisegundos | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Tiempo calculado desde que Front Door recibe la solicitud de cliente hasta que el cliente reconoce el último byte de respuesta de Front Door. |
| BackendRequestCount | Recuento de solicitudes de back-end | Número | HttpStatus</br>HttpStatusGroup</br>Back-end | Número de solicitudes enviadas de Front Door a los servidores back-end. |
| BackendRequestLatency | Latencia de las solicitudes de back-end | Milisegundos | Back-end | Tiempo calculado desde que Front Door envía la solicitud al servidor back-end hasta que Front Door recibe el último byte de respuesta del servidor back-end. |
| BackendHealthPercentage | Porcentaje de estado del back-end | Percent | Back-end</br>BackendPool | El porcentaje de sondeos de estado correctos de Front Door a los servidores back-ends. |
| WebApplicationFirewallRequestCount | Recuento de solicitudes del firewall de aplicaciones web | Número | PolicyName</br>RuleName</br>. | Número de solicitudes de cliente procesadas por la seguridad del nivel de aplicación de Front Door. |

## <a name="activity-log"></a>Registros de actividad

Los registros de actividad proporcionan información sobre las operaciones que se realizaron en su casa. Con los registros de actividad, se puede determinar el "qué, quién y cuándo" de las operaciones (PUT, POST, DELETE) realizadas en la puerta de entrada de escritura.

> [!NOTE]
> Los registros de actividad no incluyen las operaciones de lectura (GET) ni las realizadas en Azure Portal o mediante las API de administración originales.

Puede obtener acceso a los registros de actividad en su casa o acceder a registros de todos los recursos de Azure en Azure Monitor. 

Para ver los registros de actividad:

1. Seleccione la instancia de puerta de entrada.
2. Haga clic en **Registro de actividad**.

    ![registro de actividad](./media/front-door-diagnostics/activity-log.png)

3. Seleccione el ámbito de filtrado que desee y haga clic en **Aplicar**.

## <a name="diagnostic-logging"></a>Registros de diagnóstico
Los registros de diagnóstico proporcionan información valiosa acerca de las operaciones y los errores que son importantes para la auditoría, así como para solucionar problemas. Los registros de diagnóstico son diferentes de los registros de actividad. Los registros de actividad proporcionan información sobre las operaciones llevadas a cabo en los recursos de Azure. Los registros de diagnóstico proporcionan información detallada acerca de las operaciones que el recurso ha realizado. Obtenga más información sobre [los registros de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md). 

Para configurar los registros de diagnóstico para la puerta de entrada:

1. Seleccione la instancia del servicio APIM.
2. Haga clic en **Configuración de diagnóstico**.

    ![registros de diagnóstico](./media/front-door-diagnostics/diagnostic-log.png)

3. Haga clic en **Activar diagnóstico**. Los registros de diagnóstico se pueden archivar junto con las métricas en una cuenta de almacenamiento, transmitirlos en secuencias a un centro de eventos o enviarlos a los registros de Azure Monitor. 

Actualmente, el servicio de puerta de Azure proporciona diagnósticos solicitar registros (cada hora por lotes) sobre API individual con cada entrada que tenga el siguiente esquema:

| Propiedad  | DESCRIPCIÓN |
| ------------- | ------------- |
| ClientIp | Dirección IP del cliente que realizó la solicitud. |
| ClientPort | El puerto IP del cliente que realizó la solicitud. |
| HttpMethod | Método HTTP utilizado por la solicitud. |
| HttpStatusCode | El código de estado HTTP devuelto desde el servidor proxy. |
| HttpStatusDetails | Estado resultante en la solicitud. Significado de este valor de cadena puede encontrarse en una tabla de referencia de estado. |
| HttpVersion | Tipo de la solicitud o la conexión. |
| RequestBytes | El tamaño del mensaje de solicitud en bytes, incluidos los encabezados de solicitud y el cuerpo de solicitud HTTP. |
| RequestUri | URI de la solicitud recibida. |
| ResponseBytes | Bytes enviados por el servidor back-end como la respuesta.  |
| RoutingRuleName | El nombre de la regla de enrutamiento que coinciden con la solicitud. |
| SecurityProtocol | La versión del protocolo TLS/SSL utilizada por la solicitud o null si no hay cifrado. |
| TimeTaken | El período de tiempo que tardó la acción, en milisegundos. |
| UserAgent | El tipo de explorador que usa el cliente |
| TrackingReference | La cadena de referencia exclusivo que identifica una solicitud atendida por puerta de entrada, también se envía como encabezado X-Azure-Ref al cliente. Se requiere para buscar los detalles en los registros de acceso para una solicitud específica. |

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).
