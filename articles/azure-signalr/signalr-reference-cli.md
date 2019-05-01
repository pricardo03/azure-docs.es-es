---
title: 'Ejemplos de la CLI de Azure: Azure SignalR Service'
description: 'Ejemplos de la CLI de Azure: Azure SignalR Service'
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 1fbe96d827bcf6bb39a6cf7a4e6811174b862d59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61071083"
---
# <a name="azure-cli-reference"></a>Referencia de la CLI de Azure

En la tabla siguiente se incluyen vínculos a scripts de Bash para Azure SignalR Service creados con la CLI de Azure.

| | |
|-|-|
|**Creación**||
| [Creación de una instancia nueva de SignalR Service y un nuevo grupo de recursos](scripts/signalr-cli-create-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio.  |
|**Integración**||
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR](scripts/signalr-cli-create-with-app-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega un nuevo plan de App Service y de aplicación Web para hospedar una aplicación Web de ASP.NET Core que usa el SignalR Service. La aplicación web se configura con una configuración de aplicación para conectar con el nuevo recurso de SignalR Service. |
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR y GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación Web de Azure y plan de hospedaje para el host de una aplicación Web de ASP.NET Core que usa el SignalR Service. La aplicación web está configurada con los valores de la aplicación para la cadena de conexión al nuevo recurso de SignalR Service y los secretos de cliente para admitir la [autenticación de GitHub](https://developer.github.com/v3/guides/basics-of-authentication/), como se muestra en el [tutorial de autenticación](signalr-concept-authenticate-oauth.md). La aplicación web también está configurada para usar un origen de implementación de repositorio de Git local. |
| | |
