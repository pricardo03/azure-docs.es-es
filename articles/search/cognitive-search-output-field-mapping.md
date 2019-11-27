---
title: Asignación de entrada a campos de salida
titleSuffix: Azure Cognitive Search
description: Extraiga y enriquezca los campos de datos de origen y asígnelos a los campos de salida de un índice de Búsqueda cognitiva de Azure.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d2d5e717154d16cc5579c1495aff9c1eebf54b17
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132379"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>Cómo asignar campos enriquecidos con IA a un índice de búsqueda

En este artículo, aprenderá a asignar campos de entrada enriquecidos a campos de salida en un índice de búsqueda. Una vez que haya [definido un conjunto de aptitudes](cognitive-search-defining-skillset.md), debe asignar los campos de salida de cualquier aptitud que aporte directamente valores a un campo dado del índice de búsqueda. Las asignaciones de campo son necesarias para mover contenido de documentos enriquecidos al índice.


## <a name="use-outputfieldmappings"></a>Usar outputFieldMappings
Para asignar los campos, agregue `outputFieldMappings` a la definición del indexador tal como se muestra a continuación:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

El cuerpo de la solicitud está estructurado de la siguiente manera:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
En cada asignación de campo de salida, establezca el nombre del campo enriquecido (sourceFieldName) y el nombre del campo, tal como se indica en el índice (targetFieldName).

La ruta de acceso en un campo sourceFieldName puede representar un elemento o varios elementos. En el ejemplo anterior, ```/document/content/sentiment``` representa un único valor numérico, mientras que ```/document/content/organizations/*/description``` representa varias descripciones de la organización. En casos donde hay varios elementos, estos son "reducidos" en una matriz que contiene cada uno de los elementos. Más concretamente, en el ejemplo ```/document/content/organizations/*/description```, los datos del campo *Descripciones* tienen el aspecto de una matriz plana de descripciones antes de que se indexe:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Pasos siguientes
Una vez haya asignado los campos enriquecidos a los campos de búsqueda, puede establecer los atributos de campo de cada uno de los campos de búsqueda [como parte de la definición del índice](search-what-is-an-index.md).

Para obtener más información acerca de las asignaciones de campos, consulte [Field mappings in Azure Cognitive Search indexers](search-indexer-field-mappings.md) (Asignaciones de campos en los indexadores de Búsqueda cognitiva de Azure).
