---
title: Mensajes y conexiones en Azure SignalR
description: Información general sobre los conceptos claves en torno a mensajes y conexiones de Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 5a0430e9ad124319147342c49fc51e11472ac8ff
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53812871"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Mensaje y conexión en Azure SignalR Service

Azure SignalR Service tiene un modelo de facturación basado en el número de conexiones y el número de mensajes. A continuación, se explica cómo se definen y cuentan los mensajes y las conexiones con fines de facturación.

## <a name="message-formats-supported"></a>Formatos de mensajes compatibles

Azure SignalR Service admite los mismos formatos que admite ASP.NET Core SignalR: [JSON](https://www.json.org/) y [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Tamaño del mensaje

Azure SignalR Service no tiene límite en el tamaño de los mensajes.

En la práctica, un mensaje grande se divide en mensajes más pequeños de no más de 2 KB cada uno que se transmiten como mensajes independientes. Los SDK controlan la división y el ensamblado de los mensajes. El desarrollador no debe hacer nada.

Pero un mensaje grande afecta negativamente el rendimiento de la mensajería. Use un mensaje de tamaño más pequeño cada vez que sea posible y pruebe elegir el tamaño de mensaje óptimo para cada escenario de caso de uso.

## <a name="how-to-count-messages-for-billing-purpose"></a>¿Cómo contar los mensajes para fines de facturación?

Solo contamos los mensajes salientes de SignalR Service y omitiremos los mensajes de ping entre clientes y servidores.

Los mensajes mayores de 2 KB se cuentan como mensajes múltiples de 2 KB cada uno. El gráfico de recuento de mensajes de Azure Portal se actualizará cada 100 mensajes por centro.

Por ejemplo, un usuario tiene 3 clientes y 1 servidor de aplicaciones. Un cliente envía un mensaje de 4 KB para permitir que el servidor difunda a todos los clientes. El recuento de mensajes será 8: 1 mensaje del servicio al servidor de aplicaciones, 3 mensajes del servicio a los clientes y cada mensaje se cuenta como 2 mensajes de 2 KB.

El recuento de mensajes que se muestra en Azure Portal sigue siendo 0 hasta que se acumula para sumar más de 100.

## <a name="how-to-count-connections"></a>¿Cómo se cuentan las conexiones?

Existen conexiones de servidores y conexiones de cliente. De manera predeterminada, cada servidor de aplicaciones tiene 5 conexiones por centro con SignalR Service y cada cliente tiene 1 conexión de cliente con SignalR Service.

El recuento de conexiones que se muestra en Azure Portal incluye conexiones de servidor y conexiones de cliente.

Por ejemplo, un usuario tiene dos servidores de aplicaciones y define 5 centros en los códigos. El recuento de conexiones de servidor que se muestra en Azure Portal serán 2 servidores de aplicación * 5 centros * 5 conexiones/centro = 50 conexiones de servidor.

## <a name="related-resources"></a>Recursos relacionados

- [Configuración de ASP.NET Core SignalR](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)