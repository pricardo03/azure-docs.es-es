---
title: Comandos de la extensión de MongoDB para administrar los datos almacenados en la API de Azure Cosmos DB para MongoDB
description: En este artículo se describe cómo usar los comandos de la extensión de MongoDB para administrar los datos almacenados en la API de Azure Cosmos DB para MongoDB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: aef77f121f20d867c8ec5e764d8c9639c961713d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60446591"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>Use los comandos de la extensión de MongoDB para administrar los datos almacenados en la API de Azure Cosmos DB para MongoDB 

Azure Cosmos DB es un servicio de base de datos con varios modelos y de distribución global de Microsoft. Puede comunicarse con la API de Azure Cosmos DB para MongoDB mediante cualquiera de código abierto [controladores de cliente de MongoDB](https://docs.mongodb.org/ecosystem/drivers). API de Azure Cosmos DB para MongoDB habilita el uso de los controladores de cliente existentes mediante la adhesión a la [protocolo de conexión de MongoDB](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Mediante las API de Azure Cosmos DB para MongoDB, puede disfrutar las ventajas de Cosmos DB como distribución global, particionamiento automático, alta disponibilidad, las garantías de latencia, automático, el cifrado en reposo, copias de seguridad, y muchas más, mientras conserva su inversión en en la aplicación de MongoDB.

## <a name="mongodb-protocol-support"></a>Soporte de protocolo de MongoDB

De forma predeterminada, las API de Azure Cosmos DB para MongoDB es compatible con MongoDB versión 3.2 del servidor para obtener más información, consulte [admite las características y sintaxis](mongodb-feature-support.md). Las funciones o los operadores de consulta que agregó en la versión 3.4 de MongoDB están disponibles actualmente como versión preliminar de API de Azure Cosmos DB para MongoDB. Los siguientes comandos de extensión admiten funcionalidad específica de Azure Cosmos DB cuando se realizan operaciones CRUD en los datos almacenados en la API de Azure Cosmos DB para MongoDB:

* [Crear base de datos](#create-database)
* [Actualizar base de datos](#update-database)
* [Obtener base de datos](#get-database)
* [Crear colección](#create-collection)
* [Actualizar colección](#update-collection)
* [Obtener colección](#get-collection)

## <a id="create-database"></a> Crear base de datos

El comando de extensión de base de datos de creación crea una nueva base de datos de MongoDB. Se usa el nombre de la base de datos desde el contexto de las bases de datos en la que se ejecutó el comando. El formato del comando CreateDatabase es como sigue:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

En la tabla siguiente se describe los parámetros dentro del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction   |  string  |   Nombre del comando personalizado, debe ser "CreateDatabase".      |
| offerThroughput | int  | Rendimiento aprovisionado que se establece en la base de datos. Este parámetro es opcional. |

### <a name="output"></a>Salida

Devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [resultado predeterminado](#default-output) de comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Creación de una base de datos**

Para crear una base de datos denominada "test", use el siguiente comando:

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**Crear una base de datos con un rendimiento**

Para crear una base de datos denominada "test" y el rendimiento aprovisionado de 1000 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a id="update-database"></a> Actualizar base de datos

El comando de extensión de base de datos de actualización actualiza las propiedades asociadas con la base de datos especificado. Actualmente, solo se puede actualizar la propiedad "offerThroughput".

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

En la tabla siguiente se describe los parámetros dentro del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction    |    string     |   Nombre del comando personalizado. Debe ser "UpdateDatabase".      |
|  offerThroughput   |  int       |     Rendimiento aprovisionado nuevo que desea establecer en la base de datos.    |

### <a name="output"></a>Salida

Devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [resultado predeterminado](#default-output) de comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Actualizar el rendimiento aprovisionado asociado con una base de datos**

Para actualizar el rendimiento aprovisionado de una base de datos con nombre "test" a 1200 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a id="get-database"></a> Obtener base de datos

El comando de extensión de la base de datos de get devuelve el objeto de base de datos. Se usa el nombre de la base de datos desde el contexto de base de datos en la que se ejecutó el comando.

```
{
  customAction: "GetDatabase"
}
```

En la tabla siguiente se describe los parámetros dentro del comando:


|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  customAction   |   string      |   Nombre del comando personalizado. Debe ser "GetDatabase"|
        
### <a name="output"></a>Salida

Si el comando se ejecuta correctamente, la respuesta contiene un documento con los siguientes campos:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  `ok`   |   `int`     |   Estado de respuesta. 1 == correcto. 0 == error.      |
| `database`    |    `string`        |   Nombre de la base de datos.      |
|   `provisionedThroughput`  |    `int`      |    Rendimiento aprovisionado que se establece en la base de datos. Este es un parámetro de respuesta opcional.     |

Si se produce un error en el comando, se devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [resultado predeterminado](#default-output) de comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Obtener la base de datos**

Para obtener el objeto de base de datos de una base de datos denominada "test", use el comando siguiente:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a id="create-collection"></a> Crear colección

El comando de extensión de colección de creación crea una nueva colección de MongoDB. Se usa el nombre de la base de datos desde el contexto de las bases de datos en la que se ejecutó el comando. El formato del comando CreateCollection es como sigue:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

En la tabla siguiente se describe los parámetros dentro del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction    | string | Nombre del comando personalizado. Debe ser "CreateDatabase"     |
| collection      | string | Nombre de la colección                                   |
| offerThroughput | int    | Rendimiento aprovisionado para establecer la base de datos. Es un parámetro opcional |
| shardKey        | string | Ruta de acceso de clave de partición para crear una colección con particiones. Es un parámetro opcional |

### <a name="output"></a>Salida

Devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [resultado predeterminado](#default-output) de comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Crear una colección unsharded**

Para crear una colección de unsharded con el nombre "testCollection" y el rendimiento aprovisionado de 1000 RU, use el siguiente comando: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Crear una colección con particiones**

Para crear una colección con particiones con nombre "testCollection" y el rendimiento aprovisionado de 1000 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a id="update-collection"></a> Actualizar colección

El comando update de la extensión de colección actualiza las propiedades asociadas a la colección especificada.

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

En la tabla siguiente se describe los parámetros dentro del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  customAction   |   string      |   Nombre del comando personalizado. Debe ser "UpdateCollection".      |
|  collection   |   string      |   Nombre de la colección.       |
| offerThroughput   |int|   Rendimiento aprovisionado para establecer en la colección.|

## <a name="output"></a>Salida

Devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [resultado predeterminado](#default-output) de comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Actualizar el rendimiento aprovisionado asociado a una colección**

Para actualizar el rendimiento aprovisionado de una colección con nombre "testCollection" a 1200 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a id="get-collection"></a> Obtener colección

El comando personalizado de colección get devuelve el objeto de colección.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

En la tabla siguiente se describe los parámetros dentro del comando:


|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction    |   string      |   Nombre del comando personalizado. Debe ser "GetCollection".      |
| collection    |    string     |    Nombre de la colección.     |

### <a name="output"></a>Salida

Si el comando se ejecuta correctamente, la respuesta contiene un documento con los siguientes campos


|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Estado de respuesta. 1 == correcto. 0 == error.      |
| `database`    |    `string`     |   Nombre de la base de datos.      |
| `collection`    |    `string`     |    Nombre de la colección.     |
|  `shardKeyDefinition`   |   `document`      |  Documento de especificaciones que se utiliza como una clave de partición de índice. Este es un parámetro de respuesta opcional.       |
|  `provisionedThroughput`   |   `int`      |    Rendimiento aprovisionado para establecer en la colección. Este es un parámetro de respuesta opcional.     |

Si se produce un error en el comando, se devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [resultado predeterminado](#default-output) de comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Obtener la colección**

Para obtener el objeto de colección para una colección denominada "testCollection", use el comando siguiente:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a id="default-output"></a> Salida predeterminada de un comando personalizado

Si no se especifica una respuesta personalizada contiene un documento con los siguientes campos:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Estado de respuesta. 1 == correcto. 0 == error.      |
| `code`    |   `int`      |   Solo se devuelve cuando el comando produce un error (es decir, bien == 0). Contiene el código de error de MongoDB. Este es un parámetro de respuesta opcional.      |
|  `errMsg`   |  `string`      |    Solo se devuelve cuando el comando produce un error (es decir, bien == 0). Contiene un mensaje de error descriptivo. Este es un parámetro de respuesta opcional.      |

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede continuar para obtener información sobre los siguientes conceptos de Azure Cosmos DB: 

* [Indexación en Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Expiración automática de los datos de Azure Cosmos DB con período de vida](../cosmos-db/time-to-live.md)