---
title: 'Creación de conceptos y definición de un índice: Azure Search'
description: Introducción a los términos y conceptos de índice en Azure Search, incluidos los elementos de los componentes y la estructura física.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/01/2019
ms.custom: seodec2018
ms.openlocfilehash: 77f4b597ad4b87db7e720dd57191c6b192a4c93b
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000958"
---
# <a name="create-a-basic-index-in-azure-search"></a>Creación de un índice básico en Azure Search

En Azure Search, un *índice* es un almacén persistente de *documentos* y otras construcciones que se usa para las búsquedas de texto completo y filtrado en los servicios de Azure Search. Desde un punto de vista conceptual, un documento es una sola unidad de datos habilitada para búsquedas en el índice. Por ejemplo, un minorista de comercio electrónico podría tener un documento para cada objeto que vende, una organización de noticias podría tener un documento para cada artículo, etc. Estos conceptos pueden equipararse a equivalentes de base de datos más conocidos: un *índice* es conceptualmente similar a una *tabla* y los *documentos* son más o menos equivalentes a las *filas* de una tabla.

Al agregar o cargar un índice, Azure Search crea las estructuras físicas basadas en el esquema que proporciona. Por ejemplo, si un campo del índice está marcado como habilitado para búsquedas, se crea un índice invertido para ese campo. Posteriormente, cuando agrega o carga documentos o cuando envía consultas de búsqueda a Azure Search, está enviando las solicitudes a un índice específico del servicio de búsqueda. La carga de campos con valores de documento se denomina *indexación* o ingesta de datos.

Puede crear un índice en el portal, la [API REST](search-create-index-rest-api.md) o el [SDK de .NET](search-create-index-dotnet.md).

## <a name="components-of-an-index"></a>Componentes de un índice

De forma esquemática, un índice de Azure Search se compone de los siguientes elementos. 

La [*recopilación de campos*](#fields-collection) es normalmente el elemento más grande de un índice, en el que cada campo recibe un nombre, se escribe y se le asignan unos atributos con los comportamientos permitidos que determinan cómo se usa. Otros elementos incluyen [proveedores de sugerencias](#suggesters), [perfiles de puntuación](#scoring-profiles), [analizadores](#analyzers) con elementos de componentes que admitan personalización y opciones de [CORS](#cors).

```json
{  
  "name": (optional on PUT; required on POST) "name_of_index",  
  "fields": [  
    {  
      "name": "name_of_field",  
      "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",  
      "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),  
      "filterable": true (default) | false,  
      "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),  
      "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),  
      "key": true | false (default, only Edm.String fields can be keys),  
      "retrievable": true (default) | false,  
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }  
  ],  
  "suggesters": [  
    {  
      "name": "name of suggester",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": ["field1", "field2", ...]  
    }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "name of scoring profile",  
      "text": (optional, only applies to searchable fields) {  
        "weights": {  
          "searchable_field_name": relative_weight_value (positive #'s),  
          ...  
        }  
      },  
      "functions": (optional) [  
        {  
          "type": "magnitude | freshness | distance | tag",  
          "boost": # (positive number used as multiplier for raw score != 1),  
          "fieldName": "...",  
          "interpolation": "constant | linear (default) | quadratic | logarithmic",  
          "magnitude": {  
            "boostingRangeStart": #,  
            "boostingRangeEnd": #,  
            "constantBoostBeyondRange": true | false (default)  
          },  
          "freshness": {  
            "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)  
          },  
          "distance": {  
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)  
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)  
          },  
          "tag": {  
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)  
          }  
        }  
      ],  
      "functionAggregation": (optional, applies only when functions are specified)   
        "sum (default) | average | minimum | maximum | firstMatching"  
    }  
  ],  
  "analyzers":(optional)[ ... ],
  "charFilters":(optional)[ ... ],
  "tokenizers":(optional)[ ... ],
  "tokenFilters":(optional)[ ... ],
  "defaultScoringProfile": (optional) "...",  
  "corsOptions": (optional) {  
    "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],  
    "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)  
  }  
}
```

## <a name="fields-collection-and-attribution"></a>Recopilación y atribución de campos
Al definir el esquema, debe especificar el nombre, el tipo y los atributos de cada campo del índice. El tipo de campo permite clasificar los datos que se almacenan en ese campo. Los atributos se establecen en campos individuales para especificar cómo se usa el campo. En la tabla siguiente se enumeran los tipos y los atributos que puede especificar.

### <a name="data-types"></a>Tipos de datos
| Type | DESCRIPCIÓN |
| --- | --- |
| *Edm.String* |Texto que opcionalmente se puede acortar para búsquedas de texto completo (separación de palabras, lematización, etc.). |
| *Collection(Edm.String)* |Una lista de cadenas que opcionalmente se pueden acortar para búsquedas de texto completo. En teoría, no hay ningún límite superior para el número de elementos de una colección, pero el límite de 16 MB en el tamaño de la carga se aplica a las colecciones. |
| *Edm.Boolean* |Contiene valores true/false. |
| *Edm.Int32* |Valores enteros de 32 bits. |
| *Edm.Int64* |Valores enteros de 64 bits. |
| *Edm.Double* |Datos numéricos de precisión doble. |
| *Edm.DateTimeOffset* |Los valores de hora y fecha se representan con el formato OData V4 (por ejemplo, `yyyy-MM-ddTHH:mm:ss.fffZ` o `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Un punto que representa una ubicación geográfica en todo el mundo. |

Puede encontrar información más detallada sobre los [tipos de datos de Azure Search admitidos aquí](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="index-attributes"></a>Atributos de índice
| Atributo | DESCRIPCIÓN |
| --- | --- |
| *Clave* |Una cadena que proporciona el identificador único de cada documento, que se usa para buscar los documentos. Todos los índices deben tener una clave. Solo un campo puede ser la clave y se debe establecer su tipo en Edm.String. |
| *Retrievable* |Establece si el campo se puede devolver en un resultado de búsqueda. |
| *Filterable* |Permite que el campo se use en consultas de filtro. |
| *Sortable* |Permite que una consulta ordene los resultados de búsqueda mediante este campo. |
| *Facetable* |Permite que un campo se use en una estructura de [navegación con facetas](search-faceted-navigation.md) para el filtrado autodirigido. Normalmente los campos que contienen valores repetitivos que se pueden usar para agrupar varios documentos (por ejemplo, varios documentos que forman parte de una única categoría de servicio o un único producto) funcionan mejor como facetas. |
| *Searchable* |Marca el campo como campo de búsqueda de texto completo. |

Puede encontrar información más detallada sobre los [atributos de índice de Azure Search aquí](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="suggesters"></a>Proveedores de sugerencias
Un proveedor de sugerencias es una sección del esquema que define qué campos de un índice se utilizan para admitir consultas con la función Autocompletar o con escritura automática en las búsquedas. Normalmente las cadenas de búsqueda parcial se envían a las sugerencias (API REST de Azure Search Service) mientras el usuario está escribiendo una consulta de búsqueda y la API devuelve un conjunto de frases sugeridas. El proveedor de sugerencias que defina en el índice determina qué campos se utilizan para generar los términos de búsqueda de escritura anticipada. Para más información sobre los detalles de configuración, consulte [Add suggesters](index-add-suggesters.md) (Agregar proveedores de sugerencias).

## <a name="scoring-profiles"></a>Perfiles de puntuación

Un perfil de puntuación es una sección del esquema que define comportamientos de puntuación personalizados que permiten influir en los elementos que aparecen más arriba en los resultados de la búsqueda. Los perfiles de puntuación se componen de ponderaciones de campos y de funciones. Para poder utilizarlos, especifique un perfil por nombre en la cadena de consulta.

El perfil de puntuación predeterminada funciona en segundo plano para calcular un resultado de búsqueda para todos los elementos de un conjunto de resultados. Puede usar un perfil de puntuación interno y sin nombre. Como alternativa, configure defaultScoringProfile para que use un perfil personalizado como valor predeterminado, al que se invoca cuando no se especifica un perfil personalizado en la cadena de consulta.

Consulte [Adición de perfiles de puntuación](index-add-scoring-profiles.md) para más información.

## <a name="analyzers"></a>Analizadores

El elemento de analizadores establece el nombre del analizador de idioma que se utilizará para el campo. Para más información acerca del conjunto de valores permitido, consulte [Language analyzers in Azure Search](index-add-language-analyzers.md) (Analizadores de idioma en Azure Search). Esta opción puede utilizarse solo con campos habilitados para la búsqueda y no se puede establecer junto con **searchAnalyzer** ni **indexAnalyzer**. Una vez que se elige el analizador, no se podrá cambiar para el campo.

## <a name="cors"></a>CORS

JavaScript del lado cliente no puede llamar a ninguna API de forma predeterminada debido a que el explorador impedirá todas las solicitudes entre orígenes. Para permitir consultas de origen cruzado en el índice, habilite CORS (uso compartido de recursos entre orígenes) estableciendo el atributo **corsOptions**. Por motivos de seguridad, solamente las API de consulta admiten CORS. 

Se pueden establecer las opciones siguientes para CORS:

+ **allowedOrigins** (obligatorio): se trata de una lista de orígenes a los que se le concederá acceso a su índice. Esto significa que cualquier código JavaScript que se suministre desde esos orígenes podrá consultar el índice (suponiendo que proporcione la clave de API correcta). Cada origen tiene normalmente el formato `protocol://<fully-qualified-domain-name>:<port>` aunque `<port>` se omite a menudo. Para más información, consulte [Uso compartido de recursos entre orígenes (wikipedia)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

  Si desea permitir el acceso a todos los orígenes, incluya `*` como elemento único en la matriz **allowedOrigins**. *Esto no es recomendable para los servicios de búsqueda de producción* pero a menudo resulta útil para el desarrollo y la depuración.

+ **maxAgeInSeconds** (opcional): los exploradores usan este valor para determinar la duración (en segundos) para almacenar en la memoria caché las respuestas preparatorias de CORS. Esto debe ser un entero no negativo. Cuanto mayor sea este valor es, mejor será el rendimiento, pero más tiempo tardarán en surtir efecto los cambios en la directiva CORS. Si no se establece, se usará una duración predeterminada de 5 minutos.

## <a name="next-steps"></a>Pasos siguientes

Con una descripción de la composición del índice, puede continuar en el portal para crear el primer índice.

> [!div class="nextstepaction"]
> [Incorporación de un índice (portal)](search-create-index-portal.md)