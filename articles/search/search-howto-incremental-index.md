---
title: Configuración de la indexación incremental del seguimiento de cambios basado en el contenido enriquecido
titleSuffix: Azure Cognitive Search
description: Habilite el seguimiento de cambios y conserve el estado del contenido enriquecido para el procesamiento controlado en un conjunto de aptitudes cognitivas.
author: vkurpad
manager: eladz
ms.author: vikurpad
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ac082d6ecb6624dc0d5bc0ab927ff8b91ebdabce
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510085"
---
# <a name="how-to-set-up-incremental-indexing-of-enriched-documents-in-azure-cognitive-search"></a>Cómo configurar la indexación incremental de documentos enriquecidos en Azure Cognitive Search

En este artículo se muestra cómo agregar el estado y el almacenamiento en caché a los documentos enriquecidos que se mueven a través de una canalización de enriquecimiento de Azure Cognitive Search para que pueda indexar de forma incremental los documentos desde cualquiera de los orígenes de datos admitidos. De forma predeterminada, un conjunto de aptitudes no tiene estado y el cambio de cualquier parte de su composición requiere una nueva ejecución completa del indexador. Con la indexación incremental, el indizador puede determinar qué partes de la canalización han cambiado, reutilizar el enriquecimiento existente para las partes sin cambios y revisar el enriquecimiento de los pasos con cambios. El contenido almacenado en caché se coloca en Azure Storage.

Si no está familiarizado con la configuración de indexadores, comience con la [información general sobre el indexador](search-indexer-overview.md) y, después, continúe con los [conjuntos de aptitudes](cognitive-search-working-with-skillsets.md) para obtener información sobre las canalizaciones de enriquecimiento. Para obtener más información sobre los conceptos clave, vea el tema sobre la [indexación incremental](cognitive-search-incremental-indexing-conceptual.md).

La indexación incremental se configura mediante la [API de REST api-version=2019-05-06-Preview](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations).

> [!NOTE]
> Esta característica aún no está disponible en el portal y ha de utilizarse mediante programación.
>

## <a name="modify-an-existing-indexer"></a>Modificación de un indexador existente

Si tiene un indexador existente, siga estos pasos para habilitar la indexación incremental.

### <a name="step-1-get-the-indexer"></a>Paso 1: Obtención del indizador

Comience con un indexador válido existente que tenga definidos el índice y el origen de datos necesarios. El indexador debe ser ejecutable. Con un cliente de API, construya una [solicitud GET](https://docs.microsoft.com/rest/api/searchservice/get-indexer) para obtener la configuración actual del indizador al que quiere agregar la indexación incremental.

```http
GET https://[service name].search.windows.net/indexers/[your indexer name]?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
```

### <a name="step-2-add-the-cache-property"></a>Paso 2: Adición de la propiedad cache

Edite la respuesta de la solicitud GET para agregar la propiedad `cache` al indizador. El objeto de caché únicamente requiere una sola propiedad, que es la cadena de conexión a la cuenta de Azure Storage.

```json
    "cache": {
        "storageConnectionString": "[your storage connection string]"
    }
```

### <a name="step-3-reset-the-indexer"></a>Paso 3: Restablecimiento del indizador

> [!NOTE]
> El restablecimiento del indexador hará que todos los documentos de su origen de datos se procesen de nuevo para poder almacenar en caché el contenido. Todos los enriquecimientos cognitivos se volverán a ejecutar en todos los documentos.
>

Se requiere un restablecimiento del indizador al configurar la indexación incremental de los indizadores existentes para asegurarse de que todos los documentos se encuentran en un estado coherente. Restablezca el indizador mediante la [API de REST](https://docs.microsoft.com/rest/api/searchservice/reset-indexer).

```http
POST https://[service name].search.windows.net/indexers/[your indexer name]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
```

### <a name="step-4-save-the-updated-definition"></a>Paso 4: Almacenamiento de la definición actualizada

Actualice la definición del indizador con una solicitud PUT, el cuerpo de la solicitud debe contener la definición del indizador actualizada.

```http
PUT https://[service name].search.windows.net/indexers/[your indexer name]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
{
    "name" : "your indexer name",
    ...
    "cache": {
        "storageConnectionString": "[your storage connection string]",
        "enableReprocessing": true
    }
}
```

Si ahora emite otra solicitud GET en el indizador, la respuesta del servicio incluirá una propiedad `cacheId` en el objeto de caché. `cacheId` es el nombre del contenedor que incluirá todos los resultados almacenados en caché y el estado intermedio de cada documento procesado por este indizador.

## <a name="enable-incremental-indexing-on-new-indexers"></a>Habilitación de la indexación incremental en los nuevos indexadores

Para configurar la indexación incremental para un indexador nuevo, incluya la propiedad `cache` en la carga útil de definición del indexador. Asegúrese de que utiliza la versión `2019-05-06-Preview` de la API.

## <a name="overriding-incremental-indexing"></a>Invalidación de la indexación incremental

Cuando está configurada, la indexación incremental realiza un seguimiento de los cambios a través de su canal de indexación y lleva los documentos a una consistencia final a través de su índice y sus proyecciones. En algunos casos, tendrá que invalidar este comportamiento para asegurarse de que el indizador no realice trabajo adicional como resultado de una actualización de la canalización de indexación. Por ejemplo, la actualización de la cadena de conexión al origen de datos requerirá un restablecimiento del indizador y volverá a indexar todos los documentos cuando el origen de datos haya cambiado. Pero si solo estaba actualizando la cadena de conexión con una clave nueva, no querrá que el cambio se traduzca en actualizaciones de los documentos existentes. Por el contrario, tal vez quiera que el indizador invalide la memoria caché y enriquezca los documentos aunque no se realicen cambios en la canalización de indexación. Por ejemplo, puede que quiera invalidar el indizador si va a volver a implementar una aptitud personalizada con un nuevo modelo y quiere que la aptitud se vuelva a ejecutar en todos los documentos.

### <a name="override-reset-requirement"></a>Invalidación del requisito de restablecimiento

Al realizar cambios en la canalización de indexación, cualquier cambio que dé lugar a una invalidación de la caché requiere un restablecimiento del indizador. Si realiza un cambio en la canalización del indizador y no quiere que el seguimiento de cambios invalide la memoria caché, debe establecer el parámetro QueryString `ignoreResetRequirement` en `true` para las operaciones en el indizador o el origen de datos.

### <a name="override-change-detection"></a>Invalidación de la detección de cambios

Al realizar actualizaciones del conjunto de aptitudes que den lugar a que los documentos se marquen como incoherentes, por ejemplo, al actualizar la URL de una aptitud personalizada cuando se vuelve a implementar la aptitud, establezca el parámetro `disableCacheReprocessingChangeDetection` de la cadena de consulta en `true` en las actualizaciones del conjunto de aptitudes.

### <a name="force-change-detection"></a>Forzado de la detección de cambios

Cuando quiera que la canalización de indexación reconozca un cambio en una entidad externa, como la implementación de una nueva versión de una aptitud personalizada, tendrá que actualizar el conjunto de aptitudes y "tocar" la aptitud específica editando la definición de la aptitud, específicamente la dirección URL para forzar la detección de cambios e invalidar la caché para esa aptitud.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se trata la indexación incremental de los indexadores que incluyen conjuntos de aptitudes. Para completar aún más sus conocimientos, revise los artículos sobre la reindexación en general, aplicables a todos los escenarios de indexación de Azure Cognitive Search.

+ [Recompilación de un índice de Azure Cognitive Search](search-howto-reindex.md). 
+ [Indexación de grandes conjuntos de datos en Azure Cognitive Search](search-howto-large-index.md). 
