---
title: Introducción a las solicitudes HTTP de Conexiones híbridas de Azure Relay en .NET | Microsoft Docs
description: Escriba una aplicación de consola en C# para las solicitudes HTTP de Conexiones híbridas de Azure Relay en .NET.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/16/2018
ms.author: spelluru
ms.openlocfilehash: 55e97ff95245ce222ccbc2a99f6ae2882cef3715
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248782"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Introducción a las solicitudes HTTP de Conexiones híbridas de Relay en .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Este tutorial proporciona una introducción a [Conexiones híbridas de Azure Relay](relay-what-is-it.md#hybrid-connections). Obtenga información acerca de cómo usar Microsoft .NET para crear una aplicación cliente que envíe solicitudes a una aplicación de escucha correspondiente. 

## <a name="what-will-be-accomplished"></a>Lo que se logrará
Las conexiones híbridas requieren un componente de cliente y un componente de servidor. En este tutorial, se siguen estos pasos para crear dos aplicaciones de consola:

1. Creación de un espacio de nombres de Relay mediante Azure Portal.
2. Creación de una conexión híbrida en dicho espacio de nombres mediante Azure Portal.
3. Escritura de una aplicación de consola de servidor (de escucha) para recibir solicitudes.
4. Escritura de una aplicación de consola de cliente (remitente) para enviar solicitudes.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* [Visual Studio 2015 o posterior](https://www.visualstudio.com). En los ejemplos de este tutorial se usa Visual Studio 2017.
* Una suscripción de Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-by-using-the-azure-portal"></a>1. Creación de un espacio de nombres mediante Azure Portal
Si ya ha creado un espacio de nombres de Relay, vaya a la sección [Creación de una conexión híbrida mediante Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-by-using-the-azure-portal"></a>2. Creación de una conexión híbrida mediante Azure Portal
Si ya ha creado una conexión híbrida, vaya a la sección [Creación de una aplicación de servidor](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Creación de una aplicación de servidor (agente de escucha)
En Visual Studio, escriba una aplicación de consola en C# para escuchar y recibir mensajes de Relay.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Creación de una aplicación de cliente (remitente)
En Visual Studio, escriba una aplicación de consola en C# para enviar mensajes a Relay.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Ejecución de las aplicaciones
1. Ejecute la aplicación de servidor. Verá el texto siguiente en la ventana de la consola:

    ```
    Online
    Server listening
    ```
1. Ejecute la aplicación cliente. Verá `hello!` en la ventana cliente. El cliente envió una solicitud HTTP al servidor y este respondió con `hello!`. 
3. Ahora, para cerrar las ventanas de la consola, presione **ENTRAR** en ambas ventanas de la consola. 

Enhorabuena, ha creado una aplicación de conexiones híbridas de extremo a extremo.

## <a name="next-steps"></a>Pasos siguientes

* [Preguntas más frecuentes acerca de Relay](relay-faq.md)
* [Creación de un espacio de nombres](relay-create-namespace-portal.md)
* [Introducción a Node](relay-hybrid-connections-node-get-started.md)

