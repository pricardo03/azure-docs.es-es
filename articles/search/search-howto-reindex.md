---
title: 'Recompilación de un índice de Azure Search o actualización del contenido que permite búsquedas: Azure Search'
description: Agregue elementos nuevos, actualice elementos o documentos existentes o elimine documentos obsoletos en una indexación de recompilación completa o incremental parcial para actualizar un índice de Azure Search.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 2595912732389c8a415d1854a84a7b9c182e4dc7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871155"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Recompilación de un índice de Azure Search

En este artículo se explica cómo recompilar un índice de Azure Search, las circunstancias en las que las recompilaciones son necesarias y recomendaciones para mitigar el impacto de las recompilaciones en las solicitudes de consulta en curso.

Una *recompilación* se refiere a quitar y volver a crear las estructuras de datos físicas asociadas con un índice, incluidos todos los índices invertidos basados en campos. En Azure Search, no se pueden quitar y volver a crear campos individuales. Para recompilar un índice, se debe eliminar todo el almacenamiento de campos, se debe volver a crear según un esquema de índice existente o revisado y, a continuación, se debe volver a completar con los datos insertados en el índice o extraídos de orígenes externos. Es común recompilar los índices durante el desarrollo, pero también es posible que deba recompilar un índice de nivel de producción para dar cabida a cambios estructurales, como la adición de tipos complejos de campos a proveedores de sugerencias.

A diferencia de las recompilaciones que aceptan un índice sin conexión, la *actualización de datos* se ejecuta como una tarea en segundo plano. Puede agregar, quitar y reemplazar documentos con una interrupción mínima para las cargas de trabajo de las consultas, aunque las consultas suelen tardar más en completarse. Para obtener más información sobre cómo actualizar el contenido de un índice, consulte [Add, update, or delete documents in Azure Search](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) (Agregar, actualizar o eliminar documentos en Azure Search).

## <a name="rebuild-conditions"></a>Condiciones de recompilación

| Condición | DESCRIPCIÓN |
|-----------|-------------|
| Cambiar la definición de un campo | Revisar el nombre, el tipo de datos o los [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index) específicos (si se pueden buscar, filtrar, ordenar, clasificar) de un campo requiere una recompilación completa. |
| Asignar un analizador a un campo | Los [analizadores](search-analyzers.md) se definen en un índice y, a continuación, se asignan a los campos. Puede agregar una nueva definición de analizador a un índice en cualquier momento, pero solo puede *asignar* un analizador cuando se crea el campo. Esto es así tanto para la propiedad **analyzer** como para la propiedad **indexAnalyzer**. La propiedad **searchAnalyzer** es una excepción (puede asignar esta propiedad a un campo existente). |
| Actualizar o eliminar una definición de analizador en un índice | No puede eliminar ni cambiar una configuración de analizador existente (analizador, tokenizador, filtro de token o filtro de caracteres) en el índice, a menos que recompile todo el índice. |
| Agregar un campo a un proveedor de sugerencias | Si ya existe un campo y desea agregarlo a una construcción de [proveedor de sugerencias](index-add-suggesters.md), debe recompilar el índice. |
| Eliminar un campo | Para quitar físicamente todos los rastros de un campo, tendrá que recompilar el índice. Cuando no resulta práctica una recompilación inmediata, puede modificar el código de la aplicación para deshabilitar el acceso al campo "eliminado". Físicamente, el contenido y la definición del campo permanecen en el índice hasta la próxima recompilación, cuando aplica un esquema que omite el campo en cuestión. |
| Cambiar los niveles | Si necesita más capacidad, no hay ninguna actualización local. Se crea un servicio nuevo en el punto de capacidad nuevo y los índices se deben compilar de cero en el servicio nuevo. |

Cualquier otra modificación puede realizarse sin afectar a las estructuras físicas existentes. En concreto, los siguientes cambios *no* requieren una recompilación del índice:

+ Adición de un nuevo campo
+ Definición del atributo **retrievable** en un campo existente
+ Definición de **searchAnalyzer** en un campo existente
+ Adición de una nueva definición de analizador en un índice
+ Adición, actualización o eliminación de los perfiles de puntuación
+ Adición, actualización o eliminación de configuraciones CORS
+ Adición, actualización o eliminación de synonymMaps

Cuando agrega un campo nuevo, a los documentos indexados existentes se les asigna un valor NULL para el campo nuevo. En una futura nueva indexación, valores provenientes de los datos de origen reemplazarán los valores NULL que agregó Azure Search. Para obtener más información sobre cómo actualizar el contenido de un índice, consulte [Add, update, or delete documents in Azure Search](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) (Agregar, actualizar o eliminar documentos en Azure Search).

## <a name="partial-or-incremental-indexing"></a>Indexación parcial o incremental

En Azure Search, no se puede controlar la indexación por campo, ni se pueden eliminar o volver a crear campos específicos. De forma similar, no hay ningún mecanismo integrado para [indexar documentos en función de criterios](https://stackoverflow.com/questions/40539019/azure-search-what-is-the-best-way-to-update-a-batch-of-documents). Los requisitos que tenga para la indexación basada en criterios deben cumplirse mediante código personalizado.

Sin embargo, lo que puede hacer fácilmente, es *actualizar los documentos* en un índice. Para muchas soluciones de búsqueda, los datos de origen externo son volátiles y la sincronización entre el origen de datos y un índice de búsqueda es una práctica común. En el código, llame a la operación para [agregar, actualizar o eliminar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) o al [equivalente de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexesoperationsextensions.createorupdate?view=azure-dotnet) para actualizar el contenido del índice o para agregar valores a un nuevo campo.

## <a name="partial-indexing-with-indexers"></a>Indexación parcial con indizadores

Los [indizadores](search-indexer-overview.md) simplifican la tarea de actualización de datos. Un indizador puede indizar solo una tabla o vista del origen de datos externo. Para indexar varias tablas, el enfoque más sencillo es crear una vista que combine las tablas y proyecte las columnas que quiere indexar. 

Al utilizar indizadores que rastrean orígenes de datos externos, compruebe si hay una columna de "marca de agua" en el origen de datos. Si existe una, puede usarla para la detección de cambios incrementales si selecciona solo las filas con contenido nuevo o revisado. En [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), se usa un campo `lastModified`. En [Azure Table Storage](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` tiene la misma finalidad. De manera similar, tanto el [indexador de Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) como el [indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md#indexing-changed-documents) tienen campos para marcar las actualizaciones de fila. 

Para obtener más información sobre los indizadores, consulte la [información general sobre ellos](search-indexer-overview.md) y la [API REST para restablecer indizadores](https://docs.microsoft.com/rest/api/searchservice/reset-indexer).

## <a name="how-to-rebuild-an-index"></a>Cómo recompilar un índice

Planifique recompilaciones completas frecuentes durante el desarrollo activo, cuando los esquemas de índice están en proceso de cambio. Para las aplicaciones ya en producción, se recomienda crear un nuevo índice que se ejecute en paralelo a un índice existente para evitar el tiempo de inactividad de la consulta.

Se requieren permisos de lectura y escritura en el nivel de servicio para las actualizaciones del índice. 

No puede recompilar un índice en el portal. Mediante programación, puede llamar a la [API de REST para actualizar índices](https://docs.microsoft.com/rest/api/searchservice/update-index) o a las [API de .NET equivalentes](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexesoperations.createorupdatewithhttpmessagesasync?view=azure-dotnet) para una recompilación completa. Una solicitud de actualización del índice es idéntica a la de la [API de REST para crear índices](https://docs.microsoft.com/rest/api/searchservice/create-index), pero tiene un contexto diferente.

El siguiente flujo de trabajo está orientado hacia la API de REST, pero se aplica igualmente al SDK de .NET.

1. Si va a reutilizar un nombre de índice, [quite el índice existente](https://docs.microsoft.com/rest/api/searchservice/delete-index). 

   Se quitan inmediatamente todas las consultas destinadas a ese índice. Eliminar un índice es una acción irreversible, ya que se destruye el almacenamiento físico para la colección de campos y otras construcciones. Asegúrese de que entiende las implicaciones de eliminar un índice antes de quitarlo. 

2. Formule una solicitud de [actualización del índice](https://docs.microsoft.com/rest/api/searchservice/update-index) con el punto de conexión de servicio, la clave de API y una [clave de administración](https://docs.microsoft.com/azure/search/search-security-api-keys). La clave de administración es necesaria para las operaciones de escritura.

3. En el cuerpo de la solicitud, proporcione un esquema de índice con las definiciones de campo modificadas o cambiadas. El cuerpo de la solicitud contiene el esquema del índice, así como los constructores para los perfiles de puntuación, los analizadores, los proveedores de sugerencias y las opciones de CORS. Los requisitos del esquema se documentan en [crear un índice](https://docs.microsoft.com/rest/api/searchservice/create-index).

4. Envíe una solicitud de [actualización del índice](https://docs.microsoft.com/rest/api/searchservice/update-index) para recompilar la expresión física del índice en Azure Search. 

5. [Cargue documentos en el índice](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) desde un origen de datos externo.

Cuando se crea el índice, se asigna almacenamiento físico para cada campo en el esquema de índice, con un índice invertido que se crea para cada campo de búsqueda. Los campos que no pueden buscarse se pueden usar en filtros o expresiones, pero no cuentan con índices invertidos y no están habilitados para búsquedas de texto completo o aproximadas. En una recompilación de índices, se eliminan y vuelven a crear estos índices invertidos en función del esquema de índice proporcionado.

Al cargar el índice, el índice invertido de cada campo se rellena con todas las palabras únicas y con tokens de cada documento, con una asignación a los identificadores del documento correspondiente. Por ejemplo, al indizar un conjunto de datos de hoteles, el índice invertido creado para un campo Ciudad podría contener términos para Seattle, Portland y así sucesivamente. El identificador de documento de los documentos que incluyan Seattle o Portland en el campo Ciudad aparecerá junto con el término. En cualquier operación para [agregar, actualizar o eliminar](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents), los términos y la lista de identificadores de documento se actualizan en consecuencia.

> [!NOTE]
> Si tiene requisitos estrictos de Acuerdo de Nivel de Servicio, puede considerar la posibilidad de aprovisionar un nuevo servicio específicamente para esta tarea, para que el desarrollo y la indización se produzcan completamente aislados del índice de producción. Un servicio independiente se ejecuta en su propio hardware, lo que elimina cualquier posibilidad de contención de recursos. Una vez completado el desarrollo, puede dejar el nuevo índice en su lugar, redirigir las consultas hacia el nuevo punto de conexión e índice o puede ejecutar código terminado para publicar un índice revisado en el servicio de Azure Search original. Actualmente no hay ningún mecanismo para mover un índice listo para usarse a otro servicio.

## <a name="view-updates"></a>Visualización de actualizaciones

Puede empezar a consultar un índice en cuanto se carga el primer documento. Si conoce el identificador de un documento, la [API REST para buscar documentos](https://docs.microsoft.com/rest/api/searchservice/lookup-document) devuelve el documento específico. Para realizar pruebas más amplias, debe esperar hasta que el índice se haya cargado completamente y, a continuación, usar consultas para comprobar el contexto que espera ver.

## <a name="see-also"></a>Vea también

+ [Información general del indexador](search-indexer-overview.md)
+ [Indexación de grandes conjuntos de datos a escala](search-howto-large-index.md)
+ [Indexing in the portal](search-import-data-portal.md) (Indexación en el portal)
+ [Azure SQL Database indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) (Indexador de Azure SQL Database)
+ [Indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Indexador de Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexador de Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Security in Azure Search](search-security-overview.md) (Seguridad en Azure Search)