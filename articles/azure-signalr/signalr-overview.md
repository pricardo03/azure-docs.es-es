---
title: Qué es Azure SignalR | Microsoft Docs
description: Introducción a Azure SignalR Service.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: a159833936ec4762213f063e235fa4f9237af95b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951107"
---
# <a name="what-is-azure-signalr-service"></a>Qué es Azure SignalR Service

Azure SignalR Service simplifica la incorporación de funcionalidades web en tiempo real a las aplicaciones a través de HTTP. Esta funcionalidad en tiempo real permite al servicio insertar las actualizaciones de contenido en los clientes conectados, como una única página web o aplicaciones móviles. Como resultado, los clientes se actualizan sin necesidad de sondear el servidor ni de enviar nuevas solicitudes HTTP para las actualizaciones.

En este artículo se proporciona una introducción a Azure SignalR Service.

## <a name="what-is-azure-signalr-service-used-for"></a>¿Para qué se usa Azure SignalR Service? 

Hay muchos tipos de aplicación que requieren actualizaciones de contenido en tiempo real. En los ejemplos se indican varios escenarios adecuados para usar Azure SignalR Service:

* Aplicaciones que requieren actualizaciones desde el servidor con mucha frecuencia. Algunos ejemplos son aplicaciones de juegos, de votación, de subastas y de GPS.
* Paneles y aplicaciones de supervisión. Estos ejemplos también incluyen paneles empresariales y actualizaciones de venta instantáneas.
* Aplicaciones de colaboración. Las aplicaciones de pizarra y el software de reuniones de equipo son ejemplos de aplicaciones de colaboración.
* Aplicaciones que requieren notificaciones. Redes sociales, correo electrónico, chat, juegos, alertas de viaje y muchas otras aplicaciones utilizan notificaciones.

SignalR proporciona una abstracción de una serie de técnicas que se usan para compilar aplicaciones web en tiempo real. [WebSockets](https://wikipedia.org/wiki/WebSocket) es el transporte óptimo, pero otras técnicas, como [Server-Sent Events (SSE)](https://wikipedia.org/wiki/Server-sent_events) y Long Polling, se usan cuando otras opciones no están disponibles. SignalR detecta e inicializa automáticamente el transporte adecuado en función de las características admitidas en el servidor y el cliente.

Además, SignalR proporciona un modelo de programación de aplicaciones en tiempo real que permite que el servidor envíe mensajes a todas las conexiones, o a un subconjunto de conexiones que pertenecen a un usuario específico o que se han colocado en un grupo arbitrario.

## <a name="how-to-use-azure-signalr-service"></a>Cómo usar Azure SignalR Service

Actualmente hay tres formas de usar Azure SignalR Service:

- **[Escalar una aplicación de ASP.NET Core SignalR](signalr-overview-scale-aspnet-core.md)**: integre Azure SignalR Service con una aplicación de ASP.NET Core SignalR para escalar horizontalmente cientos de miles de conexiones.
- **[Compilar aplicaciones en tiempo real sin servidor](signalr-overview-azure-functions.md)** : use la integración de Azure Functions con Azure SignalR Service para crear aplicaciones en tiempo real sin servidor en lenguajes como JavaScript, C# y Java.
- **[Enviar mensajes desde el servidor a clientes a través de la API REST](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)**: Azure SignalR Service proporciona la API REST para permitir que las aplicaciones envíen mensajes a los clientes conectados con SignalR Service, en cualquier lenguaje de programación compatible con REST.

