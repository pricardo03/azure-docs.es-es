---
title: Límites en Azure Cosmos DB
description: En este artículo se describe los límites de Azure Cosmos DB.
author: arramac
ms.author: arramac
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2019
ms.openlocfilehash: 0086327661df637dc0ae60208ed9424b4610ef0e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969498"
---
# <a name="limits-in-azure-cosmos-db"></a>Límites en Azure Cosmos DB

En este artículo se proporciona información general de los límites en el servicio de Azure Cosmos DB.

## <a name="storage-and-throughput"></a>Almacenamiento y rendimiento

Después de crear una cuenta de Azure Cosmos en su suscripción, puede administrar los datos en su cuenta por [crear bases de datos, los contenedores y los elementos](databases-containers-items.md). Puede aprovisionar el rendimiento en un nivel de contenedor o un nivel de base de datos en términos de [(RU/s o RUs) de unidades de solicitud](request-units.md). En la tabla siguiente se enumera los límites de almacenamiento y rendimiento por contenedor/base de datos.

| Recurso | Límite predeterminado |
| --- | --- |
| Unidades de solicitud máximos por contenedor ([modo aprovisionado rendimiento dedicado](databases-containers-items.md#azure-cosmos-containers)) | 1.000.000 de forma predeterminada. Se puede aumentar por [rellene una incidencia de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com) |
| Unidades de solicitud máximos por base de datos ([modo aprovisionado rendimiento compartido](databases-containers-items.md#azure-cosmos-containers)) | 1.000.000 de forma predeterminada. Se puede aumentar por [rellene una incidencia de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com) |
| Unidades de solicitud máximos por clave de partición (lógicos) | 10 000 |
| Almacenamiento máximo en todos los elementos por clave de partición (lógicos)| 10 GB |
| Número máximo de claves de partición (lógicos) distinta | Sin límite |
| Almacenamiento máximo por contenedor | Sin límite |
| Almacenamiento máximo por base de datos | Sin límite |

> [!NOTE]
> Para que ver recomendaciones administrar las cargas de trabajo que tienen claves de partición que necesitan aumentar los límites de almacenamiento o rendimiento, consulte [diseño para las claves de partición activa](synthetic-partition-keys.md)
>

Un contenedor de Cosmos (o base de datos de rendimiento compartido) debe tener un rendimiento mínimo de 400 RU. A medida que crece el contenedor, el rendimiento mínimo admitido depende también de los siguientes factores:

* El máximo almacenamiento consumido en el contenedor se mide en incrementos de 40 RU por GB de almacenamiento consumido. Por ejemplo, si un contenedor contiene 100 GB de datos, el rendimiento debe ser al menos de 4000 unidades de solicitud
* El rendimiento máximo que nunca se aprovisiona en el contenedor. El servicio admite disminuir el rendimiento de un contenedor en un 10% de los máximos aprovisionado. Por ejemplo, si el rendimiento aumentó a 10 000 RU, a continuación, el más bajo rendimiento aprovisionado posible sería 1000 RU
* El número total de contenedores que alguna vez ha creado en una base de datos compartido del rendimiento, medido en 100 RU por contenedor. Por ejemplo, si ha creado cinco contenedores dentro de una base de datos de rendimiento compartido, el rendimiento debe ser al menos 500 RU

El rendimiento actual y mínimo de un contenedor o una base de datos se puede recuperar desde el portal de Azure o el SDK. Para obtener más información, consulte [aprovisionamiento del rendimiento en contenedores y las bases de datos](set-throughput.md). En resumen, estos son los límites de RU aprovisionados mínimos. 

| Recurso | Límite predeterminado |
| --- | --- |
| RUs mínimos por contenedor ([modo aprovisionado rendimiento dedicado](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| RUs mínimas por base de datos ([modo aprovisionado rendimiento compartido](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| RUs mínimos por contenedor dentro de una base de datos de rendimiento compartido | 100 |
| RUs mínimos por cada GB de almacenamiento consumido | 40 |

COSMOS DB admite el escalado elástico de rendimiento (RU) por cada contenedor o base de datos mediante el SDK o el portal. Cada contenedor puede escalar de forma sincrónica e inmediatamente dentro de un intervalo de escala de 10 a 100 veces, entre los valores mínimos y máximo. Si el valor de rendimiento solicitada está fuera del intervalo, el escalado se realiza asincrónicamente. Escalado asincrónica puede tardar minutos a horas en completarse según el rendimiento solicitado y el tamaño de almacenamiento de datos en el contenedor.  

## <a name="control-plane-operations"></a>Operaciones del plano de control

También puede [aprovisionar y administrar su cuenta de Azure Cosmos](how-to-manage-database-account.md) mediante el portal de Azure, Azure PowerShell, CLI de Azure y plantillas de Azure Resource Manager. En la tabla siguiente se enumera los límites por suscripción, la cuenta y el número de operaciones.

| Recurso | Límite predeterminado |
| --- | --- |
| Cuentas de base de datos máximo por suscripción | 50 de forma predeterminada. Se puede aumentar por [rellene una incidencia de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com)|
| Número máximo de conmutaciones por error regionales | 1/hora de forma predeterminada. Se puede aumentar por [rellene una incidencia de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com)|

> [!NOTE]
> Conmutaciones por error regionales solo se aplican a las cuentas de operaciones de escritura de una sola región. Las cuentas de escritura en varias regiones no requieren ni tiene ningún límite sobre el cambio de la región de escritura.

COSMOS DB crea automáticamente copias de seguridad de los datos a intervalos regulares. Para obtener más información sobre los intervalos de retención de copia de seguridad y windows, consulte [restauración copias de seguridad en línea y los datos y a petición en Azure Cosmos DB](online-backup-and-restore.md).

## <a name="per-container-limits"></a>Límites de cada contenedor

Dependiendo de qué API que se utiliza, un contenedor de Azure Cosmos puede representar una colección, una tabla, o el grafo. Los contenedores admiten configuraciones para [restricciones de clave única](unique-keys.md), [procedimientos almacenados, desencadenadores y UDF](stored-procedures-triggers-udfs.md), y [directiva de indexación](how-to-manage-indexing-policy.md). En la tabla siguiente se enumera los límites específicos de las configuraciones dentro de un contenedor. 

| Recurso | Límite predeterminado |
| --- | --- |
| Longitud máxima del nombre de base de datos o un contenedor | 255 |
| Procedimientos almacenados máximos por contenedor | 100 <sup>*</sup>|
| UDF máximas por contenedor | 25 <sup>*</sup>|
| Número máximo de rutas de acceso en la directiva de indexación| 100 <sup>*</sup>|
| Número máximo de claves únicas por contenedor|10 <sup>*</sup>|
| Número máximo de rutas de acceso por la restricción de clave única|16 <sup>*</sup>|

<sup>*</sup> Puede aumentar cualquiera de estos límites de cada contenedor por ponerse en contacto con soporte técnico de Azure o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com).

## <a name="per-item-limits"></a>Límites por elemento

Dependiendo de qué API que se utiliza, un elemento de Azure Cosmos puede representan un documento en una colección, una fila en una tabla, o un nodo o perimetral en un gráfico. En la tabla siguiente se muestra los límites por cada elemento en Cosmos DB. 

| Recurso | Límite predeterminado |
| --- | --- |
| Tamaño máximo de un elemento | 2 MB (UTF-8 de longitud de la representación JSON) |
| Longitud máxima del valor de clave de partición | 2048 bytes |
| Longitud máxima del valor de Id. | 1024 bytes |
| Número máximo de propiedades por elemento | Ningún límite práctico |
| Profundidad de anidamiento máximo | Ningún límite práctico |
| Longitud máxima del nombre de propiedad | Ningún límite práctico |
| Longitud máxima del valor de propiedad | Ningún límite práctico |
| Longitud máxima del valor de propiedad de cadena | Ningún límite práctico |
| Longitud máxima del valor de propiedad numérica | Doble precisión IEEE754 64 bits |

No hay ninguna restricción en las cargas de elemento como el número de profundidad de anidamiento y propiedades, excepto para las restricciones de longitud de clave de partición y los valores de identificador y el general el tamaño de restricción de 2 MB. Es posible que deba configurar la directiva de indexación para contenedores con estructuras elemento grandes o complejos para reducir el consumo de RU. Consulte [modelado elementos en Cosmos DB](how-to-model-partition-example.md) para un ejemplo del mundo real y los patrones para administrar elementos de gran tamaño.

## <a name="per-request-limits"></a>Límites por solicitud

COSMOS DB admite [operaciones CRUD y de consulta](https://docs.microsoft.com/rest/api/cosmos-db/) frente a recursos como contenedores, los elementos y las bases de datos.  

| Recurso | Límite predeterminado |
| --- | --- |
| Tiempo de ejecución máximo para una única operación (por ejemplo, la ejecución de un procedimiento almacenado o una recuperación de la página de consulta única)| 5 segundos |
| Tamaño máximo de la solicitud (procedimiento almacenado, CRUD)| 2 MB |
| Tamaño máximo de respuesta (por ejemplo, la consulta paginada) | 4 MB |

Una vez que una operación como consulta alcanza el límite de tamaño de respuesta o tiempo de espera de ejecución, devuelve una página de resultados y un token de continuación al cliente para reanudar la ejecución. No hay ningún límite práctico en la duración de que una sola consulta puede ejecutarse en páginas/continuaciones.

COSMOS DB utiliza HMAC para la autorización. Puede usar una clave de patrón, o un [los tokens de recursos](secure-access-to-data.md) para control de acceso específico a recursos como contenedores, crear particiones en las claves o elementos. En la tabla siguiente se enumera los límites de los tokens de autorización de Cosmos DB.

| Recurso | Límite predeterminado |
| --- | --- |
| Tiempo máximo de expiración de tokens principal | 15 minutos  |
| Hora de expiración del token de recurso mínima | 10 min  |
| Tiempo de expiración del token de máximo de recursos | 24 horas de forma predeterminada. Se puede aumentar por [rellene una incidencia de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com)|
| Desplazamiento del reloj máximo para la autorización de token| 15 minutos |

COSMOS DB admite la ejecución de desencadenadores en escrituras. El servicio admite un máximo de un desencadenador previo y un desencadenador posterior por operación de escritura. 

## <a name="sql-query-limits"></a>Límites de la consulta SQL

COSMOS DB admite la consulta de elementos mediante [SQL](how-to-sql-query.md). En la tabla siguiente se describe las restricciones en las instrucciones de consulta, por ejemplo en cuanto al número de cláusulas o longitud de la consulta.

| Recurso | Límite predeterminado |
| --- | --- |
| Longitud máxima de la consulta SQL| 256 KB <sup>*</sup>|
| Combinaciones máximas por consulta| 5 <sup>*</sup>|
| Operadores and máximo por consulta| 2000 <sup>*</sup>|
| ORs máximos por consulta| 2000 <sup>*</sup>|
| UDF máximas por consulta| 10 <sup>*</sup>|
| Argumentos máximos por en expresión| 6000 <sup>*</sup>|
| Máximo de puntos de cada polígono| 4096 <sup>*</sup>|

<sup>*</sup> Puede aumentar cualquiera de estos límites de la consulta SQL al ponerse en contacto con soporte técnico de Azure o ponerse en contacto con nosotros a través [formular Cosmos DB](mailto:askcosmosdb@microsoft.com).

## <a name="mongodb-api-specific-limits"></a>Límites específicos de API de MongoDB

COSMOS DB admite el protocolo de conexión de MongoDB para las aplicaciones escritas para MongoDB. Puede encontrar los comandos admitidos y en las versiones del protocolo [sintaxis y las características de MongoDB admite](mongodb-feature-support.md).

En la tabla siguiente se enumera los límites específicos de compatibilidad de características de MongoDB. Otros límites de servicio que se mencionan con SQL API (principal) también se aplican a la API de MongoDB.

| Recurso | Límite predeterminado |
| --- | --- |
| Tamaño máximo de memoria de consulta de MongoDB | 40 MB |
| Tiempo de ejecución máximo para las operaciones de MongoDB| 30 s |

## <a name="try-cosmos-db-free-limits"></a>Pruebe los límites de licencia gratuita de Cosmos DB

En la tabla siguiente se enumera los límites para el [probar gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) prueba.

| Recurso | Límite predeterminado |
| --- | --- |
| Duración del período de prueba | 30 días (se puede renovar en cualquier número de veces) |
| Máximo de contenedores por suscripción (API de tabla SQL, Gremlin) | 1 |
| Máximo de contenedores por suscripción (API de MongoDB) | 3 |
| Rendimiento máximo por contenedor | 5000 |
| Rendimiento máximo por base de datos de rendimiento compartido | 20000 |
| Almacenamiento total máximo por cuenta | 10 GB |

Pruebe la distribución global de Cosmos DB admite en solo las regiones de EE. UU., Europa del Norte y sudeste asiático. No se puede crear incidencias de soporte técnico de Azure para las cuentas de prueba de Azure Cosmos DB. Sin embargo, se proporciona compatibilidad para los suscriptores con planes de soporte técnico existentes.

## <a name="next-steps"></a>Pasos siguientes

Conceptos básicos de más información acerca de Cosmos DB [distribución global](distribute-data-globally.md) y [particiones](partitioning-overview.md) y [rendimiento aprovisionado](request-units.md).

Comience a usar Azure Cosmos DB con una de nuestras guías rápidas:

* [Introducción a SQL API de Azure Cosmos DB](create-sql-api-dotnet.md)
* [Introducción a la API de Azure Cosmos DB para MongoDB](create-mongodb-nodejs.md)
* [Introducción a la API Cassandra de Azure Cosmos DB](create-cassandra-dotnet.md)
* [Introducción a Gremlin API de Azure Cosmos DB](create-graph-dotnet.md)
* [Introducción a Table API de Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)
