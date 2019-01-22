---
title: Mensajes y conexiones en Azure SignalR
description: Información general sobre los conceptos claves en torno a mensajes y conexiones de Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: c2348df7a1a55584807a03216e294486ddadfc52
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352604"
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

Los mensajes mayores de 2 KB se cuentan como mensajes múltiples de 2 KB cada uno. El gráfico de recuento de mensajes de Azure Portal se actualiza cada 100 mensajes por centro.

Por ejemplo, tiene tres clientes y un servidor de aplicaciones. Un cliente envía un mensaje de 4 KB para permitir que el servidor difunda a todos los clientes. El recuento de mensajes es 8: Un mensaje del servicio al servidor de aplicaciones, tres mensajes del servicio a los clientes y cada mensaje se cuenta como dos mensajes de 2 KB.

El recuento de mensajes que se muestra en Azure Portal sigue siendo 0 hasta que se acumula para sumar más de 100.

## <a name="how-to-count-connections"></a>¿Cómo se cuentan las conexiones?

Existen conexiones de servidores y conexiones de cliente. De manera predeterminada, cada servidor de aplicaciones tiene cinco conexiones por centro con SignalR Service y cada cliente tiene una conexión de cliente con SignalR Service.

El recuento de conexiones que se muestra en Azure Portal incluye conexiones de servidor y conexiones de cliente.

Por ejemplo, tiene dos servidores de aplicaciones y define cinco centros en los códigos. El número de conexiones del servidor es 50: 2 servidores de aplicaciones * 5 centros * 5 conexiones por centro.

ASP.NET SignalR calcula las conexiones de los servidores de otra forma. Tiene un centro predeterminado, además de los centros definidos por el cliente. Cada servidor de aplicaciones necesita cinco conexiones de servidores más de forma predeterminada. El número de conexiones del centro predeterminado se mantiene constante con respecto a los restantes centros.

## <a name="how-to-count-inbound-traffic--outbound-traffic"></a>Recuento del tráfico entrante y saliente

Entrante y saliente es desde la perspectiva de SignalR Service. El tráfico se calcula en bytes. Al igual que el número de mensajes, el tráfico también tiene su frecuencia de muestreo. El gráfico de entrada y salida de Azure Portal se actualiza cada 100 KB por centro.

## <a name="related-resources"></a>Recursos relacionados

- [Tipo de agregación en Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [Configuración de ASP.NET Core SignalR](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)