---
title: Aprenda a administrar cuentas de base de datos en Azure Cosmos DB
description: Aprenda a administrar cuentas de base de datos en Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: c27cee4842c0e65e1737f100a215cff82a0fd439
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033101"
---
# <a name="manage-indexing-in-azure-cosmos-db"></a>Administración automática en Azure Cosmos DB

En Azure Cosmos DB, puede elegir si desea que un contenedor indexe automáticamente todos los elementos o no. De forma predeterminada, todos los elementos de un contenedor de Azure Cosmos se indexan automáticamente, pero puede desactivar la indexación automática. Cuando se desactiva la indexación, se puede acceder a los elementos a través de sus propios vínculos o a través de consultas mediante un identificador del elemento como, por ejemplo, el identificador de documento. Se puede solicitar explícitamente proporcionar los resultados sin usar la indexación pasando el encabezado de **x-ms-documentdb-enable-scan** en la API REST o la opción de solicitud **EnableScanInQuery** mediante el SDK de .NET.

Cuando se desactiva la indexación automática, podrá agregar al índice de manera selectiva algunos elementos específicos. Por el contrario, puede dejar activada la indexación automática y excluir de forma selectiva elementos específicos. Las configuraciones de indexación activada/desactivada son útiles cuando tiene un subconjunto de los elementos que necesita consultar.  

Las unidades de procesamiento de escritura y de solicitud son proporcionales al número de valores que se deben indexar, el cual se especifica en el conjunto incluido en la directiva de indexación. Si conoce bien los patrones de consulta, puede elegir explícitamente el subconjunto de inclusión/exclusión de rutas de acceso para mejorar el procesamiento de escritura.

## <a name="manage-indexing-using-azure-portal"></a>Administración de la indexación mediante Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Cree una cuenta de Azure Cosmos DB o seleccione una ya existente.

3. Abra el panel **Explorador de datos**.

4. Seleccione un contenedor existente, expándalo y modifique los valores siguientes:

   * Abra la ventana **Escala y configuración**.
   * Cambie **indexingMode** de *coherente* a *ninguno* o incluya o excluya determinadas rutas de acceso de la indexación.
   * Haga clic en **Aceptar** para guardar los cambios.

   ![Administración de la indexación mediante Azure Portal](./media/how-to-manage-indexing/how-to-manage-indexing-portal.png)

## <a name="manage-indexing-using-azure-sdks"></a>Administración de la indexación mediante SDK de Azure

### <a id="dotnet"></a>SDK para .NET

En el ejemplo siguiente se muestra cómo incluir un elemento explícitamente mediante el [SDK de .NET para SQL API](sql-api-sdk-dotnet.md) y la propiedad [RequestOptions.IndexingDirective](/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective).

```csharp
// To override the default behavior to exclude (or include) a document in indexing,
// use the RequestOptions.IndexingDirective property.
client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new { id = "myDocumentId", isRegistered = true },
    new RequestOptions { IndexingDirective = IndexingDirective.Include });
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

* [Introducción a la indexación](index-overview.md)
* [Directiva de indexación](index-policy.md)
* [Tipos de índice](index-types.md)
* [Rutas de acceso del índice](index-paths.md)
