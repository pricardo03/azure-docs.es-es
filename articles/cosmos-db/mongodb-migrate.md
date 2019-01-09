---
title: Migración de los datos de MongoDB a Azure Cosmos DB mediante mongoimport y mongorestore
description: Aprenderá a usar mongoimport y mongorestore para importar datos a una API a Cosmos DB.
keywords: mongoimport, mongorestore
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: rimman
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB to Cosmos DB.
ms.openlocfilehash: 4cd30c7981cd6807113729292db403a80cbddef0
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793757"
---
# <a name="migrate-your-mongodb-data-to-azure-cosmos-db"></a>Migración de los datos de MongoDB a Azure Cosmos DB

 En este tutorial se proporcionan instrucciones sobre cómo migrar los datos almacenados en MongoDB a la cuenta de Azure Cosmos DB configurada para usar la API de Cosmos DB para MongoDB. Si va a importar datos desde MongoDB y planea usarlos con la API de SQL de Azure Cosmos DB, debe usar la [herramienta de migración de datos](import-data.md) para importarlos.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Preparar un plan de migración
> * Migrar datos mediante mongoimport
> * Migrar datos mediante mongorestore

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Revise y complete los siguientes requisitos previos antes de empezar la migración.

### <a name="plan-for-the-migration"></a>Planeación de la migración

En esta sección se describe cómo planear la migración de datos. Calcularemos las cargas de RU, determinaremos la latencia de la máquina en el servicio en la nube, y calcularemos el tamaño del lote y el número de trabajos de inserción.


#### <a name="pre-create-and-scale-your-collections"></a>Creación previa y escalado de las colecciones

Antes de empezar la migración mediante mongoimport o mongorestore, cree todas las colecciones en [Azure Portal](https://portal.azure.com) o mediante los controladores y herramientas de MongoDB. 

En [Azure Portal](https://portal.azure.com), aumente el rendimiento de las colecciones para la migración. Gracias al mayor rendimiento, puede evitar la limitación de velocidad y realizar la migración en menos tiempo. Para ahorrar costos, puede reducir el rendimiento inmediatamente después de la migración.

Además de aprovisionar el rendimiento en un nivel de colección, también puede aprovisionarlo en el nivel de base de datos para un conjunto de colecciones con el fin de compartir el rendimiento aprovisionado. Deberá crear previamente la base de datos y las colecciones, así como definir una clave de partición para cada colección en la base de datos de rendimiento compartido.

Puede crear colecciones con particiones con su herramienta, controlador o SDK favoritos. En este ejemplo, se usará el shell de Mongo para crear la colección:

```bash
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
El comando devuelve los siguientes resultados:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

#### <a name="calculate-the-approximate-ru-charge-for-a-single-document-write"></a>Cálculo de la carga de RU aproximada para una sola operación de escritura de documento

En el shell de MongoDB, conéctese a su cuenta de Cosmos configurada para usar la API de Cosmos DB para MongoDB. Encontrará instrucciones en [Conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md).

A continuación, ejecute un comando de inserción de ejemplo en uno de los documentos de ejemplo:
   
```bash
db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
```
        
Ejecute el comando `db.runCommand({getLastRequestStatistics: 1})`.

Recibirá una respuesta similar al siguiente resultado:
     
```bash
globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "insert",
    "RequestCharge": 10,
    "RequestDurationInMilliSeconds": NumberLong(50)
}
```
        
Tome nota de la carga de solicitudes.
    
#### <a name="determine-the-latency-from-your-machine-to-cosmos-db"></a>Determinación de la latencia de la máquina en Cosmos DB
    
Habilite el registro detallado desde el shell de MongoDB mediante el comando `setVerboseShell(true)`.
    
Ejecute una consulta básica en la base de datos con el comando `db.coll.find().limit(1)`.

Recibirá una respuesta similar al siguiente resultado:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Quite el documento insertado antes de ejecutar la migración para asegurarse de que no hay ningún documento duplicado. Puede quitar los documentos con el comando `db.coll.remove({})`.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>Cálculo de valores aproximados de las propiedades batchSize y numInsertionWorkers

Para la propiedad **batchSize**, divida el rendimiento total aprovisionado (RU/s) por las RU consumidas para una sola operación de escritura de documento, tal y como se completó en la sección "Determinación de la latencia de la máquina en Cosmos DB". Si el valor calculado es menor o igual a 24, use ese número como el valor de propiedad. Si el valor calculado es mayor que 24, establezca el valor de propiedad en 24.
    
Para el valor de la propiedad **numInsertionWorkers**, use esta ecuación:

`numInsertionWorkers = (Provisioned RUs throughput * Latency in seconds) / (batchSize * Consumed RUs for a single write)`

Podemos usar los siguientes valores con el fin de calcular un valor para la propiedad **numInsertionWorkers**:

| Propiedad | Valor |
|--------|-----|
| **batchSize** | 24 |
| RU aprovisionadas | 10 000 |
| Latencia | 0,100 s |
| RU consumidas | 10 RU |
| **numInsertionWorkers** | (10 000 RU x 0,100 s)/(24 x 10 RU) = **4,1666** |

Ejecute el comando de migración **monogoimport**. Las propiedades de los comandos se describen más adelante en este artículo.

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
```

También puede usar el comando **monogorestore**. Asegúrese de que todas las colecciones tienen un rendimiento establecido en una cantidad igual o superior al número de RU empleado en los cálculos anteriores.
   
```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
```

### <a name="complete-the-prerequisites"></a>Realización de los requisitos previos

Después de planear la migración, complete los pasos siguientes: 

* **Obtenga los datos de ejemplo**: asegúrese de que tiene algunos datos de ejemplo antes de iniciar la migración. 

* **Aumente el rendimiento:** la duración de la migración de datos depende de la cantidad de rendimiento aprovisionado para una sola colección o base de datos. Asegúrese de aumentar el rendimiento para migraciones de datos más grandes. Después de haber completado la migración, reduzca el rendimiento para ahorrar costos. 

* **Habilite SSL:**  Cosmos DB tiene estándares y requisitos de seguridad estrictos. No olvide habilitar SSL al interactuar con la cuenta de Cosmos. Los procedimientos de este artículo incluyen cómo habilitar SSL para los comandos mongoimport y mongorestore.

* **Recursos de Cosmos DB**: antes de comenzar la migración de datos, cree previamente todas las colecciones desde Azure Portal. Si realiza la migración a una cuenta de Cosmos que tiene un rendimiento de nivel de base de datos aprovisionado, proporcione una clave de partición al crear las colecciones.

* **Obtenga la cadena de conexión**: en [Azure Portal](https://portal.azure.com), en el panel izquierdo, seleccione la entrada **Azure Cosmos DB**. En **Suscripciones**, seleccione el nombre de la cuenta. En **Cadena de conexión**, seleccione **Cadena de conexión**. El lado derecho del portal muestra la información necesita para conectarse a su cuenta:

    ![Información de Cadena de conexión](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>Uso de mongoimport

Para importar datos en la cuenta de Cosmos, use la plantilla siguiente.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Reemplace los parámetros \<your_hostname>, \<your_username> y \<your_password> por los valores específicos de su cuenta. En el ejemplo siguiente, usamos **sampleDB** como el valor de \<your_database> y **sampleColl** como el valor de \<your_collection>:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>Uso de mongorestore

Para restaurar datos en la cuenta de Cosmos configurada con la API de Cosmos DB para MongoDB, utilice la siguiente plantilla para ejecutar la importación.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Reemplace los parámetros \<your_hostname>, \<your_username> y \<your_password> por los valores específicos de su cuenta. En el ejemplo siguiente, usamos **./dumps/dump-2016-12-07** como el valor de \<path_to_backup>:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos, podrá eliminar el grupo de recursos, la cuenta de Cosmos y todos los recursos relacionados. Siga estos pasos para eliminar el grupo de recursos:

1. Vaya al grupo de recursos donde creó la cuenta de Cosmos.
1. Seleccione **Eliminar grupo de recursos**.
1. Confirme el nombre del grupo de recursos que quiere eliminar y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente tutorial y obtenga más información sobre cómo consultar datos con la API de Azure Cosmos DB para MongoDB. 

> [!div class="nextstepaction"]
> [¿Cómo consultar datos de MongoDB?](../cosmos-db/tutorial-query-mongodb.md)
