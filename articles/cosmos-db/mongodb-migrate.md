---
title: Usar mongoimport y mongorestore con la API de Azure Cosmos DB para MongoDB | Microsoft Docs
description: Obtenga información sobre cómo usar mongoimport y mongorestore para importar datos a una API para la cuenta de MongoDB
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 13422434e6392ec7681ec4478533c45a84f40c9a
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706983"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-mongodb-api-account"></a>Tutorial: Migración de los datos a una cuenta de la API de MongoDB para Azure Cosmos DB

En este tutorial se proporcionan instrucciones sobre cómo migrar los datos almacenados en MongoDB a la cuenta de la API de MongoDB para Azure Cosmos DB. Si va a importar datos desde MongoDB y planea usarlos con la API de Azure Cosmos DB, debe usar la [Herramienta de migración de datos](import-data.md) para importarlos.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Planear la migración
> * Requisitos previos para la migración
> * Migración de datos mediante mongoimport
> * Migración de datos mediante mongorestore

Antes de migrar datos a la cuenta de la API de MongoDB, asegúrese de que tiene algunos datos de ejemplo de MongoDB. Si no tiene una base de datos de MongoDB de ejemplo, puede descargar e instalar el [servidor de comunidad de MongoDB](https://www.mongodb.com/download-center), crear una base de datos de ejemplo y utilizar los archivos mongoimport.exe o mongorestore.exe para cargar datos de ejemplo. 

## <a name="plan-for-migration"></a>Planear la migración

1. Cree previamente las colecciones y escálelas:
        
   * De forma predeterminada, Azure Cosmos DB aprovisiona una nueva colección de MongoDB con 1000 unidades de solicitud por segundo (RU/s). Antes de empezar la migración mediante mongoimport o mongorestore, cree todas las colecciones en [Azure Portal](https://portal.azure.com) o mediante los controladores y herramientas de MongoDB. Si el tamaño de los datos supera los 10 GB, asegúrese de crear una [colección con particiones](partition-data.md) con una clave de partición adecuada. MongoDB recomienda almacenar los datos de entidad en colecciones. Puede ubicar conjuntamente las entidades de tamaño y rendimiento de aprovisionamiento comparables en el nivel de base de datos de Azure Cosmos.

   * En [Azure Portal](https://portal.azure.com), aumente el rendimiento de las colecciones de 1000 RU/s para una colección con una sola partición y 2500 RU/s para una colección con particiones simplemente durante la migración. Gracias al mayor rendimiento, puede evitar la limitación de velocidad y realizar la migración en menos tiempo. Para ahorrar costos, puede reducir el rendimiento inmediatamente después de la migración.

   * Además de aprovisionar las RU/s en el nivel de colección, también puede aprovisionar RU/s para un conjunto de colecciones en el nivel de base de datos primaria. Para ello es necesario crear previamente la base de datos y las colecciones, así como definir una clave de partición para cada colección.

   * Puede crear colecciones con particiones con su herramienta, controlador o SDK favoritos. En este ejemplo, se usará el shell de Mongo para crear la colección:

        ```bash
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        Resultados:

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. Calcule la carga de RU aproximada para una sola operación de escritura de documento:

    a. Conéctese a la cuenta de MongoDB API de Azure Cosmos DB desde el shell de MongoDB. Encontrará instrucciones en [Connect a MongoDB application to Azure Cosmos DB](connect-mongodb-account.md) (Conectar una aplicación de MongoDB a Azure Cosmos DB).
    
   b. Ejecute un comando de inserción de ejemplo en uno de los documentos de ejemplo desde el shell de MongoDB:
   
      ```bash
      db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
      ```
        
   c. Ejecute ```db.runCommand({getLastRequestStatistics: 1})``` y recibirá una respuesta similar a la siguiente:
     
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
        
    d. Tome nota de la carga de solicitudes.
    
1. Determine la latencia del equipo en el servicio en la nube de Azure Cosmos DB:
    
     a. Habilite el registro detallado desde el shell de MongoDB mediante este comando: ```setVerboseShell(true)```.
    
    b. Ejecute una consulta simple en la base de datos: ```db.coll.find().limit(1)```. Recibirá una respuesta similar a la siguiente:

       ```bash
       Fetched 1 record(s) in 100(ms)
       ```
        
1. Quite el documento insertado antes de la migración para asegurarse de que no hay ningún documento duplicado. Puede quitar los documentos con este comando: ```db.coll.remove({})```.

1. Calcular los valores aproximados de *batchSize* y *numInsertionWorkers*:

    * En el caso de *batchSize*, divida el total de RU aprovisionadas entre las RU consumidas en la operación de escritura de documento del paso 3.
    
    * Si el valor de *batchSize* calculado es <= 24, use ese número como valor de *batchSize*.
    
    * Si el valor de *batchSize* calculado es > 24, establezca *batchSize* en 24.
    
    * En el caso de *numInsertionWorkers*, use esta ecuación: *numInsertionWorkers = (rendimiento aprovisionado * latencia en segundos) / (tamaño del lote * RU usadas en una sola operación de escritura)*.
        
    |Propiedad|Valor|
    |--------|-----|
    |batchSize| 24 |
    |RU aprovisionadas | 10000 |
    |Latencia | 0,100 s |
    |RU cargadas en una sola operación de escritura de documento | 10 RU |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10 000 RU x 0,1 s) / (24 x 10 RU) = 4,1666*

1. Ejecute el comando de migración. Las opciones para migrar los datos se describen en las secciones siguientes.

   ```bash
   mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   O con mongorestore (asegúrese de que todas las colecciones tienen un rendimiento establecido en una cantidad igual o superior al número de RU empleado en los cálculos anteriores):
   
   ```bash
   mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="prerequisites-for-migration"></a>Requisitos previos para la migración

* **Aumente el rendimiento:** la duración de la migración de datos depende de la cantidad de rendimiento configurado para una sola colección o un conjunto de colecciones. Asegúrese de aumentar el rendimiento para migraciones de datos más grandes. Después de haber completado la migración, reduzca el rendimiento para ahorrar costos. Para más información sobre cómo aumentar el rendimiento en [Azure Portal](https://portal.azure.com), consulte [Niveles de rendimiento y planes de tarifa de Azure Cosmos DB](performance-levels.md).

* **Habilitar SSL:** Azure Cosmos DB tiene estándares y requisitos de seguridad estrictos. No olvide habilitar SSL al interactuar con la cuenta. Los procedimientos del resto del artículo incluyen cómo habilitar SSL para mongoimport y mongorestore.

* **Cree recursos de Azure Cosmos DB:** antes de comenzar la migración de datos, cree previamente todas las colecciones desde Azure Portal. Si va a migrar a una cuenta de Azure Cosmos DB que tiene un rendimiento de nivel de base de datos, asegúrese de proporcionar una clave de partición al crear las colecciones de Azure Cosmos DB.

## <a name="get-your-connection-string"></a>Obtención de la cadena de conexión 

1. En [Azure Portal](https://portal.azure.com), en el panel izquierdo, haga clic en la entrada **Azure Cosmos DB**.
1. En el panel **Suscripciones**, seleccione el nombre de cuenta.
1. En la hoja **Cadena de conexión**, haga clic en **Cadena de conexión**.

   El panel derecho contiene toda la información que necesita para conectarse correctamente a la cuenta.

   ![Hoja Cadena de conexión](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="migrate-data-by-using-mongoimport"></a>Migración de datos mediante mongoimport

Para importar datos a la cuenta de Azure Cosmos DB, use la plantilla siguiente. Rellene *host*, *nombre de usuario* y *contraseña* con los valores específicos de la cuenta.  

Plantilla:

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Ejemplo:  

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="migrate-data-by-using-mongorestore"></a>Migración de datos mediante mongorestore

Para restaurar datos en la cuenta de la API para MongoDB, utilice la siguiente plantilla para ejecutar la importación. Rellene *host*, *nombre de usuario* y *contraseña* con los valores específicos de la cuenta.

Plantilla:

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Ejemplo:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="next-steps"></a>Pasos siguientes

Puede pasar al tutorial siguiente y obtener más información sobre cómo consultar datos de MongoDB con Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[¿Cómo consultar datos de MongoDB?](../cosmos-db/tutorial-query-mongodb.md)
