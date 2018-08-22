---
title: Envío de eventos a Azure Event Hubs mediante Go | Microsoft Docs
description: Introducción al envío de eventos a Event Hubs mediante Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: 40b3aa82c3e9e8ab9a30362c0a41998877655725
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005604"
---
# <a name="send-events-to-event-hubs-using-go"></a>Envío de eventos a Event Hubs mediante Go

Azure Event Hubs es un sistema de administración de eventos de alta escalabilidad que puede controlar millones de eventos por segundo, habilitando aplicaciones para procesar y analizar las grandes cantidades de datos generados por los dispositivos conectados y por otros sistemas. Una vez recopilados en un centro de eventos, puede recibir y controlar los eventos mediante controladores en proceso o mediante el reenvío a otros sistemas de análisis.

Para más información sobre Event Hubs, consulte [Información general de Event Hubs][Event Hubs overview].

Este tutorial describe cómo enviar eventos a un centro de eventos desde una aplicación escrita en Go. Para recibir eventos, use el paquete **Go eph** (host del procesador de eventos) como se describe en [el artículo de recepción correspondiente](event-hubs-go-get-started-receive-eph.md).

El código de este tutorial se ha tomado de [estos ejemplos de GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), que puede examinar para ver toda la aplicación en funcionamiento incluidas las instrucciones de importación y las declaraciones de variables.

Hay otros ejemplos disponibles [en el repositorio de paquetes de Event Hubs](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Go instalado de forma local. Siga [estas instrucciones](https://golang.org/doc/install) si es necesario.
* Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita][] antes de empezar.
* Un centro de eventos y un espacio de nombres de Event Hubs existentes. Puede crear estas entidades siguiendo las instrucciones de [este artículo](event-hubs-create.md).

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

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Event Hubs, visite las siguientes páginas:

* [Recepción de eventos mediante EventProcessorHost](event-hubs-go-get-started-receive-eph.md)
* [Información general de Event Hubs][Event Hubs overview]
* [Creación de un centro de eventos](event-hubs-create.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
