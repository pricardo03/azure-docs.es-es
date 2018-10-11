---
title: Resolución de conflictos de arquitectura multimaestro en Azure Cosmos DB
description: 'En este artículo se describen las categorías de conflicto y los modos de resolución de conflictos, como Últimos casos de escritura correcta (LWW), Personalizado: procedimiento definido por el usuario, y Personalizado: asíncrono en la arquitectura multimaestro de Azure Cosmos DB.'
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 39fd393e78a2b66749c6aa34a758b185b38effdf
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041194"
---
# <a name="multi-master-conflict-resolution-in-azure-cosmos-db"></a>Resolución de conflictos de arquitectura multimaestro en Azure Cosmos DB 

La arquitectura multimaestro de Azure Cosmos DB proporciona modos de resolución de conflictos globales para garantizar que los datos sean consistentes en todas las regiones donde se replican.

## <a name="conflict-categories"></a>Categorías de conflicto

Existen tres categorías de conflictos que pueden ocurrir al trabajar con los datos de Azure Cosmos DB:

* **Conflictos de inserción:** este tipo de conflicto ocurre cuando la aplicación inserta simultáneamente dos o más documentos con el mismo índice único (por ejemplo, id.) de dos o más regiones. En este caso, todas las escrituras pueden tener éxito inicialmente, pero según la directiva de resolución de conflictos que elija, solo se confirma un documento con el id. original.

* **Conflictos de reemplazo:** este tipo de conflicto ocurre cuando la aplicación actualiza un solo documento simultáneamente desde dos o más regiones.

* **Conflictos de eliminación:** este tipo de conflicto ocurre cuando la aplicación elimina un documento de una región y lo actualiza en una o más regiones. 

## <a name="conflict-resolution-modes"></a>Modos de resolución de conflictos

Azure Cosmos DB ofrece tres modos de resolución de conflictos. Los modos de resolución de conflictos se definen para cada colección.

> [!NOTE]
> Los usuarios de la API de SQL pueden elegir entre tres modos diferentes de resolución de conflictos. En cuanto al resto de modelos de API (MongoDB, Cassandra, Graph y Table), los conflictos se resuelven utilizando la directiva Últimos casos de escritura correcta.

### <a name="last-writer-wins-lww"></a>Últimos casos de escritura correcta (LWW)

Últimos casos de escritura correcta (LWW) es la directiva de resolución de conflictos predeterminada. En este modo, los conflictos se resuelven en función de un valor numérico que se ha pasado en una propiedad del documento.

El siguiente fragmento de código es un ejemplo de cómo configurar la directiva de resolución de conflictos "Últimos casos de escritura correcta" cuando se usa el SDK de .NET. El elemento "ConflictResolutionPath" define la ruta de acceso a la propiedad que se usa para resolver el conflicto. En este ejemplo, `/userDefinedId` es la ruta de acceso para la resolución de conflictos, y el documento con el mayor valor de `userDefinedId` siempre ganará el conflicto. Para registrar un modo de resolución de tipo "Últimos casos de escritura correcta", aprovisione la colección con el elemento ConflictResolutionPolicy tal como se muestra a continuación.

```csharp
DocumentCollection lwwCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
   Id = "myCollection", 
   ConflictResolutionPolicy = new ConflictResolutionPolicy
   {
      Mode = ConflictResolutionMode.LastWriterWins,
      ConflictResolutionPath = "/userDefinedId"
   }
});
```

 El modo de resolución "Últimos casos de escritura correcta" se aplica a diferentes categorías de conflictos de datos de la siguiente manera:

* **Conflictos de inserción y actualización:** si dos o más documentos entran en conflicto en las operaciones de inserción o reemplazo, el documento que contiene el mayor valor para la ruta de acceso de resolución de conflictos se convierte en el ganador (es decir, userDefinedId). Si varios documentos tienen el mismo valor numérico para la ruta de acceso de resolución de conflictos, el ganador seleccionado no es determinista. Sin embargo, todas las regiones confluirán en un solo ganador.

* **Conflicto de eliminación:** si hay conflictos de eliminación, la eliminación siempre gana sobre otros conflictos de reemplazo, independientemente del valor de la ruta de acceso para la resolución de conflictos.

### <a name="custom--user-defined-procedure"></a>Personalizado: procedimiento definido por el usuario

En este modo, es el usuario quien controla la resolución de conflictos al registrar un Procedimiento definido por el usuario (UDP) en la colección. Este procedimiento UDP tiene una firma específica. Si selecciona esta opción, pero no registra un procedimiento UDP, o si el UDP emite una excepción en tiempo de ejecución, los conflictos se escriben en la fuente de conflictos donde se pueden resolver individualmente.

Para registrar un modo personalizado de resolución de conflictos mediante procedimientos definidos por el usuario, aprovisione la colección con el elemento ConflictResolutionPolicy, tal como se muestra a continuación.

```csharp
DocumentCollection udpCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
  Id = "myCollection",
  ConflictResolutionPolicy = new ConflictResolutionPolicy
  {
     Mode = ConflictResolutionMode.Custom,
     ConflictResolutionProcedure = UriFactory.CreateStoredProcedureUri("myDatabase","myCollection","myUdpStoredProcedure").ToString()
  }
});
```

A continuación, agregue el procedimiento definido por el usuario a la colección tal como se muestra a continuación.

```csharp
StoredProcedure myUdpStoredProc = new StoredProcedure
{
   Id = "myUdpStoredProcedure",
   Body = myUdpStoredProcText
};
await myClient.UpsertStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("myDatabase", "myCollection"), myUdpStoredProc);
```

El procedimiento almacenado registrado con la colección tiene una firma especial. En este ejemplo que le mostramos a continuación, el procedimiento UDP usa la propiedad `UserDefinedId` para resolver conflictos. El documento con el mayor valor gana el conflicto.

```javascript
function myUdpStoredProcedure(incomingDocument, existingDocument, isDeleteConflict, conflictingDocuments){
    var collection = getContext().getCollection();

    if (!incomingDocument) {
        if (existingDocument) {

            collection.deleteDocument(existingDocument._self, {}, function(err, responseOptions) {
                if (err) throw err;
            });
        }
    } else if (isDeleteConflict) {
        // delete always wins.
    } else {
        var documentToUse = incomingDocument;

        if (existingDocument) {
            if (documentToUse.userDefinedId < existingDocument.userDefinedId) {
                documentToUse = existingDocument;
            }
        }

        var i;
        for (i = 0; i < conflictingDocuments.length; i++) {
            if (documentToUse.userDefinedId < conflictingDocuments[i].userDefinedId) {
                documentToUse = conflictingDocuments[i];
            }
        }

        tryDelete(conflictingDocuments, incomingDocument, existingDocument, documentToUse);
    }

    function tryDelete(documents, incoming, existing, documentToInsert) {
        if (documents.length > 0) {
            collection.deleteDocument(documents[0]._self, {}, function(err, responseOptions) {
                if (err) throw err;

                documents.shift();
                tryDelete(documents, incoming, existing, documentToInsert);
            });
        } else if (existing) {
            collection.replaceDocument(existing._self, documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        } else {
            collection.createDocument(collection.getSelfLink(), documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        }
    }
}

```

Este procedimiento tiene cuatro parámetros:

* **incomingDocument:** especifica la versión conflictiva del documento. El conflicto puede ser un conflicto de inserción, reemplazo o eliminación. Si se trata de un conflicto de eliminación, este será una imagen de eliminación (marcador de exclusión) sin contenido.

* **existingDocument:** especifica la versión confirmada del documento, que tiene el mismo valor "rid" que el elemento incomingDocument. Este parámetro se establece como NULL para un conflicto de inserción y eliminación.

* **isDeleteConflict:** es una marca booleana que indica si el documento entrante está en conflicto con un documento eliminado anteriormente. Cuando este parámetro se establece en true, el elemento existingDocument también será NULL.

* **conflicttingDocuments:** especifica una colección de la versión confirmada de todos los documentos en la base de datos, los cuales están en conflicto con el elemento incomingDocument de la columna de identificador, o con cualquier otro campo de índice único. Estos documentos tendrán un valor "rid" diferente en comparación con el elemento incomingDocument.

El procedimiento definido por el usuario tiene acceso total a la clave de partición de Cosmos DB y puede realizar cualquier operación de almacenamiento para resolver conflictos. Si el procedimiento definido por el usuario no confirma la versión del conflicto, el sistema eliminará el conflicto y el elemento existingDocument permanecerá confirmado. Si se produce un error en el procedimiento definido por el usuario o este no existe, Azure Cosmos DB agregará el conflicto a la fuente de conflictos de solo lectura donde se puede procesar de forma asíncrona, tal como se muestra en el [modo asincrónico de resolución de conflictos](#custom--asynchronous). 

### <a name="custom--asynchronous"></a>Personalizado: asincrónico  

En este modo, Azure Cosmos DB excluye la confirmación de todos los conflictos (insertar, reemplazar y eliminar) y los registra en la fuente de conflictos de solo lectura para que la aplicación del usuario se encargue de la resolución diferida. Esta aplicación puede resolver conflictos de forma asincrónica y usar cualquier lógica, o consultar cualquier origen, aplicación o servicio externo para resolver el conflicto.

Para registrar un modo asincrónico personalizado de resolución de conflictos, aprovisione la colección con el elemento ConflictResolutionPolicy, tal como se muestra a continuación.

```csharp
DocumentCollection manualCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
    Id = "myCollection",
    ConflictResolutionPolicy = new ConflictResolutionPolicy
    {
        Mode = ConflictResolutionMode.Custom
    }
});
```

Para leer y procesar cualquier conflicto de la fuente de conflictos, implemente el código que se muestra a continuación. Los datos almacenados en la fuente de conflictos conllevan un costo de almacenamiento adicional. Por lo tanto, se recomienda eliminar los datos almacenados en la fuente de conflictos después de que se procesen.

```csharp
FeedResponse<Conflict> response = await myClient.ReadConflictFeedAsync(myCollectionUri);
  foreach (Conflict conflict in response)
  {
      switch(conflict.OperationKind)
      {
         case OperationKind.Create:
         //Process Insert Conflicts.
         …
         case OperationKind.Replace:
         //Process Replace Conflicts
         …
         case OperationKind.Delete:
         //Process Delete Conflicts
         …
      }
  //Optionally delete the conflict after processed
  await myClient.DeleteConflictAsync(conflict.SelfLink);
  }
```

> [!NOTE]
> Recuerde que la fuente de conflictos no incluye ningún agente de escucha para enviar notificaciones para realizar el procesamiento posterior, como la fuente de cambios en Cosmos DB. Debe implementar la lógica para sondear la fuente de conflictos y determinar si realmente existen conflictos.

## <a name="code-samples"></a>Ejemplos de código

A continuación, se muestran aplicaciones de ejemplo que muestran la resolución de conflictos de las API enumeradas. Cada ejemplo genera conflictos dentro de un contenedor y luego muestra cómo se resuelven los mismos mediante cada modo de resolución de conflictos admitido.

|Modelo de API  | SDK |Muestra |
|---------|---------|---------|
|API de SQL    | .NET    |[azure-cosmos-db-sql-dotnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-dotnet-multi-master)  |
|API de SQL    | Nodo    |[azure-cosmos-js/samples/MultiRegionWrite/](https://github.com/Azure/azure-cosmos-js/tree/master/samples/MultiRegionWrite)  |
|API de SQL    | Java    |[azure-cosmos-db-sql-java-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-java-multi-master)  |
|MongoDB  | .NET    |[azure-cosmos-db-mongodb-dotnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-multi-master)   |
|Table API  | .NET    |[azure-cosmos-db-table-dotnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-multi-master)       |
|API de Gremlin | .NET | [azure-cosmos-db-gremlin-dontnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-gremlin-dontnet-multi-master)|

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha obtenido información acerca de las categorías de conflictos y los modos de resolución de conflictos para Azure Cosmos DB. A continuación, eche un vistazo a los siguientes recursos:

* [Usar los clientes de MongoDB con la arquitectura multimaestro](multi-master-oss-nosql.md)
