---
title: Indexar blobs que contiene varios buscar documentos del índice desde el indizador de Blob de Azure para la búsqueda de texto completo - Azure Search
description: Rastree los blobs de Azure para el contenido de texto mediante el indexador de blobs de Azure Search. Cada blob puede contener uno o varios documentos del índice de Azure Search.
ms.date: 02/12/2019
author: arv100kri
manager: briansmi
ms.author: arjagann
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seofeb2018
ms.openlocfilehash: e95eff015340659b642dff800a03f615e22c1577
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871206"
---
# <a name="indexing-blobs-producing-multiple-search-documents"></a>Indexación de blobs de varios documentos de búsqueda
De forma predeterminada, un indexador de blobs tratará el contenido de un blob como un documento de búsqueda única. Ciertos **parsingMode** valores admiten escenarios donde puede dar lugar a un blob individual en varios documentos de búsqueda. Los diferentes tipos de **parsingMode** que permiten a un indizador para extraer más de documento de una búsqueda de un blob es:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

> [!IMPORTANT]
> `jsonLines` modo de análisis está en versión preliminar pública y no debe usarse en entornos de producción. Para obtener más información, consulte [REST api-version=2017-11-11-Preview](search-api-2017-11-11-preview.md). 

## <a name="one-to-many-document-key"></a>Clave de documento de uno a varios
Cada documento que se muestra en un índice de Azure Search se identifica mediante una clave de documento. 

Cuando no se especifica ningún modo de análisis y si no hay ninguna explícita asignación para el campo de clave en el índice de Azure Search automáticamente [asigna](search-indexer-field-mappings.md) el `metadata_storage_path` propiedad como clave. Esta asignación garantiza que cada blob aparece como un documento de búsqueda distinct.

Al usar cualquiera de los modos de análisis mencionados anteriormente, un blob se asigna a "many" Buscar documentos, realizar una clave de documento, basándose solamente en los metadatos del blob no es adecuado. Para superar esta restricción, Azure Search es capaz de generar una clave de documento "uno a varios" para cada entidad individual extraído de un blob. Esta propiedad se denomina `AzureSearch_DocumentKey` y se agrega a cada entidad individual extraído del blob. El valor de esta propiedad se garantiza que sea único para cada entidad individual _entre blobs_ y las entidades se mostrarán como documentos individuales de búsqueda.

De forma predeterminada, cuando no hay asignaciones de campo explícito para el campo de clave de índice se especifican, el `AzureSearch_DocumentKey` asignadas a él, mediante la `base64Encode` función de asignación de campos.

## <a name="example"></a>Ejemplo
Supongamos que haya una definición de índice con los siguientes campos:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Y el contenedor de blobs tiene blobs con la estructura siguiente:

_Blob1.json_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2.json_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Cuando creas un indexador y establezca el **parsingMode** a `jsonLines` : sin especificar ningún campo explícita se aplicarán las asignaciones para el campo de clave, la siguiente asignación implícitamente
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Esta configuración dará como resultado el índice de búsqueda de Azure que contiene la información siguiente (Id. de codificada en base64 se ha reducido por motivos de brevedad)

| id | temperatura | presión |  timestamp |
|----|-------------|----------|-----------|
| aHR0 ... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Asignación de campo personalizado para el campo de clave de índice

Suponiendo que la misma definición de índice que el ejemplo anterior, supongamos que el contenedor de blobs tiene blobs con la estructura siguiente:

_Blob1.json_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2.json_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

Cuando se crea un indizador con `delimitedText` **parsingMode**, puede sentir natural para configurar una función de asignación de campos para el campo de clave como sigue:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Sin embargo, esta asignación le _no_ como resultado 4 documentos que se muestran en el índice, porque el `recordid` campo no es único _entre blobs_. Por lo tanto, se recomienda hacer uso de la asignación de campos implícita que se aplica desde la `AzureSearch_DocumentKey` propiedad para el campo de índice de clave para los modos de análisis de "uno a varios".

Si desea configurar la asignación de un campo explícito, asegúrese de que el _sourceField_ es distinta para cada entidad individual **entre todos los blobs**.

> [!NOTE]
> El enfoque usado por `AzureSearch_DocumentKey` de garantizar la unicidad por entidad extraído está sujeta a cambios y, por tanto, no debe confiar en su valor para las necesidades de su aplicación.

## <a name="see-also"></a>Vea también

+ [Indexadores de Azure Search](search-indexer-overview.md)
+ [Indexación de Azure Blob Storage con Azure Search](search-howto-index-json-blobs.md)
+ [Indexación de blobs CSV con el indexador de blobs de Azure Search](search-howto-index-csv-blobs.md)
+ [Indexación de blobs JSON con el indexador de blobs de Azure Search](search-howto-index-csv-blobs.md)

## <a name="NextSteps"></a>Pasos siguientes
* Para obtener más información sobre Azure Search, consulte la [página del servicio Search](https://azure.microsoft.com/services/search/).