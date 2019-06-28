---
title: Supervisión de métricas y registro en Azure Front Door Service| Microsoft Docs
description: Este artículo describe las diferentes métricas y registros de acceso que admite Azure Front Door Service.
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
ms.openlocfilehash: 16770ea0a320b3d9f081cc21a102ab050a6467f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736808"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door-service"></a>Supervisión de métricas y registro en Azure Front Door Service

Con Azure Front Door Service, puede supervisar los recursos de las siguientes maneras:

- **Métricas**. Application Gateway actualmente tiene siete métricas para ver los contadores de rendimiento.
- **Registros**. Los registros de actividad y diagnóstico permiten que un recurso guarde o consuma datos de rendimiento, acceso u otros con fines de supervisión.

### <a name="metrics"></a>Métricas

Las métricas son una característica de determinados recursos de Azure en los que puede ver contadores de rendimiento en el portal. Las métricas siguientes están disponibles en Front Door:

| Métrica | Nombre de métrica para mostrar | Unidad | Dimensiones | DESCRIPCIÓN |
| --- | --- | --- | --- | --- |
| RequestCount | Recuento de solicitudes | Recuento | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de solicitudes de cliente que atiende Front Door.  |
| RequestSize | Tamaño de la solicitud | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como solicitudes de clientes a Front Door. |
| ResponseSize | Tamaño de la respuesta | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como respuestas de Front Door a los clientes. |
| TotalLatency | Latencia total | Milisegundos | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Tiempo calculado desde que Front Door recibe la solicitud de cliente hasta que el cliente reconoce el último byte de respuesta de Front Door. |
| BackendRequestCount | Recuento de solicitudes de back-end | Recuento | HttpStatus</br>HttpStatusGroup</br>Back-end | Número de solicitudes enviadas de Front Door a los servidores back-end. |
| BackendRequestLatency | Latencia de las solicitudes de back-end | Milisegundos | Back-end | Tiempo calculado desde que Front Door envía la solicitud al servidor back-end hasta que Front Door recibe el último byte de respuesta del servidor back-end. |
| BackendHealthPercentage | Porcentaje de estado del back-end | Percent | Back-end</br>BackendPool | El porcentaje de sondeos de estado correctos de Front Door a los servidores back-ends. |
| WebApplicationFirewallRequestCount | Recuento de solicitudes del firewall de aplicaciones web | Recuento | PolicyName</br>RuleName</br>. | Número de solicitudes de cliente procesadas por la seguridad del nivel de aplicación de Front Door. |

## <a name="activity-log"></a>Registros de actividad

Los registros de actividad proporcionan información sobre las operaciones realizadas en Front Door Service. También determinan qué, quién y cuándo para cualquier operación de escritura (put, post o delete) realizada en Front Door Service.

>[!NOTE]
>Los registros de actividad no incluyen operaciones de lectura (get). Tampoco incluyen operaciones realizadas con Azure Portal o la Management API original.

Acceda a registros de actividad en el servicio Front Door Service o a los registros de todos los recursos de Azure en Azure Monitor. Para ver los registros de actividad:

1. Seleccione la instancia de Front Door.
2. Seleccione **Registro de actividad**.

    ![Registro de actividades](./media/front-door-diagnostics/activity-log.png)

3. Elija un ámbito de filtrado y, a continuación, seleccione **Aplicar**.

## <a name="diagnostic-logging"></a>Registros de diagnóstico
Los registros de diagnóstico proporcionan información valiosa acerca de las operaciones y los errores que son importantes para la auditoría, así como para solucionar problemas. Los registros de diagnóstico son diferentes de los registros de actividad.

Los registros de actividad proporcionan información sobre las operaciones llevadas a cabo en los recursos de Azure. Los registros de diagnóstico proporcionan conclusiones detalladas sobre las operaciones que realiza el recurso. Para más información, vea [Información general sobre los registros de diagnóstico de Azure](../azure-monitor/platform/diagnostic-logs-overview.md).

![Registros de diagnóstico](./media/front-door-diagnostics/diagnostic-log.png)

Para configurar los registros de diagnóstico para Front Door Service:

1. Seleccione su instancia del servicio Azure Front Door Service.

2. Seleccione **Configuración de diagnóstico**.

3. Seleccione **Activar diagnósticos**. Archive los registros de diagnóstico junto con las métricas en una cuenta de almacenamiento, transmítalos en secuencias a un centro de eventos o envíelos a los registros de Azure Monitor.

Front Door Service actualmente proporciona los registros de diagnóstico (agrupados por lotes). Los registros de diagnóstico proporcionan solicitudes API individuales con cada entrada con el siguiente esquema:

| Propiedad  | DESCRIPCIÓN |
| ------------- | ------------- |
| ClientIp | Dirección IP del cliente que realizó la solicitud. |
| ClientPort | Puerto IP del cliente que realizó la solicitud. |
| HttpMethod | Método HTTP utilizado por la solicitud. |
| HttpStatusCode | El código de estado HTTP devuelto desde el servidor proxy. |
| HttpStatusDetails | Estado resultante en la solicitud. El significado de este valor de cadena puede encontrarse en una tabla de referencia de estado. |
| HttpVersion | Tipo de la solicitud o conexión. |
| RequestBytes | El tamaño del mensaje de solicitud HTTP en bytes, incluidos los encabezados de solicitud y el cuerpo de solicitud. |
| RequestUri | URI de la solicitud recibida. |
| ResponseBytes | Bytes enviados por el servidor back-end como respuesta.  |
| RoutingRuleName | El nombre de la regla de enrutamiento que coincidió con la solicitud. |
| SecurityProtocol | La versión del protocolo TLS/SSL utilizada por la solicitud o null si no hay cifrado. |
| TimeTaken | El período de tiempo que tardó la acción, en milisegundos. |
| UserAgent | El tipo de explorador utilizado por el cliente. |
| TrackingReference | La cadena de referencia exclusiva que identifica una solicitud atendida por Front Door, que también se envía como encabezado X-Azure-Ref al cliente. Se requiere para buscar los detalles en los registros de acceso para una solicitud específica. |

## <a name="next-steps"></a>Pasos siguientes

- [Cree una instancia de Front Door](quickstart-create-front-door.md)
- Información acerca de cómo [funciona Front Door](front-door-routing-architecture.md)
