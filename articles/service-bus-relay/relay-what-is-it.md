---
title: ¿Qué es Relay de Azure? | Microsoft Docs
description: En este artículo se proporciona información general sobre el servicio Azure Relay que le permite desarrollar aplicaciones en la nube que consumen servicios locales que se ejecutan en la red corporativa sin necesidad de abrir una conexión de firewall ni realizar cambios molestos en la infraestructura de red.
services: service-bus-relay
author: spelluru
manager: ''
editor: ''
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 10/08/2018
ms.author: spelluru
ms.openlocfilehash: 46a9045cdf422ed4f14e5588b3342e8bfde2e4c8
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888152"
---
# <a name="what-is-azure-relay"></a>¿Qué es Relay de Azure?
El servicio Azure Relay le permite exponer de forma segura servicios que se ejecutan en la red corporativa en la nube pública. Puede hacerlo sin tener que abrir una conexión de firewall y sin realizar cambios molestos en la infraestructura de la red corporativa. 

El servicio Azure Relay admite los siguientes escenarios entre servicios locales y aplicaciones que se ejecutan en la nube o en otro entorno local. 

- Comunicación unidireccional tradicional, de solicitud/respuesta y de punto a punto. 
- Distribución de eventos en el ámbito de Internet para habilitar escenarios de publicación y suscripción 
- Comunicación de socket bidireccional y sin almacenamiento en búfer en los límites de red.

Azure Relay es diferente a las tecnologías de integración en el nivel de red, como VPN. Una instancia de Azure Relay se puede limitar a un único punto de conexión de la aplicación de una única máquina. La tecnología VPN es mucho más intrusiva, ya que se basa en modificar el entorno de red. 

## <a name="basic-flow"></a>Flujo básico
En el patrón de transferencia de datos, los pasos básicos son:

1. Un servicio local se conecta al servicio de retransmisión a través de un puerto de salida. 
2. Este servicio crea un socket bidireccional para la comunicación asociado a una dirección determinada. 
3. Después, el cliente puede comunicarse con el servicio local enviando tráfico al servicio de retransmisión destinado a esa dirección. 
4. El servicio de retransmisión *retransmite* entonces los datos al servicio local a través de un socket bidireccional dedicado al cliente. El cliente no necesita una conexión directa al servicio local. No necesita conocer la ubicación del servicio. Y el servicio local no necesita ningún puerto de entrada abierto en el firewall.


## <a name="features"></a>Características 
Relay de Azure tiene dos características:

- [Conexiones híbridas](#hybrid-connections): usa los sockets web de estándar abierto, con lo que se admiten escenarios multiplataforma.
- [Retransmisiones de WCF](#wcf-relays): usa Windows Communication Foundation (WCF) para habilitar las llamadas a procedimientos remotos. WCF Relay es la oferta de Relay heredada que muchos clientes ya pueden utilizar con sus modelos de programación de WCF.

## <a name="hybrid-connections"></a>conexiones híbridas

La característica Conexiones híbridas de Azure Relay es una evolución segura y de protocolo abierto de las características de Relay que ya existían anteriormente. Puede usarla en cualquier plataforma y en cualquier lenguaje. La característica Conexiones híbridas de Azure Relay se basa en protocolos de HTTP y WebSockets. Le permite enviar solicitudes y recibir respuestas a través de sockets web o HTTP (S). Esta característica es compatible con la API de WebSocket en los exploradores web más habituales. 

Para más información sobre el protocolo de Conexiones híbridas, consulte [Guía del protocolo de conexiones híbridas](relay-hybrid-connections-protocol.md). Puede usar Conexiones híbridas con cualquier biblioteca de sockets web de cualquier entorno de ejecución o lenguaje.

> [!NOTE]
> Conexiones híbridas de Azure Relay reemplaza a la anterior característica Conexiones híbridas de BizTalk Services. La característica Conexiones híbridas de BizTalk Services se basaba en Azure Service Bus WCF Relay. La funcionalidad Conexiones híbridas de Azure Relay complementa la característica WCF Relay que ya existía anteriormente. Estas dos funcionalidades del servicio (WCF Relay y Conexiones híbridas) coexisten en el servicio Azure Relay. Aunque comparten una puerta de enlace común, se trata de implementaciones diferentes.

## <a name="wcf-relay"></a>Retransmisión de WCF
WCF Relay es totalmente compatible con .NET Framework y WCF. Puede crear una conexión entre el servicio local y el servicio de retransmisión mediante un conjunto de enlaces de “retransmisión” WCF. En segundo plano, los enlaces de retransmisión se asignan a nuevos elementos de enlace de transporte diseñados para crear componentes de canal WCF que se integran con Service Bus en la nube. Para más información, consulte [Introducción a WCF Relay](relay-wcf-dotnet-get-started.md).

## <a name="hybrid-connections-vs-wcf-relay"></a>Conexiones híbridas en comparación con Retransmisión de WCF
Conexiones híbridas y WCF Relay habilitan una conexión segura a los recursos de dentro de una red corporativa. El uso de una u otra depende de sus necesidades particulares que se detallan en la siguiente tabla:

|  | Retransmisión de WCF | conexiones híbridas |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **Script de Java/Node.JS** | |x |
| **Protocolo abierto basado en estándares** | |x |
| **Varios modelos de programación de RPC** | |x |

## <a name="architecture-processing-of-incoming-relay-requests"></a>Arquitectura: Procesamiento de solicitudes entrantes de retransmisión
El siguiente diagrama muestra el control de las solicitudes de transmisión entrantes por parte del servicio Azure Relay:

![Procesamiento de solicitudes entrantes de retransmisión WCF](./media/relay-what-is-it/ic690645.png)

1. El cliente de escucha envía una solicitud de escucha al servicio Azure Relay. Azure Load Balancer enruta la solicitud a uno de los nodos de la puerta de enlace. 
2. El servicio Azure Relay crea una retransmisión en el almacén de puerta de enlace. 
3. El cliente de envío envía una solicitud para conectarse al servicio de escucha. 
4. La puerta de enlace que recibe la solicitud busca la transmisión en el almacén de puerta de enlace. 
5. La puerta de enlace reenvía la solicitud de conexión a la puerta de enlace correcta mencionada en el almacén de puerta de enlace. 
6. La puerta de enlace envía una solicitud al cliente de escucha para que cree un canal temporal al nodo de la puerta de enlace que esté más próximo al cliente de envío. 
7. Ahora, el cliente de escucha crea un canal temporal y envía un mensaje de respuesta a la puerta de enlace más cercana al cliente de envío.
8. La puerta de enlace reenvía el mensaje de respuesta al cliente de envío. 

Cuando se establece la conexión de retransmisión, los clientes pueden intercambiar mensajes a través del nodo de puerta de enlace que se usa para el encuentro.

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a los Websockets de .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Introducción a las solicitudes HTTP de .NET](relay-hybrid-connections-http-requests-dotnet-get-started.md)
* [Introducción a los Websockets de Node](relay-hybrid-connections-node-get-started.md)
* [Introducción a las solicitudes HTTP de Node](relay-hybrid-connections-http-requests-node-get-started.md)
* [Preguntas más frecuentes acerca de Relay](relay-faq.md)

