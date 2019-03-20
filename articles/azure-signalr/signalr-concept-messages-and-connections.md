---
title: Mensajes y conexiones de Azure SignalR Service
description: Introducción a los conceptos claves de mensajes y conexiones de Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: e82ce8f5c97aed7e2cb832d8e808ff84691f7c9e
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556777"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Mensajes y conexiones de Azure SignalR Service

Azure SignalR Service tiene un modelo de facturación basado en el número de conexiones y el número de mensajes. Este artículo explica cómo se definen y cuentan los mensajes y las conexiones para la facturación.


## <a name="message-formats"></a>Formatos de mensajes 

Azure SignalR Service admite los mismos formatos que admite ASP.NET Core SignalR: [JSON](https://www.json.org/) y [MessagePack](/aspnet/core/signalr/messagepackhubprotocol).

## <a name="message-size"></a>Tamaño del mensaje

Azure SignalR Service no tiene límite de tamaño para los mensajes.

Los mensajes grandes se dividen en mensajes más pequeños de no más de 2 KB cada uno que se transmiten por separado. Los SDK controlan la división y el ensamblado de los mensajes. El desarrollador no debe hacer nada.

Los mensajes grandes afectan negativamente al rendimiento de la mensajería. Use mensajes de menor tamaño siempre que sea posible y pruebe para determinar el tamaño de mensaje óptimo para cada escenario de caso de uso.

## <a name="how-messages-are-counted-for-billing"></a>Cómo se cuentan los mensajes para la facturación

Para la facturación, solo se cuentan mensajes salientes de Azure SignalR Service. Los mensajes de ping entre clientes y servidores se omiten.

Los mensajes mayores de 2 KB se cuentan como mensajes múltiples de 2 KB cada uno. El gráfico de recuento de mensajes de Azure Portal se actualiza cada 100 mensajes por centro.

Por ejemplo, suponga que tiene tres clientes y un servidor de aplicaciones. Un cliente envía un mensaje de 4 KB para permitir que el servidor difunda a todos los clientes. El número de mensajes es ocho: un mensaje desde el servicio al servidor de aplicaciones y tres mensajes desde el servicio a los clientes. Cada mensaje se cuenta como dos mensajes de 2 KB.

El recuento de mensajes que se muestra en Azure Portal seguirá siendo 0 hasta que se acumule para sumar más de 100.

## <a name="how-connections-are-counted"></a>Cómo se cuentan las conexiones

Existen conexiones de servidores y conexiones de cliente. De manera predeterminada, cada servidor de aplicaciones tiene cinco conexiones por centro con Azure SignalR Service y cada cliente tiene una conexión de cliente con Azure SignalR Service.

El recuento de conexiones que se muestra en Azure Portal incluye conexiones de servidor y conexiones de cliente.

Por ejemplo, suponga que tiene dos servidores de aplicaciones y define cinco centros en el código. El número de conexiones del servidor será 50: 2 servidores de aplicaciones * 5 centros * 5 conexiones por centro.

ASP.NET SignalR calcula las conexiones de los servidores de otra forma. Incluye un centro predeterminado además de los centros que defina. De forma predeterminada, cada servidor de aplicaciones necesita cinco conexiones de servidores más. El número de conexiones del centro predeterminado se mantiene igual que el de los restantes centros.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Cómo se cuenta el tráfico entrante y saliente

La distinción entre el tráfico entrante y el tráfico saliente se basa en la perspectiva de Azure SignalR Service. El tráfico se calcula en bytes. Al igual que el número de mensajes, el tráfico también tiene una frecuencia de muestreo. El gráfico de entrada y salida de Azure Portal se actualiza cada 100 KB por centro.

## <a name="related-resources"></a>Recursos relacionados

- [Tipos de agregación en Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [Configuración de ASP.NET Core SignalR](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)