---
title: 'Ejemplos de la CLI de Azure: Azure SignalR Service'
description: 'Ejemplos de la CLI de Azure: Azure SignalR Service'
author: sffamily
ms.service: signalr
ms.topic: sample
ms.date: 04/20/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: a39f0abf4f3a8cc30d6f8f83dcad7e9069f348e4
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258787"
---
# <a name="azure-cli-samples"></a>Ejemplos de la CLI de Azure

En la tabla siguiente se incluyen vínculos a scripts de Bash para Azure SignalR Service creados con la CLI de Azure.

| | |
|-|-|
|**Creación**||
| [Creación de una instancia nueva de SignalR Service y un nuevo grupo de recursos](scripts/signalr-cli-create-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio.  |
|**Integración**||
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR](scripts/signalr-cli-create-with-app-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación web y un nuevo plan de App Service para hospedar una aplicación web ASP.NET Core que usa la instancia de SignalR Service. La aplicación web se configura con una configuración de aplicación para conectar con el nuevo recurso de SignalR Service. |
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR y GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación web de Azure y un nuevo plan de hospedaje para hospedar una aplicación web ASP.NET Core que usa la instancia de SignalR Service. La aplicación web está configurada con los valores de la aplicación para la cadena de conexión al nuevo recurso de SignalR Service y los secretos de cliente para admitir la [autenticación de GitHub](https://developer.github.com/v3/guides/basics-of-authentication/), como se muestra en el [tutorial de autenticación](signalr-authenticate-oauth.md). La aplicación web también está configurada para usar un origen de implementación de repositorio de Git local. |
| | |