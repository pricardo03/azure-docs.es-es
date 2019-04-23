---
title: Pasos de antes de la migración para migraciones de datos de MongoDB a la API de Azure Cosmos DB para MongoDB
description: Este documento proporciona información general sobre los requisitos previos para una migración de datos desde MongoDB a Cosmos DB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: roaror
ms.openlocfilehash: 476a143555323bbb5058541000a5b1a26d23b71a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014419"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Pasos de antes de la migración para migraciones de datos de MongoDB a la API de Azure Cosmos DB para MongoDB

Antes de migrar los datos de MongoDB (ya sea local o en la nube (IaaS)) a la API de Azure Cosmos DB para MongoDB, debe:

1. [Creación de una cuenta de Azure Cosmos DB](#create-account)
2. [Calcular el rendimiento necesario para las cargas de trabajo](#estimate-throughput)
3. [Elegir una clave de partición óptima para sus datos](#partitioning)
4. [Comprender la directiva de indexación que se puede establecer en los datos](#indexing)

Si ya ha completado los anteriores requisitos previos para la migración, consulte el [MongoDB migrar datos a la API de Azure Cosmos DB para MongoDB](../dms/tutorial-mongodb-cosmos-db.md) para las instrucciones de migración de datos reales. De lo contrario, este documento proporciona instrucciones para controlar estos requisitos previos. 

## <a id="create-account"></a> Crear una cuenta de Azure Cosmos DB 

Antes de comenzar la migración, deberá [crear una cuenta de Azure Cosmos mediante API de Azure Cosmos DB para MongoDB](create-mongodb-dotnet.md). 

En la creación de la cuenta, puede seleccionar opciones que [distribuir globalmente](distribute-data-globally.md) sus datos. También tiene la opción Habilitar escrituras en varias regiones (o configuración de varios maestros), que permite que cada una de las regiones para ser tanto una operación de escritura y la región de lectura.

![Creación de cuentas](./media/mongodb-pre-migration/account-creation.png)

## <a id="estimate-throughput"></a> Calcular la necesidad de rendimiento para cargas de trabajo

Antes de iniciar la migración mediante el uso de la [Database Migration Service (DMS)](../dms/dms-overview.md), debe calcular la cantidad de rendimiento para aprovisionar para sus colecciones y bases de datos de Azure Cosmos.

El rendimiento se puede aprovisionar en uno:

- Colección

- Base de datos

> [!NOTE]
> También puede tener una combinación de los pasos anteriores, donde es posible que haya dedicado algunas colecciones en una base de datos de rendimiento aprovisionado y otros usuarios pueden compartir el rendimiento. Para obtener más información, consulte el [establecimiento del rendimiento en una base de datos y un contenedor](set-throughput.md) página.
>

Primero debe decidir si desea aprovisionar la base de datos o el rendimiento de nivel de colección o una combinación de ambos. En general, se recomienda configurar un rendimiento dedicado en el nivel de colección. Aprovisionar el rendimiento en el nivel de base de datos permite que las colecciones en la base de datos para compartir el rendimiento aprovisionado. Sin embargo, con un rendimiento compartido, hay ninguna garantía para un procesamiento específico en cada colección individual y no obtendrá un rendimiento predecible en cualquier colección específica.

Si no está seguro acerca de cuánto rendimiento debe estar dedicado a cada colección individual, puede elegir el nivel de base de datos de rendimiento. Puede pensar en el rendimiento aprovisionado configurado en la base de datos de Azure Cosmos como un equivalente lógico de la capacidad de proceso de una VM MongoDB o un servidor físico, pero es más rentable con la capacidad de escalar. Para obtener más información, consulte [aprovisionamiento del rendimiento en bases de datos y contenedores de Azure Cosmos](set-throughput.md).

Si se aprovisiona el rendimiento en el nivel de base de datos, todas las recopilaciones creadas dentro de esa base de datos deben crearse con una clave de partición o particiones. Para obtener más información acerca de las particiones, consulte [partición y escalado horizontal en Azure Cosmos DB](partition-data.md). Si no especifica una clave de partición o particiones durante la migración, Azure Database Migration Service rellena automáticamente el campo de clave de partición con un *_id* atributo que se genera automáticamente para cada documento.

### <a name="optimal-number-of-request-units-rus-to-provision"></a>Número óptimo de unidades de solicitud (RU) para aprovisionar

En Azure Cosmos DB, el rendimiento aprovisionado de antemano y se mide en unidades de solicitud (RU) por segundo. Si tiene cargas de trabajo que se ejecutan MongoDB en una máquina virtual o en el entorno local, pensar de RU como una sencilla abstracción para los recursos físicos, como el tamaño de una máquina virtual o servidor-an local y los recursos que poseen, por ejemplo, memoria, CPU, e/s por segundo. 

A diferencia de las máquinas virtuales o servidores locales, unidades de solicitud son fáciles de escalar y reducir verticalmente en cualquier momento. Puede cambiar el número de unidades reservadas aprovisionadas en cuestión de segundos y se le cobrará solo por el número máximo de RU que aprovisiona durante un período determinado de una hora. Para más información, consulte [Unidades de solicitud en Azure Cosmos DB](request-units.md).

Estos son factores clave que afectan al número de RU necesarias:
- **Tamaño del elemento (es decir, el documento)**: A medida que aumenta el tamaño de un documento de elemento, el número de RU usadas para leer o escribir el documento de elemento también aumenta.
- **Recuento de propiedades del elemento**: Suponiendo que el [indexación predeterminada](index-overview.md) en todas las propiedades, el número de RU usadas para escribir un elemento aumenta a medida que aumenta el recuento de propiedades de elemento. Puede reducir el consumo de unidades de solicitud para las operaciones de escritura por [limitando el número de propiedades indizadas](index-policy.md).
- **Las operaciones simultáneas**: Solicitar unidades consumidas también depende de la frecuencia con la que diferentes operaciones CRUD (por ejemplo, escrituras, lecturas, las actualizaciones, eliminaciones) y se ejecutan consultas más complejas. Puede usar [mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/) a las necesidades de simultaneidad de los datos de MongoDB actuales de salida.
- **Patrones de consultas**: La complejidad de una consulta afecta a cuántas unidades de solicitud consumidas por la consulta.

Si exporta los archivos JSON con [mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/) y comprender cómo muchas escrituras, lecturas, actualizaciones y eliminaciones que tienen lugar por segundo, puede usar el [planeador de capacidad de Azure Cosmos DB](https://www.documentdb.com/capacityplanner) para estimar el número inicial de RU para aprovisionar. El programador de capacidad no cuenta el costo de las consultas más complejas. Por lo tanto, si tiene consultas complejas en los datos, se consumirán RU adicionales. La calculadora también se supone que todos los campos se indizan y se usa la coherencia de sesión. La mejor manera de comprender el costo de las consultas es migrar los datos (o datos de ejemplo) a Azure Cosmos DB, [conectarse al punto de conexión de Cosmos DB](connect-mongodb-account.md) y ejecutar una consulta de ejemplo desde el Shell de MongoDB mediante el `getLastRequestStastistics` comando para obtener el cargo de solicitud, lo que dará como resultado el número de unidades de solicitud consumidas:

`db.runCommand({getLastRequestStatistics: 1})`

Este comando dará como resultado un documento JSON similar al siguiente:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Después de comprender el número de unidades de solicitud consumidas por una consulta y las necesidades de simultaneidad para esa consulta, puede ajustar el número de RU aprovisionadas. Optimizar unidades de solicitud no es un evento único, debe optimizar continuamente o escalar verticalmente las RU aprovisionadas, dependiendo de si no se espera un tráfico pesado, en lugar de una gran carga de trabajo o importación de datos.

## <a id="partitioning"></a>Elija la clave de partición
Creación de particiones es un punto clave de cuenta antes de migrar a una base de datos distribuido globalmente como Azure Cosmos DB. Azure Cosmos DB usa la creación de particiones para escalar contenedores individuales en una base de datos para satisfacer las necesidades de escalabilidad y rendimiento de la aplicación. En las particiones, los elementos de un contenedor se dividen en distintos subconjuntos llamados particiones lógicas. Para obtener información detallada y recomendaciones sobre cómo elegir la clave de partición correcta para los datos, vea el [elección de una sección de la clave de partición](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). 

## <a id="indexing"></a>Indexar los datos
De forma predeterminada, Azure Cosmos DB indexa todos los campos de datos tras la ingesta. Puede modificar el [directiva de indexación](index-policy.md) en Azure Cosmos DB en cualquier momento. De hecho, a menudo se recomienda desactivar la indexación al migrar datos y, a continuación, activarlo cuando los datos ya están en Cosmos DB. Para obtener más detalles sobre la indexación, puede leer más información al respecto en el [indexación en Azure Cosmos DB](index-overview.md) sección. 

[Azure Database Migration Service](../dms/tutorial-mongodb-cosmos-db.md) migra automáticamente las colecciones de MongoDB con los índices únicos. Sin embargo, los índices únicos deben crearse antes de la migración. Azure Cosmos DB no admite la creación de índices únicos, cuando ya hay datos en las colecciones. Para más información, consulte [Claves únicas en Azure Cosmos DB](unique-keys.md).

## <a name="next-steps"></a>Pasos siguientes
* [Migrar los datos de MongoDB a Cosmos DB mediante el servicio de migración de base de datos.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Aprovisionamiento del rendimiento en bases de datos y contenedores de Azure Cosmos](set-throughput.md)
* [Creación de particiones en Azure Cosmos DB](partition-data.md)
* [Distribución global en Azure Cosmos DB](distribute-data-globally.md)
* [Indexación en Azure Cosmos DB](index-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)
