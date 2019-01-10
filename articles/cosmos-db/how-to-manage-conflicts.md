---
title: Aprenda a administrar conflictos entre regiones en Azure Cosmos DB
description: Aprenda a administrar conflictos en Azure Cosmos DB
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 1f06b27868547dd448e3da547dd61b861db913d2
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034784"
---
# <a name="manage-conflicts-between-regions"></a>Administración de conflictos entre regiones

Con las operaciones de escritura en varias regiones, cuando se produce un conflicto de datos, se puede resolver mediante el uso de distintas directivas de resolución de conflictos. En este artículo se describe cómo administrar directivas de resolución de conflictos mediante plataformas de lenguajes diferentes.

## <a name="create-a-custom-conflict-resolution-policy"></a>Creación de una directiva de resolución de conflictos personalizada

En estos ejemplos se muestran cómo configurar un contenedor con una directiva de resolución de conflictos personalizada. Estos conflictos se mostrarán en la fuente de conflictos.

### <a id="create-custom-conflict-resolution-policy-dotnet"></a>SDK para .NET

```csharp
DocumentCollection manualCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.manualCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-java-async"></a>SDK asincrónico para Java

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy();
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-java-sync"></a>SDK sincrónico para Java

```java
DocumentCollection manualCollection = new DocumentCollection();
manualCollection.setId(this.manualCollectionName);
ConflictResolutionPolicy customPolicy = ConflictResolutionPolicy.createCustomPolicy(null);
manualCollection.setConflictResolutionPolicy(customPolicy);
DocumentCollection createdCollection = client.createCollection(database.getSelfLink(), collection, null).getResource();
```

### <a id="create-custom-conflict-resolution-policy-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
const database = client.database(this.databaseName);
const {
  container: manualContainer
} = await database.containers.createIfNotExists({
  id: this.manualContainerName,
  conflictResolutionPolicy: {
    mode: "Custom"
  }
});
```

### <a id="create-custom-conflict-resolution-policy-python"></a>SDK para Python

```python
database = client.ReadDatabase("dbs/" + self.database_name)
manual_collection = {
                    'id': self.manual_collection_name,
                    'conflictResolutionPolicy': {
                          'mode': 'Custom'
                        }
                    }
manual_collection = client.CreateContainer(database['_self'], collection)
```

## <a name="create-a-custom-conflict-resolution-policy-with-a-stored-procedure"></a>Creación de una directiva de resolución de conflictos personalizada con un procedimiento almacenado

En estos ejemplos se muestran cómo configurar un contenedor con una directiva de resolución de conflictos personalizada con un procedimiento almacenado para resolver el conflicto. Estos conflictos no se mostrarán en la fuente de conflictos, salvo que haya un error en el procedimiento almacenado.

### <a id="create-custom-conflict-resolution-policy-stored-proc-dotnet"></a>SDK para .NET

```csharp
DocumentCollection udpCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.udpCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
          ConflictResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver"),
      },
  });
```

Una vez creado el contenedor, debe crear el procedimiento almacenado `resolver`.

### <a id="create-custom-conflict-resolution-policy-stored-proc-java-async"></a>SDK asincrónico para Java

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy("resolver");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

Una vez creado el contenedor, debe crear el procedimiento almacenado `resolver`.

### <a id="create-custom-conflict-resolution-policy-stored-proc-java-sync"></a>SDK sincrónico para Java

```java
DocumentCollection udpCollection = new DocumentCollection();
udpCollection.setId(this.udpCollectionName);
ConflictResolutionPolicy udpPolicy = ConflictResolutionPolicy.createCustomPolicy(
        String.format("dbs/%s/colls/%s/sprocs/%s", this.databaseName, this.udpCollectionName, "resolver"));
udpCollection.setConflictResolutionPolicy(udpPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, udpCollection);
```

Una vez creado el contenedor, debe crear el procedimiento almacenado `resolver`.

### <a id="create-custom-conflict-resolution-policy-stored-proc-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

Una vez creado el contenedor, debe crear el procedimiento almacenado `resolver`.

### <a id="create-custom-conflict-resolution-policy-stored-proc-python"></a>SDK para Python

```python

```

Una vez creado el contenedor, debe crear el procedimiento almacenado `resolver`.

## <a name="create-a-last-writer-wins-conflict-resolution-policy"></a>Creación de una directiva de resolución de conflictos del tipo "el último en escribir gana"

En estos ejemplos se muestra cómo configurar un contenedor con una directiva de resolución de conflictos del tipo "el último en escribir gana". Si no se establece la ruta de acceso o la que se establece no es válida, se usará de forma predeterminada la propiedad `_ts`. Esta propiedad es el campo de marca de tiempo. Estos conflictos no se mostrarán en la fuente de conflictos.

### <a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>SDK para .NET

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/regionId",
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-lww-java-async"></a>SDK asincrónico para Java

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy(conflictResolutionPath);
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-lww-java-sync"></a>SDK sincrónico para Java

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/regionId");
lwwCollection.setConflictResolutionPolicy(lwwPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, lwwCollection);
```

### <a id="create-custom-conflict-resolution-policy-lww-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
const database = client.database(this.databaseName);
const { container: lwwContainer } = await database.containers.createIfNotExists(
  {
    id: this.lwwContainerName,
    conflictResolutionPolicy: {
      mode: "LastWriterWins",
      conflictResolutionPath: "/regionId"
    }
  }
);
```

Si no usa la propiedad `conflictResolutionPath`, el valor predeterminado es la propiedad `_ts`.

### <a id="create-custom-conflict-resolution-policy-lww-python"></a>SDK para Python

```python
udp_collection = {
                'id': self.udp_collection_name,
                'conflictResolutionPolicy': {
                    'mode': 'Custom',
                    'conflictResolutionProcedure': 'dbs/' + self.database_name + "/colls/" + self.udp_collection_name + '/sprocs/resolver'
                    }
                }
udp_collection = self.try_create_document_collection(create_client, database, udp_collection)
```

## <a name="read-from-conflict-feed"></a>Lectura desde la fuente de conflictos

En estos ejemplos se muestra cómo leer desde la fuente de conflictos de un contenedor. Los conflictos solo se muestran en la fuente si no se han resuelto de forma automática.

### <a id="read-from-conflict-feed-dotnet"></a>SDK para .NET

```csharp
FeedResponse<Conflict> conflicts = await delClient.ReadConflictFeedAsync(this.collectionUri);
```

### <a id="read-from-conflict-feed-java-async"></a>SDK asincrónico para Java

```java
FeedResponse<Conflict> response = client.readConflicts(this.manualCollectionUri, null)
                    .first().toBlocking().single();
for (Conflict conflict : response.getResults()) {
    /* Do something with conflict */
}
```

### <a id="read-from-conflict-feed-java-sync"></a>SDK sincrónico para Java

```java
Iterator<Conflict> conflictsIterartor = client.readConflicts(this.collectionLink, null).getQueryIterator();
while (conflictsIterartor.hasNext()) {
    Conflict conflict = conflictsIterartor.next();
    /* Do something with conflict */
}
```

### <a id="read-from-conflict-feed-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
const container = client
  .database(this.databaseName)
  .container(this.lwwContainerName);

const { result: conflicts } = await container.conflicts.readAll().toArray();
```

### <a id="read-from-conflict-feed-python">Python</a>

```python
conflicts_iterartor = iter(client.ReadConflicts(self.manual_collection_link))
conflict = next(conflicts_iterartor, None)
while conflict:
    # Do something with conflict
    conflict = next(conflicts_iterator, None)
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de los siguientes conceptos de Azure Cosmos DB:

* [Creación de particiones y distribución de datos](partition-data.md)
* [Indexación en Azure Cosmos DB](indexing-policies.md)

