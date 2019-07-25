---
title: Cuotas de servicio Azure Cosmos DB
description: Límites predeterminados y cuotas de servicio de Azure Cosmos DB en diferentes tipos de recursos.
author: arramac
ms.author: arramac
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 74df0038676e8459028084890da569ed3b75a682
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797284"
---
# <a name="azure-cosmos-db-service-quotas"></a>Cuotas de servicio de Azure Cosmos DB

En este artículo se proporciona una introducción a las cuotas predeterminadas que se ofrecen a distintos recursos en Azure Cosmos DB.

## <a name="storage-and-throughput"></a>Almacenamiento y rendimiento

Después de crear una cuenta de Azure Cosmos DB en su suscripción a Azure, puede administrar los datos en su cuenta mediante la [creación de bases de datos, contenedores y elementos](databases-containers-items.md). Puede aprovisionar el rendimiento en un nivel de contenedor o de base de datos en términos de [unidades de solicitud (RU/s o RU)](request-units.md). En la tabla siguiente se enumeran los límites de almacenamiento y rendimiento por contenedor y base de datos.

| Recurso | Límite predeterminado |
| --- | --- |
| Número máximo de RU por contenedor ([modo aprovisionado de rendimiento dedicado](databases-containers-items.md#azure-cosmos-containers)) | 1 000 000 de forma predeterminada. Para aumentarlo, [puede rellenar una incidencia de Soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través de [Preguntar a Cosmos DB](mailto:askcosmosdb@microsoft.com) |
| Número máximo de RU por base de datos ([modo aprovisionado de rendimiento dedicado](databases-containers-items.md#azure-cosmos-containers)) | 1 000 000 de forma predeterminada. Para aumentarlo, [puede rellenar una incidencia de Soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través de [Preguntar a Cosmos DB](mailto:askcosmosdb@microsoft.com) |
| Número máximo de RU por clave de partición (lógica) | 10 000 |
| Almacenamiento máximo en todos los elementos por clave de partición (lógica)| 10 GB |
| Número máximo de claves de partición (lógicas) distintas | Ilimitado |
| Almacenamiento máximo por contenedor | Ilimitado |
| Almacenamiento máximo por base de datos | Ilimitado |

> [!NOTE]
> Para que ver los procedimientos recomendados para administrar las cargas de trabajo que tienen claves de partición que requieren los límites superiores para el almacenamiento o el rendimiento, consulte [Diseño de claves de partición de acceso frecuente](synthetic-partition-keys.md).
>

Un contenedor de Cosmos (o una base de datos de rendimiento compartido) debe tener un rendimiento mínimo de 400 RU. A medida que crece el contenedor, el rendimiento mínimo admitido también depende de los factores siguientes:

* Rendimiento máximo que se ha aprovisionado en algún momento en el contenedor. El servicio admite la disminución del rendimiento de un contenedor en un 10 % del máximo aprovisionado. Por ejemplo, si el rendimiento aumentó a 10 000 RU, el rendimiento más bajo que se puede aprovisionar sería de 1000 RU.
* Número total de contenedores que ha creado alguna vez en una base de datos de rendimiento compartido, medido en 100 RU por contenedor. Por ejemplo, si ha creado cinco contenedores dentro de una base de datos de rendimiento compartido, el rendimiento debe ser de 500 RU como mínimo.

El rendimiento actual y mínimo de un contenedor o una base de datos se puede recuperar desde Azure Portal o los SDK. Para obtener más información, consulte [Aprovisionar rendimiento en contenedores y bases de datos](set-throughput.md). En resumen, los límites de RU de aprovisionamiento mínimos son los siguientes. 

| Recurso | Límite predeterminado |
| --- | --- |
| Número mínimo de RU por contenedor ([modo aprovisionado de rendimiento dedicado](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Número mínimo de RU por base de datos ([modo aprovisionado de rendimiento compartido](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Número mínimo de RU por contenedor dentro de una base de datos de rendimiento compartido | 100 |

Cosmos DB admite el escalado elástico de rendimiento (RU) por contenedor o base de datos mediante los SDK o el portal. Cada contenedor puede escalar de forma sincrónica e inmediata dentro de un intervalo de escala de 10 a 100 veces, entre los valores mínimos y máximo. Si el valor de rendimiento solicitado está fuera del intervalo, el escalado se realiza asincrónicamente. El escalado asincrónico puede tardar de algunos minutos a varias horas en completarse según el rendimiento solicitado y el tamaño del almacenamiento de datos en el contenedor.  

## <a name="control-plane-operations"></a>Operaciones del plano de control

Puede [aprovisionar y administrar su cuenta de Azure Cosmos](how-to-manage-database-account.md) mediante Azure Portal, Azure PowerShell, la CLI de Azure y plantillas de Azure Resource Manager. En la tabla siguiente se enumeran los límites por suscripción, cuenta y número de operaciones.

| Recurso | Límite predeterminado |
| --- | --- |
| Número máximo de cuentas de base de datos por suscripción | 50 de forma predeterminada. Para aumentarlo, [puede rellenar una incidencia de Soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través de [Preguntar a Cosmos DB](mailto:askcosmosdb@microsoft.com)|
| Número máximo de conmutaciones por error regionales | 1/hora de forma predeterminada. Para aumentarlo, [puede rellenar una incidencia de Soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través de [Preguntar a Cosmos DB](mailto:askcosmosdb@microsoft.com)|

> [!NOTE]
> Las conmutaciones por error regionales solo se aplican a las cuentas de escritura de una sola región. Las cuentas de escritura de varias regiones no requieren ni tienen ningún límite para el cambio de la región de escritura.

Cosmos DB crea automáticamente copias de seguridad de los datos a intervalos regulares. Para obtener más información sobre los intervalos y las ventanas de retención de copias de seguridad, consulte [Copias de seguridad en línea y restauración de datos a petición en Azure Cosmos DB](online-backup-and-restore.md).

## <a name="per-container-limits"></a>Límites de cada contenedor

En función de la API que utilice, un contenedor de Azure Cosmos puede representar una colección, una tabla o un grafo. Los contenedores admiten configuraciones para [las restricciones de clave única](unique-keys.md), [los procedimientos almacenados, los desencadenadores y las funciones definidas por el usuario (UDF)](stored-procedures-triggers-udfs.md) y [la directiva de indexación](how-to-manage-indexing-policy.md). En la tabla siguiente se enumeran los límites específicos de las configuraciones dentro de un contenedor. 

| Recurso | Límite predeterminado |
| --- | --- |
| Longitud máxima del nombre de la base de datos o el contenedor | 255 |
| Procedimientos almacenados máximos por contenedor | 100 <sup>*</sup>|
| Número máximo de UDF por contenedor | 25 <sup>*</sup>|
| Número máximo de rutas de acceso en la directiva de indexación| 100 <sup>*</sup>|
| Número máximo de claves únicas por contenedor|10 <sup>*</sup>|
| Número máximo de rutas de acceso por restricción de clave única|16 <sup>*</sup>|

<sup>*</sup> Para aumentar cualquiera de estos límites de los contenedores, puede ponerse en contacto con el Soporte técnico de Azure o con nosotros a través de [Ask Cosmos DB](mailto:askcosmosdb@microsoft.com) (Preguntar a Cosmos DB).

## <a name="per-item-limits"></a>Límites por elemento

En función de la API que use, un elemento de Azure Cosmos puede representar un documento en una colección, una fila en una tabla o un nodo o borde en un grafo. En la tabla siguiente, se muestran los límites por elemento de Cosmos DB. 

| Recurso | Límite predeterminado |
| --- | --- |
| Tamaño máximo de un elemento | 2 MB (longitud en UTF-8 de la representación JSON) |
| Longitud máxima del valor de la clave de partición | 2048 bytes |
| Longitud máxima del valor de id. | 1024 bytes |
| Número máximo de propiedades por elemento | Ningún límite práctico |
| Máxima profundidad de anidamiento | Ningún límite práctico |
| Longitud máxima del nombre de la propiedad | Ningún límite práctico |
| Longitud máxima del nombre de la propiedad | Ningún límite práctico |
| Longitud máxima del valor de propiedad de la cadena | Ningún límite práctico |
| Longitud máxima del valor de propiedad numérico | IEEE754 de doble precisión de 64 bits |

No hay ninguna restricción en las cargas de elementos, como el número de propiedades o la profundidad de anidamiento, excepto para las restricciones de longitud en los valores de identificador y de la clave de partición, y la restricción de tamaño general de 2 MB. Es posible que deba configurar la directiva de indexación para contenedores con estructuras de elementos grandes o complejas a fin de reducir el consumo de RU. Consulte [Modelar elementos en Cosmos DB](how-to-model-partition-example.md) para obtener un ejemplo real y los patrones para administrar elementos de gran tamaño.

## <a name="per-request-limits"></a>Límites por solicitud

Cosmos DB admite [operaciones CRUD y de consulta](https://docs.microsoft.com/rest/api/cosmos-db/) con recursos como contenedores, elementos y bases de datos.  

| Recurso | Límite predeterminado |
| --- | --- |
| Tiempo máximo de ejecución para una sola operación (por ejemplo, la ejecución de un procedimiento almacenado o la recuperación de una página de consulta única)| 5 segundos |
| Tamaño máximo de la solicitud (procedimiento almacenado, CRUD)| 2 MB |
| Tamaño máximo de respuesta (por ejemplo, consulta paginada) | 4 MB |

Una vez que una operación como una consulta alcanza el límite del tamaño de respuesta o del tiempo de espera de ejecución, esta devuelve una página de resultados y un token de continuación al cliente para reanudar la ejecución. No hay ningún límite práctico en la duración de la ejecución de una sola consulta en páginas o continuaciones.

Cosmos DB utiliza HMAC para la autorización. Puede usar una clave maestra o [tokens de recursos](secure-access-to-data.md) para tener un control de acceso específico a recursos como contenedores, claves de partición o elementos. En la tabla siguiente se enumeran los límites de los tokens de autorización de Cosmos DB.

| Recurso | Límite predeterminado |
| --- | --- |
| Tiempo de expiración máximo del token maestro | 15 minutos  |
| Tiempo de expiración mínimo del token maestro | 10 min  |
| Tiempo de expiración máximo del token de recursos | 24 horas de forma predeterminada. Para aumentarlo, [puede rellenar una incidencia de Soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través de [Preguntar a Cosmos DB](mailto:askcosmosdb@microsoft.com)|
| Distorsión máxima del reloj para la autorización del token| 15 minutos |

Cosmos DB admite la ejecución de desencadenadores durante las escrituras. El servicio admite un máximo de un desencadenador previo y un desencadenador posterior por operación de escritura. 

## <a name="sql-query-limits"></a>Límites de la consulta SQL

Cosmos DB admite la consulta de elementos mediante [SQL](how-to-sql-query.md). En la tabla siguiente se describen las restricciones en las instrucciones de consulta, por ejemplo, en relación con el número de cláusulas o la longitud de la consulta.

| Recurso | Límite predeterminado |
| --- | --- |
| Longitud máxima de la consulta SQL| 256 KB <sup>*</sup>|
| Número máximo de cláusulas JOIN por consulta| 5 <sup>*</sup>|
| Número máximo de operadores AND por consulta| 2000 <sup>*</sup>|
| Número máximo de operadores OR por consulta| 2000 <sup>*</sup>|
| Número máximo de UDF por consulta| 10 <sup>*</sup>|
| Número máximo de argumentos por expresión IN| 6000 <sup>*</sup>|
| Número máximo de puntos por polígono| 4096 <sup>*</sup>|

<sup>*</sup> Para aumentar cualquiera de estos límites de consulta SQL, puede ponerse en contacto con el Soporte técnico de Azure o con nosotros a través de [Ask Cosmos DB](mailto:askcosmosdb@microsoft.com) (Preguntar a Cosmos DB).

## <a name="mongodb-api-specific-limits"></a>Límites específicos de la API de MongoDB

Cosmos DB admite el protocolo de conexión de MongoDB para las aplicaciones escritas para MongoDB. Puede encontrar los comandos admitidos y las versiones del protocolo en [Sintaxis y características que admite MongoDB](mongodb-feature-support.md).

En la tabla siguiente se enumeran los límites específicos a la compatibilidad con características de MongoDB. Los otros límites de servicio que se mencionan con la API de SQL (básica) también se aplican a la API de MongoDB.

| Recurso | Límite predeterminado |
| --- | --- |
| Tamaño máximo de la memoria de consulta de MongoDB | 40 MB |
| Tiempo de ejecución máximo para las operaciones de MongoDB| 30 s |

## <a name="try-cosmos-db-free-limits"></a>Pruebe los límites gratuitos de Cosmos DB

En la tabla siguiente se enumeran los límites de la prueba de encontrará en [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/).

| Recurso | Límite predeterminado |
| --- | --- |
| Duración de la prueba | 30 días (se puede renovar cualquier número de veces) |
| Número máximo de contenedores por suscripción (API, Gremlin y Table API) | 1 |
| Número máximo de contenedores por suscripción (API de MongoDB) | 3 |
| Rendimiento máximo por contenedor | 5000 |
| Rendimiento máximo por base de datos de rendimiento compartido | 20000 |
| Almacenamiento total máximo por cuenta | 10 GB |

La Prueba de Cosmos DB admite la distribución global solo en las regiones de Centro de EE. UU., el Norte de Europa y el Sudeste Asiático. No se pueden crear incidencias de soporte técnico de Azure para las cuentas de prueba de Azure Cosmos DB. Sin embargo, se ofrece soporte técnico para aquellos suscriptores que cuenten con planes de soporte técnico existentes.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de los conceptos básicos de [distribución global](distribute-data-globally.md), [creación de particiones](partitioning-overview.md) y [rendimiento aprovisionado](request-units.md) de Cosmos DB.

Comience a usar Azure Cosmos DB con una de nuestras guías rápidas:

* [Introducción a SQL API de Azure Cosmos DB](create-sql-api-dotnet.md)
* [Introducción a la API de Azure Cosmos DB para MongoDB](create-mongodb-nodejs.md)
* [Introducción a la API Cassandra de Azure Cosmos DB](create-cassandra-dotnet.md)
* [Introducción a Gremlin API de Azure Cosmos DB](create-graph-dotnet.md)
* [Introducción a Table API de Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)
