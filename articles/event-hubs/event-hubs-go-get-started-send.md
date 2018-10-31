---
title: Envío de eventos a Azure Event Hubs mediante Go | Microsoft Docs
description: Introducción al envío de eventos a Event Hubs mediante Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: f5e30a103b09613caee8e9912a89a5bc2d390f65
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458095"
---
# <a name="send-events-to-event-hubs-using-go"></a>Envío de eventos a Event Hubs mediante Go

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

Este tutorial describe cómo enviar eventos a un centro de eventos desde una aplicación escrita en Go. 

> [!NOTE]
> Puede descargar esta guía de inicio rápido como un ejemplo desde [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), reemplazar las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos, y ejecutarlo. También puede seguir los pasos de este tutorial para crear el suyo propio.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Go instalado de forma local. Siga [estas instrucciones](https://golang.org/doc/install) si es necesario.
* Un centro de eventos y un espacio de nombres de Event Hubs existentes. Puede crear estas entidades siguiendo las instrucciones de [este artículo](event-hubs-create.md).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos
El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento de [este artículo](event-hubs-create.md) y después continúe con los pasos siguientes de este tutorial.

## <a name="install-go-package"></a>Instalación del paquete Go

Consiga el paquete Go para Event Hubs con `go get` o `dep`. Por ejemplo: 

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

## <a name="import-packages-in-your-code-file"></a>Importación de paquetes en el archivo de código

Para importar paquetes de Go, utilice el siguiente ejemplo de código:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

## <a name="create-service-principal"></a>Creación de una entidad de servicio

Cree una nueva entidad de servicio siguiendo las instrucciones que se indican en [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli). Guarde las credenciales proporcionadas en su entorno con los siguientes nombres. Los paquetes de Azure SDK para Go y de Event Hubs se han configurado previamente para buscar estos nombres de variables:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Ahora, cree un proveedor de autorización para el cliente de Event Hubs que usa estas credenciales:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="create-event-hubs-client"></a>Creación de cliente de Event Hubs

El siguiente código crea un cliente de Event Hubs:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

## <a name="send-messages"></a>Envío de mensajes

En el siguiente fragmento de código, utilice (1) para enviar mensajes de forma interactiva desde un terminal, o (2) para enviar mensajes desde el programa:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!")
```

## <a name="extras"></a>Extras

Obtenga los identificadores de las particiones del centro de eventos:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n, info.PartitionIDs)
```

Ejecute la aplicación para enviar eventos al centro de eventos. 

Felicidades. Ha enviado mensajes a un centro de eventos.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha enviado mensajes a un centro de eventos mediante Go. Para saber cómo recibir eventos de un centro de eventos mediante Go, consulte [Recepción de eventos desde Azure Event Hubs mediante Go](event-hubs-go-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
