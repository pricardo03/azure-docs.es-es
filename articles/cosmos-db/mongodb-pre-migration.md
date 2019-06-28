---
title: Pasos previos a la migración para migraciones de datos de MongoDB a la API de Azure Cosmos DB para MongoDB
description: Este documento proporciona información general sobre los requisitos previos para una migración de datos de MongoDB a Cosmos DB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: roaror
ms.openlocfilehash: 476a143555323bbb5058541000a5b1a26d23b71a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61330868"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Pasos previos a la migración para migraciones de datos de MongoDB a la API de Azure Cosmos DB para MongoDB

Antes de migrar los datos de MongoDB (ya sea local o en la nube (IaaS)) a la API de Azure Cosmos DB para MongoDB, se debe hacer lo siguiente:

1. [Creación de una cuenta de Azure Cosmos DB](#create-account)
2. [Estimar el rendimiento necesario para las cargas de trabajo](#estimate-throughput).
3. [Elegir una clave de partición óptima para los datos](#partitioning).
4. [Comprender la directiva de indexación que se puede establecer en los datos](#indexing).

Si ya se han completado los anteriores requisitos previos para la migración, consulte [Migración de datos MongoDB a la API de Azure Cosmos DB para MongoDB](../dms/tutorial-mongodb-cosmos-db.md) para obtener las instrucciones reales sobre la migración de datos. En caso contrario, este documento proporciona instrucciones para tratar estos requisitos previos. 

## <a id="create-account"></a> Creación de una cuenta de Azure Cosmos DB 

Antes de empezar la migración, deberá [crear una cuenta de Azure Cosmos mediante la API de Azure Cosmos DB para MongoDB](create-mongodb-dotnet.md). 

Al crear la cuenta, puede seleccionar valores que [distribuyan globalmente](distribute-data-globally.md) sus datos. También tiene la opción de habilitar escrituras en varias regiones (o configuración de arquitectura multimaestro), que permite que cada región sea de escritura y de lectura al mismo tiempo.

![Creación de una cuenta](./media/mongodb-pre-migration/account-creation.png)

## <a id="estimate-throughput"></a> Estimación del rendimiento necesario para las cargas de trabajo

Antes de iniciar la migración mediante la [Database Migration Service (DMS)](../dms/dms-overview.md), debe calcular la cantidad de rendimiento que se va a aprovisionar para las colecciones y bases de datos de Azure Cosmos.

El rendimiento se puede aprovisionar en uno de los siguientes elementos:

- Colección

- Base de datos

> [!NOTE]
> También puede haber una combinación de lo anterior, en la que algunas colecciones en una base de datos puedan tener rendimiento aprovisionado dedicado y otras puedan compartir el rendimiento. Para obtener más información, consulte la página [Establecimiento del rendimiento en un contenedor y una base de datos](set-throughput.md).
>

Primero debe decidir si quiere aprovisionar la base de datos o el rendimiento de nivel de colección, o una combinación de ambos. En general, se recomienda configurar un rendimiento dedicado en el nivel de colección. Aprovisionar el rendimiento en el nivel de base de datos permite que las colecciones en la base de datos compartan el rendimiento aprovisionado. Sin embargo, con un rendimiento compartido, no existen garantías que permitan conseguir un rendimiento específico en cada colección individual y no obtendrá un rendimiento predecible en ninguna colección específica.

Si no está seguro acerca de cuánto rendimiento se debe dedicar a cada colección individual, puede elegir el rendimiento de nivel de base de datos. Se puede considerar el rendimiento aprovisionado configurado en la base de datos de Azure Cosmos como un equivalente lógico de la capacidad de proceso de un servidor de máquina virtual de MongoDB o físico, pero más rentable gracias a la capacidad de escalar de forma elástica. Para obtener más información, consulte [Aprovisionamiento de rendimiento en contenedores y bases de datos de Azure Cosmos](set-throughput.md).

Si se aprovisiona el rendimiento en el nivel de base de datos, todas las colecciones creadas en esa base de datos deben crearse con una clave de partición. Para obtener más información sobre la creación de particiones, consulte [Creación de particiones y escalado horizontal en Azure Cosmos DB](partition-data.md). Si no se especifica una clave de partición durante la migración, Azure Database Migration Service rellena automáticamente el campo de clave de partición con un atributo *_id* generado automáticamente para cada documento.

### <a name="optimal-number-of-request-units-rus-to-provision"></a>Número óptimo de Unidades de solicitud (RU) para el aprovisionamiento

En Azure Cosmos DB, el rendimiento se aprovisiona de antemano y se mide en Unidades de solicitud (RU) por segundo. Si tiene cargas de trabajo que ejecutan MongoDB en una máquina virtual o en el entorno local, las Unidades de solicitud se consideran como una sencilla abstracción para los recursos físicos, como para el tamaño de una máquina virtual o en un servidor local y los recursos que poseen, por ejemplo, memoria, CPU o E/S por segundo. 

A diferencia de las máquinas virtuales o servidores locales, las Unidades de solicitud se pueden escalar y reducir verticalmente fácilmente en cualquier momento. Puede cambiar el número de Unidades de solicitud aprovisionadas en cuestión de segundos y solo se le cobrará por el número máximo de Unidades de solicitud que aprovisiona durante un período determinado de una hora. Para más información, consulte [Unidades de solicitud en Azure Cosmos DB](request-units.md).

Estos son factores clave que afectan al número de Unidades de solicitud necesarias:
- **Tamaño del elemento (por ejemplo, un documento)** : a medida que el tamaño de un elemento o documento aumenta, también lo hace el número de Unidades de solicitud consumidas para leer o escribir el elemento o documento.
- **Recuento de propiedades del elemento**: suponiendo que se aplica la [indexación predeterminada](index-overview.md) en todas las propiedades, el número de Unidades de solicitud consumidas para escribir un elemento aumenta a medida que lo hace el recuento de propiedades del elemento. Puede reducir el consumo de Unidades de solicitud para operaciones de escritura [limitando el número de las propiedades indexadas](index-policy.md).
- **Operaciones simultáneas**: Las Unidades de solicitud consumidas también dependen de la frecuencia con la que se ejecutan diferentes operaciones CRUD (como, por ejemplo, escrituras, lecturas, actualizaciones, eliminaciones) así como consultas más complejas. Puede usar [mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/) para generar las necesidades de simultaneidad de los datos de MongoDB actuales.
- **Patrones de consultas**: la complejidad de una consulta afecta a la cantidad de Unidades de solicitud que una consulta consume.

Si exporta los archivos JSON con [mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/) y comprende la cantidad de escrituras, lecturas, actualizaciones y eliminaciones por segundo que tienen lugar, puede usar el [planificador de capacidad de Azure Cosmos DB](https://www.documentdb.com/capacityplanner) para estimar el número inicial de Unidades de solicitud que se van a aprovisionar. El planificador de capacidad no tiene en cuenta el costo de consultas más complejas. Por lo tanto, si tiene consultas complejas en los datos, se consumirán Unidades de solicitud adicionales. La calculadora también asume que todos los campos se indexan y que se usa la coherencia de sesión. La mejor manera de comprender el costo de las consultas es migrar los datos (o datos de ejemplo) a Azure Cosmos DB, [conectarse al punto de conexión de Cosmos DB](connect-mongodb-account.md) y ejecutar una consulta de ejemplo desde el Shell de MongoDB mediante el comando `getLastRequestStastistics` para obtener el cargo de solicitud, lo que generará el número de Unidades de solicitud consumidas:

`db.runCommand({getLastRequestStatistics: 1})`

Este comando generará un documento JSON similar al siguiente:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Después de comprender el número de Unidades de solicitud que consume una consulta y las necesidades de simultaneidad para esa consulta, puede ajustar el número de Unidades de solicitud aprovisionadas. La optimización de las Unidades de solicitud no es un evento único: debe optimizar continuamente o escalar verticalmente las Unidades de solicitud aprovisionadas, en función de si no se espera un tráfico pesado, en lugar de una gran carga de trabajo o importación de datos.

## <a id="partitioning"></a>Elección de la clave de partición
La creación de particiones es un punto clave a tener en cuenta antes de migrar a una base de datos distribuida globalmente como Azure Cosmos DB. Azure Cosmos DB usa la creación de particiones con el fin de escalar contenedores individuales en una base de datos para satisfacer las necesidades de escalabilidad y rendimiento de la aplicación. En la creación de particiones, los elementos de un contenedor se dividen en distintos subconjuntos, que se llaman particiones lógicas. Para obtener más detalles y recomendaciones sobre cómo elegir la clave de partición correcta para los datos, consulte la sección [Elección de una clave de partición](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). 

## <a id="indexing"></a>Indexación de los datos
De forma predeterminada, Azure Cosmos DB indexa todos los campos de datos después de la ingesta. Puede modificar la [directiva de indexación](index-policy.md) en Azure Cosmos DB en cualquier momento. De hecho, a menudo se recomienda desactivar la indexación al migrar los datos y luego activarla cuando los datos ya están en Cosmos DB. Para obtener más detalles sobre la indexación, puede leer más información al respecto en la sección [Indexación en Azure Cosmos DB](index-overview.md). 

[Azure Database Migration Service](../dms/tutorial-mongodb-cosmos-db.md) migra automáticamente las colecciones de MongoDB con índices únicos. Sin embargo, los índices únicos deben crearse antes de la migración. Azure Cosmos DB no admite la creación de índices únicos cuando ya hay datos en las colecciones. Para más información, consulte [Claves únicas en Azure Cosmos DB](unique-keys.md).

## <a name="next-steps"></a>Pasos siguientes
* [Migración de los datos de MongoDB a Cosmos DB mediante Database Migration Service](../dms/tutorial-mongodb-cosmos-db.md) 
* [Aprovisionamiento del rendimiento en contenedores y bases de datos de Azure Cosmos](set-throughput.md)
* [Creación de particiones en Azure Cosmos DB](partition-data.md)
* [Distribución global en Azure Cosmos DB](distribute-data-globally.md)
* [Indexación en Azure Cosmos DB](index-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)
