---
title: Reacción ante eventos de Azure Blob Storage | Microsoft Docs
description: Utilice Azure Event Grid para suscribirse a los eventos de Blob Storage.
author: normesta
ms.author: normesta
ms.date: 01/30/2018
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: cbrooks
ms.openlocfilehash: 78ec5b6d330f03d78dcb4e798b23d588fd93398e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76835970"
---
# <a name="reacting-to-blob-storage-events"></a>Reacción ante eventos de Blob Storage

Los eventos de Azure Storage permiten a las aplicaciones reaccionar a eventos, como la creación y la eliminación de blobs. Esto se consigue sin necesidad de código complejo ni de servicios de sondeo costosos e ineficientes.

Los eventos se envían mediante [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) a los suscriptores, como con Azure Functions, Azure Logic Apps o incluso su propio agente de escucha http. Lo mejor de todo es que solo paga por lo que usa.

Blob Storage envía los eventos a Event Grid, que proporciona servicios de entrega confiables para sus aplicaciones mediante directivas de reintento enriquecidas y colas de mensajes fallidos.

Los escenarios habituales de los eventos de Blob Storage incluyen el procesamiento de imágenes o de vídeo, la indexación de búsqueda o cualquier flujo de trabajo orientado a archivos. Cargas de archivos asincrónicas son una excelente elección para los eventos. Cuando se realizan pocos cambios en el escenario, pero se requiere una respuesta inmediata, la arquitectura basada en eventos puede ser especialmente eficaz.

Si desea probarlo ahora, consulte cualquiera de estos artículos de inicio rápido:

|Si desea utilizar esta herramienta:    |Consulte este artículo: |
|--|-|
|Portal de Azure    |[Inicio rápido: Enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con Azure Portal](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[Inicio rápido: Enrutamiento de eventos de almacenamiento a un punto de conexión web con PowerShell](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure CLI    |[Inicio rápido: Enrutamiento de eventos de almacenamiento a un punto de conexión web con la CLI de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

Si la cuenta tiene un espacio de nombres jerárquico, este tutorial le mostrará cómo conectar una suscripción de Event Grid, una función de Azure y un [trabajo](https://docs.azuredatabricks.net/user-guide/jobs.html) en Azure Databricks: [Tutorial: Uso de eventos de Azure Data Lake Storage Gen2 para actualizar una tabla de Databricks Delta](data-lake-storage-events.md).

>[!NOTE]
> Solo las cuentas de almacenamiento de tipo **StorageV2 (uso general v2)** y **BlobStorage** admiten la integración de eventos. **Storage (uso general v1)** *no* admite la integración con Event Grid.

## <a name="the-event-model"></a>Modelo de evento

Event Grid usa las [suscripciones a eventos](../../event-grid/concepts.md#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Esta imagen ilustra la relación entre los publicadores de eventos, las suscripciones a eventos y los controladores de eventos.

![Modelo de Event Grid](./media/storage-blob-event-overview/event-grid-functional-model.png)

Primero, suscriba un punto de conexión a un evento. A continuación, cuando se desencadene un evento, el servicio Event Grid enviará datos sobre ese evento al punto de conexión.

Consulte el artículo del [Esquema de eventos para Blob Storage](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver:

> [!div class="checklist"]
> * Una lista completa de los eventos de Blob Storage y cómo se desencadena cada evento.
> * Un ejemplo de los datos que enviaría Event Grid para cada uno de estos eventos.
> * El propósito de cada par clave-valor que aparece en los datos.

## <a name="filtering-events"></a>Filtrado de eventos

Los [eventos de blob se pueden filtrar](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) por el tipo de evento, el nombre del contenedor o el nombre del objeto que se creó o eliminó. Los filtros de Event Grid coinciden con el principio o el final del asunto para que los eventos con un asunto coincidente se envíen al suscriptor.

Para más información sobre cómo aplicar filtros, consulte el artículo de [Filtrado de eventos para Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

El asunto de los eventos de Blob Storage utiliza el formato:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Para que coincida con todos los eventos de una cuenta de almacenamiento, los filtros de asunto se pueden dejar vacíos.

Para que coincida con eventos de blobs creados en un conjunto de contenedores que comparten un prefijo, utilice un filtro `subjectBeginsWith` como:

```
/blobServices/default/containers/containerprefix
```

Para que coincida con eventos de blobs creados en un contenedor concreto, utilice un filtro `subjectBeginsWith` como:

```
/blobServices/default/containers/containername/
```

Para que coincida con eventos de blobs creados en un contenedor concreto que comparten un prefijo de nombre de blob, utilice un filtro `subjectBeginsWith` como:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

Para que coincida con eventos de blobs creados en un contenedor concreto que comparten un sufijo de blob, utilice un filtro `subjectEndsWith` como ".log" o ".jpg". Para más información, vea [Conceptos de Event Grid](../../event-grid/concepts.md#event-subscriptions).

## <a name="practices-for-consuming-events"></a>Prácticas para consumir eventos

Las aplicaciones que controlan los eventos de Blob Storage deben seguir algunas prácticas recomendadas:
> [!div class="checklist"]
> * Dado que se pueden configurar varias suscripciones para enrutar eventos al mismo controlador de eventos, es importante no asumir que los eventos provienen de un origen determinado, sino comprobar el tema del mensaje para asegurarse de que proviene de la cuenta de almacenamiento esperable.
> * De igual forma, compruebe que eventType es uno de los que está preparado para procesar y no asuma que todos los eventos que reciba van a ser los tipos que espera.
> * Dado que los mensajes pueden llegar desordenados y con cierto retraso, utilice los campos de etag para saber si la información acerca de los objetos sigue estando actualizada.  Además, utilice los campos del secuenciador para conocer el orden de los eventos en cualquier objeto determinado.
> * Utilice el campo blobType para saber qué tipo de operaciones se permiten en el blob y qué tipos de bibliotecas de cliente se deben usar para acceder al blob. Los valores válidos son `BlockBlob` o `PageBlob`. 
> * Utilice el campo de dirección URL con los constructores `CloudBlockBlob` y `CloudAppendBlob` para acceder al blob.
> * Ignore los campos que no comprenda. Esta práctica le ayudará a mantenerse resistente a las nuevas características que puedan agregarse en el futuro.
> * Si desea asegurarse de que el evento **Microsoft.Storage.BlobCreated** se desencadena únicamente cuando un blob en bloques está completamente confirmado, filtre el evento para las llamadas de API REST `CopyBlob`, `PutBlob`, `PutBlockList` o `FlushWithClose`. Estas llamadas API desencadenan el evento **Microsoft.Storage.BlobCreated** únicamente después de que los datos se hayan confirmado en un blob en bloques. Para información sobre cómo crear un filtro, consulte [Filtrado de eventos para Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de Event Grid y pruebe los eventos de Blob Storage:

- [Una introducción a Azure Event Grid](../../event-grid/overview.md)
- [Enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado (versión preliminar)](storage-blob-event-quickstart.md)
