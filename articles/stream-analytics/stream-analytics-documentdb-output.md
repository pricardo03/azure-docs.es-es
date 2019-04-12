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
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495983"
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
Para satisfacer las necesidades de su aplicación, Azure Cosmos DB permite ajustar la base de datos y las colecciones y buscar el equilibrio entre coherencia, disponibilidad y latencia. En función de los niveles de coherencia de lectura que requiera su situación en comparación con la latencia de lectura y escritura, puede elegir un nivel de coherencia u otro en la cuenta de base de datos. También de forma predeterminada, Azure Cosmos DB permite la indexación sincrónica en cada operación CRUD de la colección. Esta es otra opción útil para controlar el rendimiento de escritura o lectura en Azure Cosmos DB. Para obtener más información, revise el artículo [Niveles de coherencia de datos optimizables en Azure Cosmos DB](../cosmos-db/consistency-levels.md).

## <a name="upserts-from-stream-analytics"></a>Upserts de Stream Analytics
La integración de Stream Analytics en Azure Cosmos DB permite insertar o actualizar registros en su colección en función de una columna de identificador de documento determinada. Esto se conoce también como *Upsert*.

Stream Analytics utiliza un enfoque Upsert optimista, donde las actualizaciones solo se realizan cuando se produce un error en la inserción con un conflicto de identificador de documento. Con compatibilidad Level 1.0, esta actualización se realiza como una revisión, por lo que habilita las actualizaciones parciales en el documento, es decir, la adición de nuevas propiedades o reemplazar que una propiedad existente se realiza de forma incremental. No obstante, los cambios en los valores de las propiedades de la matriz en el documento JSON provocan que toda la matriz se sobrescriba; es decir, la matriz no se combina. Con 1.2, se modifica el comportamiento de upsert para insertar o reemplazar el documento. Esto se describe más detalladamente en la sección 1.2 de nivel de compatibilidad siguiente.

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

## <a name="improved-throughput-with-compatibility-level-12"></a>Rendimiento mejorado con 1.2 de nivel de compatibilidad
Con el nivel de compatibilidad 1.2, integración nativa de Stream Analytics admite masiva de escritura en Cosmos DB. Esto permite escribir de manera eficaz para Cosmos DB con maximizar el rendimiento y eficaz de controlar las solicitudes de limitación. El mecanismo de escritura mejorado está disponible en un nuevo nivel de compatibilidad debido a una diferencia de comportamiento de upsert.  Antes de la 1.2, el comportamiento de upsert es insertar o combinar el documento. Con 1.2, realiza una operación Upsert comportamiento se modifica para insertar o reemplazar el documento. 

Antes de 1.2, utiliza un procedimiento almacenado personalizado a los documentos de forma masiva upsert por clave de partición en Cosmos DB, donde se escribe un lote como una transacción. Aunque un único registro produce un error transitorio (limitación), debe reintentar todo el lote. Esto hizo escenarios con limitación incluso razonable relativamente más lentamente. Comparación el siguiente muestra cómo se comportaría esos trabajos con 1.2.

A continuación el programa de instalación muestra dos trabajos de Stream Analytics idénticos leer desde la misma entrada (centro de eventos). Ambos trabajos de Stream Analytics son [completamente particionada](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) con una consulta de paso a través y escritura a las colecciones de COSMOS DB idénticas. Las métricas de la izquierda son desde el trabajo configurado con nivel de compatibilidad de 1.0 y los de la derecha se configura con 1.2. Clave de partición de las colecciones de COSMOS DB es un guid único que provienen del evento de entrada.

![comparación de las métricas de Stream analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Tasa de eventos entrantes en el centro de eventos es 2 x mayor que las colecciones de Cosmos DB (20K RU) están configuradas para la ingesta, por lo que se espera la limitación en Cosmos DB. Sin embargo, el trabajo con 1.2, constantemente se está escribiendo en un mayor rendimiento (eventos de salida/minuto) y con una menor SU % utilización promedio. En su entorno, esta diferencia dependerá pocos más factores como la elección del formato de evento, el tamaño de mensaje o evento de entrada, las claves de partición, consulta etcetera.

![cosmos db metrics comparison](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

Con 1.2, Stream Analytics es más inteligentes en el 100% de la capacidad de proceso disponible en Cosmos DB con muy pocos reenvíos de limitación/limitación de uso. Esto proporciona una mejor experiencia para otras cargas de trabajo, como las consultas que se ejecutan en la colección al mismo tiempo. En caso de que necesita probar cómo ASA se escala horizontalmente con Cosmos DB como un receptor de 1 a 10k mensajes/segundo, este es un [proyecto ejemplos de azure](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) que le permite hacer eso.
Tenga en cuenta que el rendimiento de la salida de Cosmos DB es idéntico con 1.0 y 1.1. Puesto que 1.2 no es actualmente el valor predeterminado, puede [establecer nivel de compatibilidad](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) para un trabajo de Stream Analytics mediante el portal o mediante el uso de la [crear llamada de API de REST de trabajo](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Tiene *recomienda* usar 1.2 de nivel de compatibilidad en ASA con Cosmos DB. 



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
|Patrón de nombre de colección | El nombre de la colección que se usará. `MyCollection` es una entrada válida de ejemplo: una colección denominada `MyCollection` debe existir.  |
|Id. de documento     | Opcional. El nombre de la columna en los eventos de salida que se usa como clave única en la que deben basarse las operaciones de inserción o actualización. Si se deja vacío, se insertarán todos los eventos, sin opción de actualización.|
