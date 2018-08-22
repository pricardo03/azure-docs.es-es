---
title: Recepción de eventos de Azure Event Hubs mediante Go | Microsoft Docs
description: Introducción a la recepción de eventos de Event Hubs mediante Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: eaea6adbaef7baf9bb1e617ba0a709cf14edf781
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005585"
---
# <a name="receive-events-from-event-hubs-using-go"></a>Recepción de eventos de Event Hubs mediante Go

Azure Event Hubs es un sistema de administración de eventos de alta escalabilidad que puede controlar millones de eventos por segundo, habilitando aplicaciones para procesar y analizar las grandes cantidades de datos generados por los dispositivos conectados y por otros sistemas. Una vez recopilados en un centro de eventos, puede recibir y controlar los eventos mediante controladores en proceso o mediante el reenvío a otros sistemas de análisis.

Para más información sobre Event Hubs, consulte [Información general de Event Hubs][Event Hubs overview].

Este tutorial describe cómo recibir eventos desde un centro de eventos en una aplicación Go. Para más información sobre cómo enviar eventos, consulte [el artículo de envío correspondiente](event-hubs-go-get-started-send.md).

El código de este tutorial se ha tomado de [estos ejemplos de GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), que puede examinar para ver toda la aplicación en funcionamiento incluidas las instrucciones de importación y las declaraciones de variables.

Hay otros ejemplos disponibles [en el repositorio de paquetes de Event Hubs](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Go instalado de forma local. Siga [estas instrucciones](https://golang.org/doc/install) si es necesario.
* Una cuenta de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita][] antes de empezar.
* Para recibir mensajes, debe haber mensajes en el centro de eventos de destino. Aprenda a enviar mensajes en el [tutorial de envío](event-hubs-go-get-started-send.md).
* Un centro de eventos existente (consulte la sección siguiente).
* Una cuenta de almacenamiento y un contenedor existentes (consulte la sección posterior a la sección siguiente).

### <a name="create-an-event-hub"></a>Crear un centro de eventos

Este tutorial comienza con un centro de eventos y un espacio de nombres de Event Hubs existentes. Puede crear estas entidades siguiendo las instrucciones de [este artículo](event-hubs-create.md).

### <a name="create-a-storage-account-and-container"></a>Creación de una cuenta de almacenamiento y un contenedor

Los estados como, por ejemplo, las concesiones sobre particiones y puntos de comprobación del flujo de eventos se comparten entre receptores mediante un contenedor de Azure Storage. Puede crear una cuenta de almacenamiento y un contenedor con el SDK para Go, pero también puede crearlos siguiendo las instrucciones de [Acerca de las cuentas de almacenamiento de Azure](../storage/common/storage-create-storage-account.md).

Hay ejemplos disponibles para la creación de artefactos de Storage con el SDK para Go en el [repositorio de ejemplos de Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) y en el ejemplo correspondiente de este tutorial.

## <a name="receive-messages"></a>Recepción de mensajes

Para recibir los mensajes, obtenga los paquetes de Go para Event Hubs con `go get` o `dep`:

```bash
go get -u github.com/Azure/azure-event-hubs-go/...
go get -u github.com/Azure/azure-amqp-common-go/...
go get -u github.com/Azure/go-autorest/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
dep ensure -add github.com/Azure/go-autorest
```

## <a name="import-packages-in-your-code-file"></a>Importación de paquetes en el archivo de código

Para importar paquetes de Go, utilice el siguiente ejemplo de código:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
    eph "github.com/Azure/azure-event-hubs-go/eph"
    storageLeaser "github.com/Azure/azure-event-hubs-go/storage"
    azure "github.com/Azure/go-autorest/autorest/azure"
)
```

## <a name="create-service-principal"></a>Creación de una entidad de servicio

Cree una nueva entidad de servicio siguiendo las instrucciones que se indican en [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli). Guarde las credenciales proporcionadas en su entorno con los siguientes nombres. Los paquetes de Azure SDK para Go y de Event Hubs se han configurado previamente para buscar estos nombres de variables.

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

A continuación, cree un proveedor de autorización para el cliente de Event Hubs que usa estas credenciales:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="get-metadata-struct"></a>Obtención de la estructura de metadatos

Obtenga una estructura con metadatos sobre el entorno de Azure mediante Azure SDK para Go. Las operaciones posteriores usarán esta estructura para buscar los puntos de conexión correctos.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

## <a name="create-credential-helper"></a>Creación de un asistente de credenciales 

Cree un asistente de credenciales que use las credenciales anteriores de Azure Active Directory (AAD) para crear una credencial de firma de acceso compartido (SAS) para Storage. El último parámetro le indica a este constructor que debe usar las mismas variables de entorno que se usaron anteriormente:

```go
cred, err := storageLeaser.NewAADSASCredential(
    subscriptionID,
    resourceGroupName,
    storageAccountName,
    storageContainerName,
    storageLeaser.AADSASCredentialWithEnvironmentVars())
if err != nil {
    log.Fatalf("could not prepare a storage credential: %s\n", err)
}
```

## <a name="create-checkpointer-and-leaser"></a>Creación de un generador de puntos de comprobación y de concesiones 

Cree un **generador de concesiones**, que se encargue de conceder una partición a un receptor determinado, y un **generador de puntos de comprobación**, encargado de escribir puntos de comprobación para el flujo de mensajes para que otros receptores puedan empezar la lectura desde la posición de desplazamiento correcta.

Actualmente, solo hay una instancia de **StorageLeaserCheckpointer** disponible que usa el mismo contenedor de Storage para administrar las concesiones y los puntos de comprobación. Además de la cuenta de almacenamiento y de los nombres de contenedor, **StorageLeaserCheckpointer** necesita las credenciales que se crearon en el paso anterior y la estructura del entorno de Azure para acceder correctamente al contenedor.

```go
leaserCheckpointer, err := storageLeaser.NewStorageLeaserCheckpointer(
    cred,
    storageAccountName,
    storageContainerName,
    azureEnv)
if err != nil {
    log.Fatalf("could not prepare a storage leaserCheckpointer: %s\n", err)
}
```

## <a name="construct-event-processor-host"></a>Construcción del host del procesador de eventos

Ahora tiene los elementos necesarios para construir un host EventProcessorHost, de la forma que se indica a continuación. Se usa la misma instancia de **StorageLeaserCheckpointer** para el generador de concesiones y el de puntos de comprobación, como se ha descrito anteriormente:

```go
ctx := context.Background()
p, err := eph.New(
    ctx,
    nsName,
    hubName,
    tokenProvider,
    leaserCheckpointer,
    leaserCheckpointer)
if err != nil {
    log.Fatalf("failed to create EPH: %s\n", err)
}
defer p.Close(context.Background())
```

## <a name="create-handler"></a>Creación de un controlador 

Cree un controlador y regístrelo con el host del procesador de eventos. Cuando se inicie el host, se aplicará este y todos los demás controladores especificados a los mensajes entrantes:

```go
handler := func(ctx context.Context, event *eventhubs.Event) error {
    fmt.Printf("received: %s\n", string(event.Data))
    return nil
}

// register the handler with the EPH
_, err := p.RegisterHandler(ctx, handler)
if err != nil {
    log.Fatalf("failed to register handler: %s\n", err)
}
```

## <a name="receive-messages"></a>Recepción de mensajes

Una vez configurado todo, puede iniciar el host del procesador de eventos con `Start(context)` para mantenerlo constantemente en ejecución, o con `StartNonBlocking(context)` para que se ejecute solo si los mensajes están disponibles.

Este tutorial se inicia y se ejecuta de la siguiente manera. Consulte el ejemplo mediante `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="notes"></a>Notas

Este tutorial usa una sola instancia de **EventProcessorHost**. Para aumentar el rendimiento y la confiabilidad, debe ejecutar varias instancias de **EventProcessorHost** en sistemas diferentes. El sistema de generación de concesiones garantiza que solo se asocie un receptor a una partición específica en un momento concreto y que reciba mensajes de esta.

## <a name="next-steps"></a>Pasos siguientes

Visite estas páginas para más información acerca de Event Hubs:

* [Envío de eventos con Go](event-hubs-go-get-started-send.md)
* [Información general de Event Hubs](event-hubs-about.md)
* [Creación de un centro de eventos](event-hubs-create.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
