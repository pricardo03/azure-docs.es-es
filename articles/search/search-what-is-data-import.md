---
title: 'Importación de datos para su ingesta en un índice de búsqueda: Azure Search'
description: Rellene y cargue datos en un índice de Azure Search desde orígenes de datos externos.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 83ca0c11ab0065929d939b7345cbd15869740bb3
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024356"
---
# <a name="data-import-overview---azure-search"></a>Información general: importar datos de Azure Search

En Azure Search, las consultas se ejecutan sobre el contenido cargado y guardado en un [índice de búsqueda](search-what-is-an-index.md). Este artículo examina los dos enfoques básicos para rellenar un índice: *insertar* los datos en el índice mediante programación o apuntar un [indexador de Azure Search](search-indexer-overview.md) en un origen de datos admitido para *extraer* en los datos.

Con cualquier enfoque, el objetivo es *cargar datos* desde un origen de datos externo en un índice de Azure Search. Azure Search le permitirá crear un índice vacío, pero hasta que se insertar o extraer datos en él, no es consultable.

## <a name="pushing-data-to-an-index"></a>Inserción de datos en un índice
El modelo de inserción, que se usa para enviar a Azure Search los datos mediante programación, es el enfoque más flexible. En primer lugar, no tiene ninguna restricción en el tipo de origen de datos. Cualquier conjunto de datos que se compone de documentos JSON se puede insertar en un índice de Azure Search si cada documento en el conjunto de datos tiene campos asignados a los campos definidos en el esquema de índice. En segundo lugar, no tiene ninguna restricción en la frecuencia de ejecución. Puede insertar los cambios a un índice tantas veces como desee. En el caso de las aplicaciones con requisitos de latencia muy baja (por ejemplo, si se necesita que las operaciones de búsqueda estén sincronizadas con las bases de datos dinámicas del inventario), la única opción es un modelo de inserción.

Este enfoque es más flexible que el modelo de extracción, ya que los documentos se pueden cargar individualmente o en lotes (hasta 1000 por lote o 16 MB, lo que ocurra primero). El modelo de inserción también le permite cargar documentos en Azure Search independientemente de dónde están los datos.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Cómo insertar los datos en un índice de Azure Search

Puede usar las siguientes API para cargar uno o varios documentos en un índice:

+ [Agregar, Actualizar o Eliminar documentos (API de REST)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [Clase indexAction](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) o [clase indexBatch](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Actualmente no se admite ninguna herramienta para insertar datos mediante el portal.

Para obtener una introducción a cada metodología, consulte [inicio rápido: Crear un índice de Azure Search con PowerShell y la API de REST](search-create-index-rest-api.md) o [inicio rápido: Crear un índice de Azure Search en C# ](search-import-data-dotnet.md).

<a name="indexing-actions"></a>

### <a name="indexing-actions-upload-merge-mergeorupload-delete"></a>Acciones de indexación: cargar, combinar, combinar eliminar

Puede controlar el tipo de acción de indexación en una base por documento, especificando si se debe cargar el documento en completas, combinada con el contenido del documento existente o eliminado.

En la API de REST, emitirá solicitudes HTTP POST con cuerpos de solicitud JSON a la dirección URL del extremo del índice de Azure Search. Cada objeto JSON en la matriz de "value" contiene la clave del documento y especifica una acción de indexación incorporaciones, actualizaciones, o elimina el contenido del documento. Para obtener un ejemplo de código, vea [cargar documentos](search-create-index-rest-api.md#load-documents).

En el SDK. NET, empaquetar los datos en un `IndexBatch` objeto. Un `IndexBatch` encapsula una colección de `IndexAction` objetos, cada uno de los cuales contiene un documento y una propiedad que indica qué acción se debe realizar en el documento de Azure Search. Para obtener un ejemplo de código, vea [IndexBatch construir](search-import-data-dotnet.md#construct-indexbatch).


| @search.action | DESCRIPCIÓN | Campos necesarios para cada documento | Notas |
| -------------- | ----------- | ---------------------------------- | ----- |
| `upload` |Una acción `upload` es similar a un "upsert" donde se insertará el documento si es nuevo y se actualizará/reemplazará si ya existe. |la clave, además de cualquier otro campo que desee definir |Al actualizar o reemplazar un documento existente, cualquier campo que no esté especificado en la solicitud tendrá su campo establecido en `null`. Esto ocurre incluso cuando el campo se ha establecido previamente en un valor que no sea nulo. |
| `merge` |Permite actualizar un documento existente con los campos especificados. Si el documento no existe en el índice, se producirá un error en la combinación. |la clave, además de cualquier otro campo que desee definir |Cualquier campo que se especifica en una combinación reemplazará al campo existente en el documento. En el SDK. NET, esto incluye los campos de tipo `DataType.Collection(DataType.String)`. En la API de REST, esto incluye los campos de tipo `Collection(Edm.String)`. Por ejemplo, si el documento contiene un campo `tags` con el valor `["budget"]` y ejecuta una combinación con el valor `["economy", "pool"]` para `tags`, el valor final del campo `tags` será `["economy", "pool"]`. No será `["budget", "economy", "pool"]`. |
| `mergeOrUpload` |Esta acción se comporta como `merge` si ya existe un documento con la clave especificada en el índice. Si el documento no existe, se comporta como `upload` con un nuevo documento. |la clave, además de cualquier otro campo que desee definir |- |
| `delete` |Quita el documento especificado del índice. |solo la clave |Todos los campos que especifique que no sean el campo de clave, se omitirán. Si desea quitar un campo individual de un documento, use `merge` en su lugar y establezca el campo explícitamente con el valor NULL. |

## <a name="decide-which-indexing-action-to-use"></a>Elección de la acción de indexación que va a usar
Para importar datos mediante el SDK de .NET (carga, merge, delete y mergeOrUpload). Dependiendo de cuál de las acciones siguientes elija, se deberán incluir solo ciertos campos para cada documento:


### <a name="formulate-your-query"></a>Formulación de la consulta
Hay dos maneras de [realizar búsquedas en el índice mediante la API de REST](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Una de ellas es emitir una solicitud HTTP POST en la que los parámetros de consulta se definen en un objeto JSON del cuerpo de la solicitud. La otra es emitir una solicitud HTTP GET en la que los parámetros de la consulta se definen en la dirección URL de la solicitud. POST tiene unos [límites más flexibles](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) en relación con el tamaño de los parámetros de la consulta que GET. Por este motivo, se recomienda usar la solicitud POST a menos que haya circunstancias especiales en las que utilizar la solicitud GET sea más adecuado.

Tanto para la solicitud POST como para la GET, deberá proporcionar el *nombre del servicio*, el *nombre del índice*, así como la *versión adecuada de la API* (la versión actual de la API es `2019-05-06` en el momento de publicar este documento) en la URL de la solicitud. En el caso de GET, la *cadena de consulta* del final de la dirección URL es donde se proporcionar los parámetros de la consulta. Consulte a continuación el formato de dirección URL:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2019-05-06

El formato de la solicitud POST es el mismo, pero solo con la versión de API en los parámetros de la cadena de consulta.


## <a name="pulling-data-into-an-index"></a>Extracción de datos en un índice
El modelo de extracción rastrea un origen de datos compatible y carga automáticamente los datos en el índice. En Azure Search, esta funcionalidad se implementa a través de *indexadores*, que en este momento se encuentran disponibles en las siguientes plataformas:

+ [Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Table storage](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](https://aka.ms/documentdb-search-indexer)
+ [Azure SQL Database y SQL Server en máquinas virtuales de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Los indexadores conectan un índice a un origen de datos (normalmente una tabla, vista o estructura equivalente) y asignan campos de origen a los campos equivalentes del índice. Durante la ejecución, el conjunto de filas se transforma automáticamente en JSON y se carga en el índice especificado. Todos los indexadores admiten la programación, de modo que se puede especificar con qué frecuencia se deben actualizar los datos. La mayoría de los indexadores proporcionan seguimiento de cambios, siempre el origen de datos lo admita. Mediante el seguimiento de cambios y eliminaciones en documentos existentes, además de reconocer nuevos documentos, los indexadores eliminan la necesidad de administrar activamente los datos del índice. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Cómo extraer los datos en un índice de Azure Search

La funcionalidad del indexador se expone en [Azure Portal](search-import-data-portal.md) así como en la [API de REST](/rest/api/searchservice/Indexer-operations) y el [SDK de .NET](/dotnet/api/microsoft.azure.search.indexersoperationsextensions). 

Una ventaja del uso el portal es que Azure Search normalmente puede generar automáticamente un esquema de índice predeterminado mediante la lectura de los metadatos del conjunto de datos de origen. El índice generado se puede modificar hasta que se procese, tras lo cual las únicas modificaciones de esquema que se permiten son las que no requieren volver a indexar. Si los cambios que desea realizar afectan directamente el esquema, necesitaría volver a generar el índice. 

## <a name="verify-data-import-with-search-explorer"></a>Verificación de la importación de datos con el Explorador de búsqueda

Una forma rápida de realizar una comprobación preliminar en la carga de documentos es usar el **Explorador de búsqueda** en el portal. El explorador permite consultar un índice sin tener que escribir código. La búsqueda se basa en los valores predeterminados, como la [sintaxis simple](/rest/api/searchservice/simple-query-syntax-in-azure-search) y el [parámetro de consulta searchMode](/rest/api/searchservice/search-documents) predeterminado. Los resultados se devuelven en formato JSON, con el fin de que pueda revisar todo el documento.

> [!TIP]
> Muchos [ejemplos de código de Azure Search](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) incluyen conjuntos de datos incrustados o rápidamente disponibles, lo que supone una forma sencilla de empezar a trabajar. El portal también proporciona un indexador de ejemplo y un origen de datos que consta del conjunto de datos de una pequeña inmobiliaria (denominado "realestate-us-sample"). Al ejecutar el indexador preconfigurado en el origen de datos de ejemplo, se crea un índice que se carga con documentos que, luego, se pueden consultar en el Explorador de búsqueda o mediante un código creado por usted.

## <a name="see-also"></a>Vea también

+ [Información general del indexador](search-indexer-overview.md)
+ [Tutorial del portal: crear, cargar, consultar un índice](search-get-started-portal.md)
