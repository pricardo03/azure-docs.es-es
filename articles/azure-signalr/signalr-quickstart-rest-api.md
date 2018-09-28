---
title: 'Guía de inicio rápido: API de REST del servicio Azure SignalR | Microsoft Docs'
description: Una guía de inicio rápido para usar la API de REST del servicio Azure SignalR.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: zhshang
ms.openlocfilehash: 93c1198ecfba6db809228ed6dcd99c705f53926c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972766"
---
# <a name="quickstart-broadcast-real-time-messages-from-console-app"></a>Guía de inicio rápido: Difusión de mensajes en tiempo real desde la aplicación de consola

El servicio Azure SignalR proporciona la [API de REST](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) para admitir el servidor para escenarios de comunicación del cliente, como la difusión. Puede elegir cualquier lenguaje de programación que puede llamar a la API de REST. Puede publicar mensajes en todos los clientes conectados, en un cliente específico por el nombre o en un grupo de clientes.

En esta guía de inicio rápido, obtendrá información sobre cómo enviar mensajes desde una aplicación de línea de comandos a las aplicaciones cliente conectadas en C#.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido se puede ejecutar en macOS, Windows o Linux.
* [SDK de .NET Core](https://www.microsoft.com/net/download/core)
* El editor de texto o de código que elija.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en <https://portal.azure.com/> con su cuenta de Azure.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

## <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

Mientras se implementa el servicio, pasaremos a preparar el código. Clone la [aplicación de ejemplo de GitHub](https://github.com/aspnet/AzureSignalR-samples.git), establezca la cadena de conexión del servicio SignalR y ejecute la aplicación de forma local.

1. Abra una ventana de terminal de GIT. Cambie a la carpeta donde quiere clonar el proyecto de ejemplo.

1. Ejecute el comando siguiente para clonar el repositorio de ejemplo. Este comando crea una copia de la aplicación de ejemplo en el equipo.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

Este ejemplo es una aplicación de consola que muestra el uso del servicio Azure SignalR. Proporciona dos modos:

- Modo de servidor: use comandos simples para llamar a la API de REST del servicio Azure SignalR.
- Modo de cliente: conéctese al servicio Azure SignalR y reciba mensajes del servidor.

También puede obtener información acerca de cómo generar un token de acceso para autenticarse con el servicio Azure SignalR.

### <a name="build-the-executable-file"></a>Compilación del archivo ejecutable

Se usa macOS osx.10.13-x64 como ejemplo. Puede encontrar una [referencia](https://docs.microsoft.com/dotnet/core/rid-catalog) sobre cómo compilar en otras plataformas.
```bash
cd AzureSignalR-samples/samples/Serverless/

dotnet publish -c Release -r osx.10.13-x64
```

### <a name="start-a-client"></a>Inicio de un cliente

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="start-a-server"></a>Inicio de un servidor

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless server -c "<ConnectionString>" -h <HubName>
```

## <a name="run-the-sample-without-publishing"></a>Ejecución del ejemplo sin publicar

También puede ejecutar el comando siguiente para iniciar un servidor o cliente.

```bash
# Start a server
dotnet run -- server -c "<ConnectionString>" -h <HubName>

# Start a client
dotnet run -- client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="use-user-secrets-to-specify-connection-string"></a>Uso de los secretos de usuario para especificar la cadena de conexión

Puede ejecutar `dotnet user-secrets set Azure:SignalR:ConnectionString "<ConnectionString>"` en el directorio raíz del ejemplo. Después de eso, ya no necesitará la opción `-c "<ConnectionString>"`.

## <a name="usage"></a>Uso

Una vez iniciado el servidor, use el comando para enviar el mensaje.

```
send user <User Id>
send users <User List>
send group <Group Name>
send groups <Group List>
broadcast
```

Puede iniciar a varios clientes con nombres de cliente diferentes.


[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
