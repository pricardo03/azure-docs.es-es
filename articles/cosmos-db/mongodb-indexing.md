---
title: Indexación en la API de Azure Cosmos DB para MongoDB
description: Presenta información general sobre las funcionalidades de indexación con la API de Azure Cosmos DB para MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: e51e96c0c553bcf37284878cab11f3ec592ddd05
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72753390"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Indexación mediante la API de Azure Cosmos DB para MongoDB

La API de Azure Cosmos DB para MongoDB aprovecha las funcionalidades de administración automática de índices de Cosmos DB. Como resultado, los usuarios tienen acceso a las directivas de indexación predeterminadas de Cosmos DB. Por lo tanto, si el usuario no ha definido índices o no se ha eliminado ningún índice, todos los campos se indexarán automáticamente de forma predeterminada cuando se insertan en una colección. Para la mayoría de los escenarios, se recomienda utilizar la directivas de indexación predeterminada establecida en la cuenta.

## <a name="indexing-for-version-36"></a>Indexación para la versión 3.6

Las cuentas que sirven a la versión 3.6 del al protocolo de conexión proporcionan una directiva de indexación predeterminada diferente de la proporcionada en las versiones anteriores. De forma predeterminada, solo el campo _ id está indexado. Para indexar campos adicionales, el usuario debe aplicar los comandos de administración de índices de MongoDB. Para aplicar una ordenación a una consulta, actualmente se debe crear un índice en los campos utilizados en la operación de ordenación.

### <a name="dropping-the-default-indexes-36"></a>Eliminación de los índices predeterminados (3.6)

En el caso de las cuentas que sirven a la versión 3.6 del protocolo de conexión, el único índice predeterminado es _ id, que no se puede quitar.

### <a name="creating-a-compound-index-36"></a>Creación de un índice compuesto (3.6)

Los índices compuestos verdaderos se admiten para las cuentas que usan el protocolo de conexión 3.6. El siguiente comando creará un índice compuesto en los campos "a" y "b": `db.coll.createIndex({a:1,b:1})`

Los índices compuestos se pueden usar para ordenar de forma eficaz en varios campos a la vez, por ejemplo: `db.coll.find().sort({a:1,b:1})`

## <a name="indexing-for-version-32"></a>Indexación para la versión 3.2

### <a name="dropping-the-default-indexes-32"></a>Eliminación de los índices predeterminados (3.2)

El comando siguiente se puede utilizar para eliminar los índices predeterminados de una colección ```coll```:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

### <a name="creating-a-compound-index-32"></a>Creación de un índice compuesto (3.2)

Los índices compuestos contienen referencias a varios campos de un documento. Lógicamente, son equivalentes a la creación de varios índices individuales por campo. Para aprovechar las ventajas de las optimizaciones de las técnicas de indexación de Cosmos DB, se recomienda crear varios índices individuales en lugar de un solo (no único) índice compuesto.

## <a name="common-indexing-operations"></a>Operaciones de indexación comunes

Las siguientes operaciones son comunes tanto para las cuentas que atienden la versión 3.6 del protocolo de conexión como para las que atienden a las versiones anteriores. 

## <a name="creating-unique-indexes"></a>Creación de índices únicos

Los [índices únicos](unique-keys.md) son útiles para exigir que dos o más documentos no contengan el mismo valor para los campos indexados.

>[!Important]
> Actualmente, se pueden crear índices únicos solo cuando la colección está vacía (no contiene documentos).

El comando siguiente crea un índice único en el campo "student_id":

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Para colecciones particionadas, según el comportamiento de MongoDB, la creación de un índice único requiere proporcionar la clave de partición. En otras palabras, todos los índices únicos de una colección particionada son índices compuestos donde uno de los campos es la clave de partición.

Los siguientes comandos crean una colección particionada ```coll``` (la clave de partición es ```university```) con un índice único en los campos student_id y university:

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

En el ejemplo anterior, si se omite la cláusula ```"university":1```, se devolvería un error con el mensaje siguiente:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

## <a name="ttl-indexes"></a>Índices TTL

Para habilitar la caducidad de documentos en una colección determinada, se debe crear un ["Índice TTL" (índice de período de vida)](../cosmos-db/time-to-live.md). Un índice TTL es un índice en el campo _ts con un valor de "expireAfterSeconds".

Ejemplo:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

El comando anterior provocará la eliminación de todos los documentos de la colección ```db.coll``` que no se han modificado en los últimos 10 segundos.

> [!NOTE]
> **_ts** es un campo específico de Cosmos DB y no es accesible desde los clientes de MongoDB. Es una propiedad reservada (sistema) que contiene la marca de tiempo de la última modificación del documento.

## <a name="migrating-collections-with-indexes"></a>Migración de colecciones con índices

Actualmente, solo se pueden crear índices únicos cuando la colección no contiene documentos. Las herramientas de migración de MongoDB más populares intentan crear los índices únicos después de importar los datos. Para prevenir este problema, se recomienda que los usuarios creen manualmente las colecciones y los índices únicos correspondientes en lugar de permitir que lo haga la herramienta de migración (con ```mongorestore```, este comportamiento se logra mediante la marca `--noIndexRestore` en la línea de comandos).

## <a name="next-steps"></a>Pasos siguientes

* [Indexación en Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Expiración automática de los datos de Azure Cosmos DB con período de vida](../cosmos-db/time-to-live.md)
