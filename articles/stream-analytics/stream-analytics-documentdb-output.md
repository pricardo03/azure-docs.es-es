---
title: Salida de Azure Stream Analytics a Cosmos DB
description: En este artículo se describe cómo usar Azure Stream Analytics para guardar la salida en Azure Cosmos DB para la salida de JSON, para el archivado de datos y las consultas de latencia baja en datos de JSON no estructurados.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: 734cf09869e5a2df5f9a505a3cb8ccc7bc2338d5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60402308"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Salida de Azure Stream Analytics a Azure Cosmos DB  
Stream Analytics puede tener como destino [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) para la salida de JSON, habilitando el archivado de datos y las consultas de latencia baja en datos de JSON no estructurados. En este documento tratan algunas prácticas recomendadas para implementar esta configuración.

Aquellos que no estén familiarizados con Cosmos DB pueden comenzar por la [ruta de aprendizaje de Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/). 

> [!Note]
> En este momento, Azure Stream Analytics solo admite la conexión a Azure Cosmos DB mediante la **API de SQL**.
> Aún no se admiten otras API de Azure Cosmos DB. Si apunta Azure Stream Analytics a las cuentas de Azure Cosmos DB creadas con otras API, puede que los datos no se almacenen correctamente. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Aspectos básicos de Cosmos DB como destino de salida
La salida de Azure Cosmos DB de Stream Analytics de permite escribir los resultados del procesamiento de secuencias como salida de JSON en sus colecciones de Cosmos DB. Stream Analytics no crea colecciones en la base de datos; en su lugar, requiere que las cree por adelantado. Esto es para que controle los costos de facturación de las colecciones de Cosmos DB y para que pueda optimizar el rendimiento, la coherencia y la capacidad de las colecciones directamente mediante las [API de Cosmos DB](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> Debe agregar 0.0.0.0 a la lista de direcciones IP que permite el firewall de Azure Cosmos DB.

A continuación se detallan algunas de las opciones de la colección de Cosmos DB.

## <a name="tune-consistency-availability-and-latency"></a>Ajustar la coherencia, la disponibilidad y la latencia
Para satisfacer las necesidades de su aplicación, Azure Cosmos DB permite optimizar la base de datos y las colecciones, y buscar el equilibrio entre coherencia, disponibilidad y latencia. En función de los niveles de coherencia de lectura que requiera su situación en comparación con la latencia de lectura y escritura, puede elegir un nivel de coherencia u otro en la cuenta de base de datos. También de forma predeterminada, Azure Cosmos DB permite la indexación sincrónica en cada operación CRUD de la colección. Esta es otra opción útil para controlar el rendimiento de escritura o lectura en Azure Cosmos DB. Para obtener más información, revise el artículo [Niveles de coherencia de datos optimizables en Azure Cosmos DB](../cosmos-db/consistency-levels.md).

## <a name="upserts-from-stream-analytics"></a>Upserts de Stream Analytics
La integración de Stream Analytics en Azure Cosmos DB permite insertar o actualizar registros en su colección en función de una columna de identificador de documento determinada. Esto se conoce también como *Upsert*.

Stream Analytics utiliza un enfoque Upsert optimista, donde las actualizaciones solo se realizan cuando se produce un error en la inserción con un conflicto de identificador de documento. Con el nivel de compatibilidad 1.0, esta actualización se realiza como una operación PATCH, por lo que permite actualizaciones parciales en el documento; es decir, la adición de nuevas propiedades o la sustitución de una propiedad existente se realiza de forma incremental. No obstante, los cambios en los valores de las propiedades de la matriz en el documento JSON provocan que toda la matriz se sobrescriba; es decir, la matriz no se combina. Con el nivel 1.2, el comportamiento de upsert se modifica para insertar o reemplazar el documento. Esto se describe con más detalle en la sección sobre el nivel de compatibilidad 1.2, más adelante.

Si el documento JSON entrante tiene un campo de identificador existente, dicho campo se usa automáticamente como la columna de identificador de documento de Cosmos DB y todas las escrituras posteriores se controlan como tal, dando lugar a una de estas situaciones:
- identificadores únicos provocan una operación de inserción
- identificadores duplicados e "Identificador de documento" establecido en "ID" provocan una operación upsert
- identificadores duplicados e "Identificador de documento" no establecido provocan un error después del primer documento

Si desea guardar <i>todos</i> los documentos, incluidos aquellos con un identificador duplicado, cambie el nombre del campo de identificador de la consulta (con la palabra clave AS) y deje que Cosmos DB cree el campo de identificador o reemplace el identificador por el valor de otra columna (mediante la palabra clave AS o mediante uso de la configuración "Identificador de documento").

## <a name="data-partitioning-in-cosmos-db"></a>Creación de particiones de datos en Cosmos DB
Los contenedores [ilimitados](../cosmos-db/partition-data.md) de Azure Cosmos DB son la solución recomendada para crear particiones de datos, dado que Azure Cosmos DB escala automáticamente las particiones según la carga de trabajo. Al escribir en contenedores ilimitados, Stream Analytics usa tantos escritores paralelos como el esquema de partición de entrada o el paso de consulta anterior.
> [!Note]
> En este momento, Azure Stream Analytics solo admite colecciones ilimitadas con claves de partición en el nivel superior. Por ejemplo, se admite `/region`. No se admiten las claves de partición anidadas (por ejemplo, `/region/name`). 

Para colecciones de Azure Cosmos DB fijas, Stream Analytics no permite ningún escalado vertical u horizontal, una vez que están completas. Tienen un límite de 10 GB y un rendimiento de 10 000 RU/s.  Para migrar los datos de un contenedor fijo a un contenedor ilimitado (por ejemplo, uno con al menos 1000 RU/s y una clave de partición), debe usar la [herramienta de migración de datos](../cosmos-db/import-data.md) o la [biblioteca de fuente de cambios](../cosmos-db/change-feed.md).

La escritura en varios contenedores fijos está en desuso y no es el enfoque recomendado para escalar horizontalmente el trabajo de Stream Analytics. En el artículo [Partición y escalado en Cosmos DB](../cosmos-db/sql-api-partition-data.md), se ofrecen más detalles.

## <a name="improved-throughput-with-compatibility-level-12"></a>Rendimiento mejorado con el nivel de compatibilidad 1.2
En el nivel de compatibilidad 1.2, Stream Analytics admite la integración nativa para una escritura masiva en Cosmos DB. Esto permite que la escritura en Cosmos DB sea eficaz y se maximice el rendimiento, y consigue también un control eficaz de las solicitudes de limitación. El mecanismo de escritura mejorado está disponible en un nuevo nivel de compatibilidad, debido a una diferencia en el comportamiento de upsert.  En los niveles anteriores a 1.2, upsert inserta o combina el documento. En el nivel 1.2, el comportamiento de upsert se modifica para insertar o reemplazar el documento. 

En los niveles anteriores a 1.2, se utiliza un procedimiento almacenado para realizar el upsert masivo de los documentos por clave de partición en Cosmos DB, donde se escribe un lote como una transacción. Incluso si se produce un error transitorio (limitación) en un único registro, se debe reintentar todo el lote. Esto provocó que incluso escenarios con una limitación razonable fueran relativamente más lentos. En la siguiente comparación se muestra cómo se comportarían estos trabajos con el nivel 1.2.

La siguiente configuración muestra dos trabajos de Stream Analytics idénticos que leen de la misma entrada (centro de eventos). Ambos trabajos de Stream Analytics están [completamente particionados](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) con una consulta de paso y escriben en colecciones de CosmosDB idénticas. Las métricas de la izquierda corresponden al trabajo configurado con el nivel de compatibilidad 1.0 y las de la derecha corresponden al configurado con el nivel 1.2. La clave de partición de las colecciones de Cosmos DB es un GUID único que procede del evento de entrada.

![Comparación de las métricas de Stream Analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

La tasa de eventos entrantes en el centro de eventos es dos veces mayor de lo que las colecciones de Cosmos DB están configuradas para asumir (20 000 RU), por lo que se espera que se produzca una limitación en Cosmos DB. Sin embargo, el trabajo con el nivel 1.2 escribe de forma constante con un mayor rendimiento (eventos de salida/minuto) y con un menor porcentaje medio de utilización de SU. En su entorno, esta diferencia dependerá de algunos factores más, como la elección del formato de evento, el tamaño del evento o el mensaje de entrada, las claves de partición, la consulta, etc.

![Comparación de las métricas de Cosmos DB](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

Con el nivel 1.2, Stream Analytics es más inteligentes al utilizar el 100 % del rendimiento disponible en Cosmos DB, con muy pocos reenvíos por limitación. Esto proporciona una mejor experiencia para otras cargas de trabajo, como las consultas que se ejecutan en la colección al mismo tiempo. Si necesita probar cómo ASA se escala horizontalmente con Cosmos DB como un receptor de 1 a 10 000 mensajes/segundo, dispone de un [proyecto de ejemplos de Azure](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) que se lo permite.
Tenga en cuenta que el rendimiento de salida de Cosmos DB es idéntico con los niveles 1.0 y 1.1. Dado que el nivel 1.2 no es actualmente el nivel predeterminado, puede [establecer el nivel de compatibilidad](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) de un trabajo de Stream Analytics desde el portal o con la [llamada de API REST para crear un trabajo](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Se *recomienda encarecidamente* usar el nivel de compatibilidad 1.2 en ASA con Cosmos DB. 



## <a name="cosmos-db-settings-for-json-output"></a>Configuración de Cosmos DB para salida de JSON

La creación de Cosmos DB como una salida en Stream Analytics genera una solicitud de información, tal como se muestra a continuación. En esta sección se proporciona una explicación de la definición de propiedades.

![pantalla de salida de análisis de transmisiones de documentdb](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Campo           | DESCRIPCIÓN|
|-------------   | -------------|
|Alias de salida    | Un alias para hacer referencia a esta salida en la consulta de ASA.|
|Subscription    | Elija la suscripción de Azure.|
|Identificador de cuenta      | El nombre o el URI del punto de conexión de la cuenta de Azure Cosmos DB.|
|Clave de cuenta     | La clave de acceso compartido para la cuenta de Azure Cosmos DB.|
|Base de datos        | El nombre de la base de datos de Azure Cosmos DB.|
|Patrón de nombre de colección | El nombre de la colección que se usará. `MyCollection` es una entrada válida de ejemplo; debe existir una colección denominada `MyCollection`.  |
|Id. de documento     | Opcional. El nombre de la columna en los eventos de salida que se usa como clave única en la que deben basarse las operaciones de inserción o actualización. Si se deja vacío, se insertarán todos los eventos, sin opción de actualización.|
