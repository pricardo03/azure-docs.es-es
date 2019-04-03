---
title: Supervisión de métricas y registros en el servicio de puerta de entrada de Azure | Microsoft Docs
description: En este artículo se describe los diferentes métricas y registros de acceso que admite el servicio de puerta de entrada de Azure
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
ms.openlocfilehash: 98aabf5330589bf80f1653bb2882c015a4bc133c
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862121"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door-service"></a>Supervisión de métricas y registros en el servicio de puerta de entrada de Azure

Mediante el servicio de puerta de entrada de Azure, puede supervisar los recursos de las maneras siguientes:

- **Métricas**. Application Gateway actualmente tiene siete métricas para ver los contadores de rendimiento.
- **Los registros**. Registros de diagnóstico y actividad permiten rendimiento, acceso y otros datos que se guarden o consuman desde un recurso con fines de supervisión.

### <a name="metrics"></a>Métricas

Las métricas son una característica de determinados recursos de Azure que le permiten ver los contadores de rendimiento en el portal. Estas son las métricas de puerta de entrada disponibles:

| Métrica | Nombre de métrica para mostrar | Unidad | Dimensiones | DESCRIPCIÓN |
| --- | --- | --- | --- | --- |
| RequestCount | Recuento de solicitudes | Número | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de solicitudes de cliente que atiende Front Door.  |
| RequestSize | Tamaño de la solicitud | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como solicitudes de clientes a Front Door. |
| ResponseSize | Tamaño de la respuesta | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como respuestas de Front Door a los clientes. |
| TotalLatency | Latencia total | Milisegundos | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | El tiempo se calcula de la solicitud de cliente recibida por la puerta delantera hasta que el cliente reconoce el último byte de respuesta de la puerta de entrada. |
| BackendRequestCount | Recuento de solicitudes de back-end | Número | HttpStatus</br>HttpStatusGroup</br>Back-end | Número de solicitudes enviadas de Front Door a los servidores back-end. |
| BackendRequestLatency | Latencia de las solicitudes de back-end | Milisegundos | Back-end | Tiempo calculado desde que Front Door envía la solicitud al servidor back-end hasta que Front Door recibe el último byte de respuesta del servidor back-end. |
| BackendHealthPercentage | Porcentaje de estado del back-end | Percent | Back-end</br>BackendPool | El porcentaje de sondeos de estado correctos de Front Door a los servidores back-ends. |
| WebApplicationFirewallRequestCount | Recuento de solicitudes del firewall de aplicaciones web | Número | PolicyName</br>RuleName</br>. | Número de solicitudes de cliente procesadas por la seguridad del nivel de aplicación de Front Door. |

## <a name="activity-log"></a>Registros de actividad

Los registros de actividad proporcionan información sobre las operaciones realizadas en el servicio de puerta de entrada. También determinan qué, quién y cuándo para cualquier escritura (put, post o delete) realizadas en el servicio de puerta de entrada.

>[!NOTE]
>Los registros de actividad no incluyen las operaciones de lectura (get). También no incluyen las operaciones que realizan mediante el portal de Azure o la API de administración original.

Registros de actividad de acceso en el servicio de puerta de entrada o de todos los registros de los recursos de Azure en Azure Monitor. Para ver los registros de actividad:

1. Seleccione la instancia de puerta de entrada.
2. Seleccione **registro de actividad**.

    ![Registro de actividades](./media/front-door-diagnostics/activity-log.png)

3. Elija un ámbito de filtrado y, a continuación, seleccione **aplicar**.

## <a name="diagnostic-logging"></a>Registros de diagnóstico
Los registros de diagnóstico proporcionan información valiosa sobre las operaciones y errores que son importantes para la auditoría y solución de problemas. Los registros de diagnóstico difieren de los registros de actividad.

Los registros de actividad proporcionan información sobre las operaciones realizadas en recursos de Azure. Los registros de diagnóstico proporcionan información sobre las operaciones que realiza el recurso. Para obtener más información, consulte [los registros de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md).

![Registros de diagnóstico](./media/front-door-diagnostics/diagnostic-log.png)

Para configurar los registros de diagnóstico para el servicio de puerta de entrada:

1. Seleccione el servicio Azure API Management.

2. Elija **configuración de diagnóstico**.

3. Seleccione **Activar diagnósticos**. Archivar registros de diagnóstico, junto con las métricas para una cuenta de almacenamiento, transmitirlos a event hubs o enviarlos a los registros de Azure Monitor.

Servicio de puerta de entrada actualmente proporciona los registros de diagnóstico (cada hora por lotes). Registros de diagnóstico proporcionan solicitudes API individuales con cada entrada que tenga el siguiente esquema:

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

- [Creación de un perfil de Front Door](quickstart-create-front-door.md)
- [Cómo funciona la puerta de entrada](front-door-routing-architecture.md)
