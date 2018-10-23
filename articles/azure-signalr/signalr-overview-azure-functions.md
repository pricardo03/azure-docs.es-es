---
title: Crear aplicaciones en tiempo real con Azure Functions y Azure SignalR | Microsoft Docs
description: Información general acerca de cómo usar el servicio Azure SignalR en aplicaciones sin servidor.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 09/18/2018
ms.author: zhshang
ms.openlocfilehash: 587752b25e3c98bbabbd42bd4d838fd19ae9fd45
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318714"
---
# <a name="build-real-time-apps-with-azure-functions-and-azure-signalr-service"></a>Crear aplicaciones en tiempo real con Azure Functions y el servicio Azure SignalR

Dado que Azure SignalR Service y Azure Functions son servicios altamente escalables y totalmente administrados que le permiten concentrarse en crear aplicaciones en lugar de tener que administrar la infraestructura, es normal usar los dos servicios para proporcionar comunicaciones en tiempo real en un entorno [sin servidores](https://azure.microsoft.com/solutions/serverless/).

## <a name="integrate-real-time-communications-with-azure-services"></a>Integrar las comunicaciones en tiempo real con los servicios de Azure

Azure Functions le permite escribir código en [varios lenguajes](../azure-functions/supported-languages.md), incluyendo JavaScript, C# y Java, y que se activa cada vez que se producen eventos en la nube. Los ejemplos de estos eventos incluyen:

* Solicitudes de HTTP y webhook
* Temporizadores periódicos
* Eventos de los servicios de Azure, como:
    - Event Grid
    - Event Hubs
    - Azure Service Bus
    - Fuente de cambios de Cosmos DB
    - Storage: blobs y colas
    - Conectores de Logic Apps como Salesforce y SQL Server

Al usar Azure Functions para integrar estos eventos con Azure SignalR Service, tiene la capacidad de enviar notificaciones a miles de clientes cada vez que se produce cualquier evento.

Estos son algunos escenarios comunes de la mensajería sin servidor en tiempo real que puede implementar con Azure Functions y el servicio SignalR incluyen:

* Visualizar la telemetría del dispositivo IoT en un panel o mapa en tiempo real.
* Actualizar datos en una aplicación cuando se actualicen documentos en Cosmos DB.
* Enviar notificaciones en aplicación cuando se creen nuevos pedidos en Salesforce.

## <a name="signalr-service-bindings-for-azure-functions"></a>Enlaces de SignalR Service en Azure Functions

Los enlaces de SignalR Service para Azure Functions permiten que una aplicación de Azure Functions publique mensajes en los clientes conectados a SignalR Service. Los clientes pueden conectarse al servicio mediante un SDK de cliente de SignalR que esté disponible en .NET, JavaScript y Java (tendrá más lenguajes disponibles en breve).

### <a name="an-example-scenario"></a>Escenario de ejemplo

Un ejemplo de cómo utilizar los enlaces de SignalR Service, es usar Azure Functions para integrarlo con Azure Cosmos DB y SignalR Service y así enviar mensajes en tiempo real cuando aparezcan nuevos eventos en una fuente de cambios de Cosmos DB.

![Cosmos DB, Azure Functions, SignalR Service](media/signalr-overview-azure-functions/signalr-cosmosdb-functions.png)

1. Se realiza un cambio en una colección de Cosmos DB.
2. El evento de cambio se propaga a la fuente de cambios de Cosmos DB.
3. El evento de cambios desencadena una instancia de Azure Functions mediante el desencadenador de Cosmos DB.
4. El enlace de salida de SignalR Service publica un mensaje en SignalR Service.
5. SignalR Service publica el mensaje para todos los clientes conectados.

### <a name="authentication-and-users"></a>Autenticación y usuarios

SignalR Service le permite difundir mensajes a todos los clientes o a un solo subconjunto de clientes, como los que pertenecen a un único usuario. Los enlaces de SignalR Service de Azure Functions se pueden combinar con la autenticación de App Service, y así poder autenticar a los usuarios que tengan proveedores como Azure Active Directory, Facebook y Twitter. Entonces podrá enviar mensajes directamente a estos usuarios autenticados.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, se obtuvo información general de cómo usar Azure Functions con SignalR Service para habilitar una amplia gama de escenarios de mensajes en tiempo real sin servidor. Siga una de estas guías de inicio rápido para obtener más información.

* [Azure SignalR Service Serverless Quickstart - C#](signalr-quickstart-azure-functions-csharp.md) (Guía de inicio rápido de Azure SignalR Service sin servidor: C#).
* [Azure SignalR Service Serverless Quickstart - JavaScript](signalr-quickstart-azure-functions-javascript.md) (Guía de inicio rápido de Azure SignalR Service sin servidor: JavaScript).

