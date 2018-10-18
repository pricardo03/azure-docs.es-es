---
title: Características y comandos de Apache Cassandra admitidos por Cassandra API de Azure Cosmos DB
description: Obtenga información sobre la compatibilidad con las características de Apache Cassandra en Cassandra API de Azure Cosmos DB
services: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: e3de78bdf38a326498b984dc2a9f8eaa42233d22
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091290"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Características de Apache Cassandra admitidas por Cassandra API de Azure Cosmos DB 

Azure Cosmos DB es un servicio de base de datos con varios modelos y de distribución global de Microsoft. Se puede comunicar con Cassandra API de Azure Cosmos DB con los [controladores](http://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) de cliente de Cassandra, de código abierto y compatibles con el [protocolo de conexión](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) del lenguaje de consulta Cassandra (CQL) v4. 

Con Cassandra API de Azure Cosmos DB, puede disfrutar de las ventajas de las API de Apache Cassandra, así como de las funcionalidades empresariales que ofrece Azure Cosmos DB. Entre las funcionalidades empresariales se incluyen las siguientes: [distribución global](distribute-data-globally.md), [creación de particiones de escalado horizontal automático](partition-data.md), garantías de disponibilidad y latencia, cifrado en reposo y copias de seguridad, entre otras.

## <a name="cassandra-protocol"></a>Protocolo Cassandra 

Cassandra API de Azure Cosmos DB es compatible con CQL versión **v4**. A continuación se enumeran los comandos, las herramientas, las limitaciones y las excepciones de CQL. Cualquier controlador de cliente que reconozca estos protocolos podrá conectarse a Cassandra API de Azure Cosmos DB.

## <a name="cassandra-driver"></a>Controlador Cassandra

Cassandra API de Azure Cosmos DB admite las siguientes versiones de los controladores Cassandra:

* [Java 3.5 y versiones posteriores](https://github.com/datastax/java-driver)  
* [C# 3.5 y versiones posteriores](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5 y versiones posteriores](https://github.com/datastax/nodejs-driver)  
* [Python 3.15 y versiones posteriores](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>Tipos de datos CQL 

Cassandra API de Azure Cosmos DB admite los siguientes tipos de datos CQL:

* ascii  
* bigint  
* blob  
* boolean  
* counter  
* fecha  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* text  
* Twitter en tiempo  
* timestamp  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* map  

## <a name="cql-functions"></a>Funciones de CQL

Cassandra API de Azure Cosmos DB admite las siguientes funciones de CQL:

* Se necesita el cifrado de tokens  
* Funciones de conversión de blobs 
  * typeAsBlob(value)  
  * blobAsType(value)
* Funciones UUID y timeuuid 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date)  


## <a name="cassandra-query-language-limits"></a>Límites del lenguaje de consulta Cassandra

Cassandra API de Azure Cosmos DB no tiene ningún límite en cuanto al tamaño de los datos almacenados en una tabla. Se pueden almacenar cientos de terabytes o petabytes de datos, con la seguridad de que se cumplirán los límites de la clave de partición. Del mismo modo, cada entidad o fila equivalente no tiene ningún límite en el número de columnas, aunque el tamaño total de la entidad no debe superar los 2 MB.

## <a name="tools"></a>Herramientas 

Cassandra API de Azure Cosmos DB es una plataforma de servicio administrada. No requiere ninguna administración adicional ni utilidades tales como el recolector de elementos no utilizados, Java Virtual Machine (JVM) o nodetool para administrar el clúster. Admite herramientas como cqlsh, que utiliza la compatibilidad con archivos binarios de CQLv4. 

* Para administrar la cuenta se admiten otros mecanismos tales como el explorador de datos de Azure Portal, las métricas, los diagnósticos de registro, PowerShell y la CLI.

## <a name="cql-shell"></a>Shell de CQL  

La utilidad de línea de comandos CQLSH viene con Apache Cassandra 3.1.1, con las siguientes variables de entorno habilitadas:

Antes de ejecutar los comandos siguientes, [agregue un certificado de raíz de Baltimore al almacén de certificados CA](https://docs.microsoft.com/java/azure/java-sdk-add-certificate-ca-store?view=azure-java-stable#to-add-a-root-certificate-to-the-cacerts-store). 

**Windows:** 

```bash
set SSL_VERSION=TLSv1_2 
SSL_CERTIFICATE=<path to Baltimore root ca cert>
set CQLSH_PORT=10350 
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> –ssl 
```
**Unix/Linux/Mac:**

```bash
export SSL_VERSION=TLSv1_2 
export SSL_CERTFILE=<path to Baltimore root ca cert>
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> –ssl 
```

## <a name="cql-commands"></a>Comandos de CQL

Azure Cosmos DB admite los siguientes comandos de base de datos en las cuentas de Cassandra API.

* CREATE KEYSPACE 
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH (solo se admiten los comandos no registrados) 
* DELETE

Cuando se ejecutan mediante SDK compatibles con CQLV4, todas las operaciones crud devolverán información adicional sobre el error, las unidades de solicitud consumidas o el identificador de actividad. A la hora de utilizar los comandos de eliminación y actualización se tendrá en cuenta el gobierno de recursos para evitar el uso de recursos aprovisionados. 
* Tenga en cuenta que el valor gc_grace_seconds debe ser cero si se especifica.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            string value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($“CustomPayload:  {key}: {value}”); 
        } 
```

## <a name="consistency-mapping"></a>Asignación de coherencia 

Cassandra API de Azure Cosmos DB ofrece opciones de coherencia para las operaciones de lectura. Todas las operaciones de escritura, con independencia de la coherencia de la cuenta, se escriben siempre con Acuerdos de Nivel de Servicio de rendimiento de escritura.

## <a name="permission-and-role-management"></a>Administración de permisos y roles

Azure Cosmos DB admite el control de acceso basado en roles (RBAC) y claves o contraseñas de solo lectura y de lectura y escritura, que pueden obtenerse en [Azure Portal] (https://portal.azure.com. Azure Cosmos DB no admite aún usuarios y roles para las actividades en el plano de datos. 

## <a name="planned-support"></a>Soporte planeado 
* Uso conjunto de marca de tiempo y TTL  
* El nombre de la región en el comando para crear un espacio de claves se ignora de momento. La distribución de datos se implementa en la plataforma de Cosmos DB subyacente y se expone a través del portal o de PowerShell para la cuenta. 





## <a name="next-steps"></a>Pasos siguientes

- Introducción a la [creación de una cuenta de Cassandra API, una base de datos y una tabla](create-cassandra-api-account-java.md) usando una aplicación de Java

