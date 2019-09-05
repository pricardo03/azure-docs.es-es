---
title: Trabajar con proyecciones en un almacén de conocimiento (versión preliminar) - Azure Search
description: Guardar y dar forma a los datos enriquecidos de la canalización de indexación de inteligencia artificial para su uso en escenarios que no sean de búsqueda
manager: nitinme
author: vkurpad
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: vikurpad
ms.subservice: cognitive-search
ms.openlocfilehash: 2dd61a4511d406fefec5aacd0702fa732f79de92
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186234"
---
# <a name="working-with-projections-in-a-knowledge-store-in-azure-search"></a>Trabajar con proyecciones en un almacén de conocimiento en Azure Search

> [!Note]
> Knowledge Store se encuentra en versión preliminar y no está pensado para su uso en producción. En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con .NET SDK.
>

Azure Search permite el enriquecimiento de contenido a través de aptitudes cognitivas de inteligencia artificial y aptitudes personalizadas como parte de la indexación. Los enriquecimientos agregan estructura a los documentos y hacen que las búsquedas sean más eficaces. En muchos casos, los documentos enriquecidos son útiles para escenarios que no son de búsqueda, como, por ejemplo, para la minería de datos de conocimiento.

Las proyecciones, un componente del [almacén de conocimiento](knowledge-store-concept-intro.md), son vistas de documentos enriquecidos que se pueden guardar en el almacenamiento físico para fines de minería de datos de conocimiento. Una proyección le permite "proyectar" los datos en una forma que se adapte a sus necesidades y manteniendo las relaciones para que herramientas como Power BI puedan leer los datos sin ningún trabajo adicional. 

Las proyecciones pueden ser tabulares, con los datos almacenados en filas y columnas en el almacenamiento de tablas de Azure, o bien objetos JSON almacenados en el almacenamiento de blobs de Azure. Puede definir varias proyecciones de los datos a medida que se enriquecen. Esto es útil cuando quiere que los mismos datos tengan formas diferentes para casos de uso individuales. 

El almacén de conocimiento admite dos tipos de proyecciones:

+ **Tablas**: Para los datos que se representan mejor como filas y columnas, las proyecciones de tabla le permiten definir una forma esquematizada o una proyección en el almacenamiento de tablas. 

+ **Objetos**: Cuando necesita una representación JSON de los datos y enriquecimientos, las proyecciones de objeto se guardan como blobs.

Para ver las proyecciones definidas en contexto, consulte [Cómo empezar a trabajar con el almacén de conocimiento](knowledge-store-howto.md)

## <a name="projection-groups"></a>Grupos de proyecciones

En algunos casos, necesitará proyectar sus datos enriquecidos de formas diferentes para satisfacer objetivos diferentes. El almacén de conocimiento le permite definir varios grupos de proyecciones. Los grupos de proyecciones tienen las siguientes características clave de exclusividad mutua y relación.

### <a name="mutually-exclusivity"></a>Exclusividad mutua

Todo el contenido proyectado en un mismo grupo es independiente de los datos proyectados en otros grupos de proyecciones. Esto implica que puede tener los mismos datos repetidos en cada grupo de proyecciones, pero con una forma diferente. 

Una restricción que se aplica a los grupos de proyecciones es la exclusividad mutua de los tipos de proyecciones con un grupo de proyecciones. Solo puede definir proyecciones de tabla o proyecciones de objeto en un mismo grupo. Si quiere tanto tablas como objetos, defina un grupo de proyecciones para las tablas y un segundo grupo de proyecciones para los objetos.

### <a name="relatedness"></a>Relación

Todo el contenido proyectado dentro de un mismo grupo de proyecciones mantiene las relaciones entre los datos. Las relaciones se basan en una clave generada y cada nodo secundario mantiene una referencia al nodo primario. Las relaciones no abarcan los grupos de proyección, y las tablas u objetos creados en un grupo de proyecciones no tienen ninguna relación con los datos generados en otros grupos de proyecciones.

## <a name="input-shaping"></a>Dar forma a los datos de entrada
Dar la forma o la estructura adecuadas a los datos es clave para su uso eficaz, tanto si se trata de tablas como de objetos. La capacidad de modelar o estructurar los datos en función de cómo piensa utilizarlos y acceder a ellos es una capacidad clave denominada como la aptitud de **conformador** en el conjunto de aptitudes.  

Es más fácil definir las proyecciones si tiene un objeto en el árbol de enriquecimiento que coincida con el esquema de la proyección. La [aptitud de conformador](cognitive-search-skill-shaper.md) le permite crear un objeto a partir de diferentes nodos del árbol de enriquecimiento y convertirlos en secundarios de un nuevo nodo. La aptitud de **conformador** le permite definir tipos complejos con objetos anidados.

Cuando haya definido una forma nueva que contiene todos los elementos que necesita para realizar la proyección, podrá usar esta forma como origen para las proyecciones o como entrada para otra aptitud.

## <a name="table-projections"></a>Proyecciones de tabla

Puesto que facilita la importación, se recomienda el uso de proyecciones de tabla para la exploración de datos con Power BI. Además, las proyecciones de tabla permiten cambiar la cardinalidad entre la relación de la tabla. 

Puede proyectar un solo documento del índice en varias tablas, conservando las relaciones. Al proyectar a varias tablas, la forma completa se proyectará en cada tabla, a menos que un nodo secundario sea el origen de otra tabla dentro del mismo grupo.

### <a name="defining-a-table-projection"></a>Definir una proyección de tabla

Al definir una proyección de tabla dentro del elemento `knowledgeStore` del conjunto de aptitudes, empiece asignando un nodo del árbol de enriquecimiento al origen de la tabla. Normalmente, este nodo es el resultado de una aptitud de **conformador** que agregó a la lista de aptitudes para producir una forma concreta que necesita proyectar en las tablas. Puede segmentar el nodo que elija para proyectarlo en varias tablas. La definición de las tablas es una lista de las tablas que desea proyectar. 

Cada tabla requiere tres propiedades:

+ tableName: El nombre de la tabla en Azure Storage.

+ generatedKeyName: El nombre de columna para la clave que identifica de forma única esta fila.

+ source: El nodo del árbol de enriquecimiento del que obtiene sus enriquecimientos. Habitualmente es el resultado de un conformador, pero podría ser el resultado de cualquiera de las aptitudes.

A continuación se muestra un ejemplo de proyecciones de tabla.

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [
            { "tableName": "MainTable", "generatedKeyName": "SomeId", "source": "/document/EnrichedShape" },
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/EnrichedShape/*/KeyPhrases/*" },
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/EnrichedShape/*/Entities/*" }
          ]
        },
        {
          "objects": [
            
          ]
        }
      ]
    }
}
```
Como se muestra en este ejemplo, las entidades y frases clave se modelan en tablas diferentes y contendrán una referencia al elemento primario (MainTable) para cada fila. 

La siguiente ilustración es una referencia al ejercicio Caselaw en [Cómo empezar a trabajar con el almacén de conocimiento](knowledge-store-howto.md). En un escenario en el que un caso tiene varias opiniones, y cada opinión se enriquece identificando a las entidades que contiene, podría modelar las proyecciones como se muestra aquí.

![Entidades y relaciones en las tablas](media/knowledge-store-projection-overview/TableRelationships.png "Modelar las relaciones en las proyecciones de tabla")

## <a name="object-projections"></a>Proyecciones de objeto

Las proyecciones de objeto son representaciones JSON del árbol de enriquecimiento que pueden proceder de cualquier nodo. En muchos casos, la misma aptitud de **conformador** que crea una proyección de tabla se puede usar para generar una proyección de objeto. 

```json
{
 
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
          "objects": [
            {
              "storageContainer": "Reviews", 
              "format": "json", 
              "source": "/document/Review", 
              "key": "/document/Review/Id" 
            }
          ]
        }
      ]
    }
}
```

Para generar una proyección de objeto se requieren algunos atributos específicos del objeto:

+ storageContainer: El contenedor donde se guardarán los objetos
+ source: La ruta de acceso al nodo del árbol de enriquecimiento que es la raíz de la proyección
+ key: Una ruta de acceso que representa una clave única para el objeto que se almacenará. Se utilizará para crear el nombre del blob del contenedor.

## <a name="projection-lifecycle"></a>Ciclo de vida de las proyecciones

Las proyecciones tienen un ciclo de vida que está asociado a los datos de origen del origen de datos. A medida que los datos se actualizan y se vuelven a indexar, las proyecciones se actualizan con los resultados de los enriquecimientos para garantizar que son coherentes con los datos del origen de datos. Las proyecciones heredan la directiva de eliminación que ha configurado para el índice. 

## <a name="using-projections"></a>Uso de las proyecciones

Tras ejecutar el indexador, puede leer los datos proyectados en los contenedores o tablas que especificó a través de las proyecciones. 

En cuanto al análisis, la exploración en Power BI es tan sencilla como configurar el almacenamiento de Azure Table como origen de datos. Puede crear fácilmente un conjunto de visualizaciones en los datos aprovechando las relaciones que contienen.

Como alternativa, si tiene que usar los datos enriquecidos en una canalización de ciencia de datos, podría [cargar los datos desde los blobs en un DataFrame de Pandas](../machine-learning/team-data-science-process/explore-data-blob.md).

Por último, si tiene que exportar los datos desde el almacén de conocimiento, Azure Data Factory tiene conectores para exportar los datos y colocarlos en la base de datos que elija. 

## <a name="next-steps"></a>Pasos siguientes

Como siguiente paso, cree su primer almacén de conocimiento con instrucciones y datos de ejemplo.

> [!div class="nextstepaction"]
> [Cómo crear un almacén de conocimiento](knowledge-store-howto.md).