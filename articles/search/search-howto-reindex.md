---
title: 'Recompilación de un índice de Azure Search o actualización del contenido que permite búsquedas: Azure Search'
description: Agregue elementos nuevos, actualice elementos o documentos existentes o elimine documentos obsoletos en una indexación de recompilación completa o incremental parcial para actualizar un índice de Azure Search.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 9c9af69e45af6a70c5327393a1c10385ba2c2aed
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316903"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Recompilación de un índice de Azure Search

Recompilar un índice cambia su estructura, alterando la expresión física del índice en el servicio de Azure Search. Por el contrario, actualizar un índice es una actualización solo de contenido para seleccionar los cambios más recientes de un origen de datos externo de colaboración. En este artículo se proporcionan instrucciones sobre cómo actualizar los índices, tanto estructural como esencialmente.

Se requieren permisos de lectura y escritura en el nivel de servicio para las actualizaciones del índice. Mediante programación, puede llamar a API de REST o .NET para una indexación de recompilación completa o incremental del contenido, con parámetros que especifican las opciones de actualización. 

Por lo general, las actualizaciones de un índice se realizan a petición. Sin embargo, en el caso de los índices que se rellenan con [indexadores](search-indexer-overview.md) específicos del origen, puede usar un programador integrado. El programador admite la actualización de documentos con una frecuencia de incluso cada quince minutos hasta el intervalo que desee y el patrón que necesite. Una frecuencia de actualización más rápida requiere insertar manualmente las actualizaciones del índice, quizás con una doble escritura en las transacciones, mediante la actualización simultánea del origen de datos externo y del índice de Azure Search.

## <a name="full-rebuilds"></a>Recompilaciones completas

Muchos de tipos de actualizaciones requieren una recompilación completa. Una recompilación completa se refiere a quitar un índice, tanto los datos como los metadatos, para luego volver a rellenar el índice a partir de los orígenes de datos externos. Mediante programación, [elimine](https://docs.microsoft.com/rest/api/searchservice/delete-index), [cree](https://docs.microsoft.com/rest/api/searchservice/create-index) y [recargue](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) el índice para recompilarlo. 

Después de la recompilación, recuerde que si probó modelos de consulta y perfiles de puntuación, puede esperar que haya variaciones en los resultados de la búsqueda si se modificó el contenido subyacente.

## <a name="when-to-rebuild"></a>Cuándo se debe realizar una recompilación

Planifique recompilaciones completas frecuentes durante el desarrollo activo, cuando los esquemas de índice están en proceso de cambio.

| Modificación | Estado de la recompilación|
|--------------|---------------|
| Cambiar el nombre de un campo, el tipo de datos o los [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index) | Por lo general, cambiar la definición de un campo implica la necesidad de una recompilación, excepto en lo que se refiere a estos [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index): Retrievable, SearchAnalyzer, SynonymMaps. Puede agregar los atributos Retrievable, SearchAnalyzer y SynonymMaps a un campo existente sin tener que recompilar su índice.|
| Agregar un campo | No es un requisito estricto para la recompilación. Los documentos indexados existentes tienen un valor NULL para el campo nuevo. En una futura nueva indexación, valores provenientes de los datos de origen reemplazarán los valores NULL que agregue Azure Search. |
| Eliminar un campo | No puede eliminar un campo directamente de un índice de Azure Search. En su lugar, debe hacer que la aplicación omita el campo "eliminado" para evitar usarlo. Físicamente, el contenido y la definición del campo permanecen en el índice hasta la próxima vez que recompile el índice con un esquema que omita el campo en cuestión.|

> [!Note]
> También se requiere una recompilación si cambia los niveles. Si en algún punto decide incluir más capacidad, no se realiza ninguna actualización local. Se debe crear un servicio nuevo en el punto de capacidad nuevo y los índices se deben compilar de cero en el servicio nuevo. 

## <a name="partial-or-incremental-indexing"></a>Indexación parcial o incremental

Una vez que hay un índice en producción, centre los cambios en la indexación incremental, por lo general sin interrupciones de servicio perceptibles. La indexación parcial o incremental es una carga de trabajo solo de contenido que sincroniza el contenido de un índice de búsqueda para reflejar el estado del contenido en un origen de datos de colaboración. Un documento que se agrega o elimina en el origen, se agrega o elimina en el índice. En el código, llame a la operación [Agregar, actualizar o eliminar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) o el equivalente de .NET.

> [!Note]
> Cuando se usan indexadores que rastrean los orígenes de datos externos, los mecanismos de seguimiento de cambios en los sistemas de origen se usan para la indexación incremental. En [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), se usa un campo `lastModified`. En [Azure Table Storage](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` tiene la misma finalidad. De manera similar, tanto el [indexador de Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) como el [indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md#indexing-changed-documents) tienen campos para marcar las actualizaciones de fila. Para más información sobre los indexadores, consulte la [información general sobre ellos](search-indexer-overview.md).


## <a name="see-also"></a>Otras referencias

+ [Información general del indexador](search-indexer-overview.md)
+ [Indexación de grandes conjuntos de datos a escala](search-howto-large-index.md)
+ [Indexing in the portal](search-import-data-portal.md) (Indexación en el portal)
+ [Azure SQL Database indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) (Indexador de Azure SQL Database)
+ [Indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Indexador de Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexador de Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Security in Azure Search](search-security-overview.md) (Seguridad en Azure Search)