---
title: Obtención de métricas Azure Cosmos DB desde Azure Monitor
description: ''
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 2eb61a6b9afa3cabf1733be120dfbdacb7de4534
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276533"
---
# <a name="monitor-and-debug-azure-cosmos-db-metrics-from-azure-monitor"></a>Supervisión y depuración de métricas de Azure Cosmos DB desde Azure Monitor

Las métricas de Azure Cosmos DB desde Azure Monitor API. Azure Monitor proporciona varias formas de interactuar con las métricas, como Azure Portal, el acceso a ellas a través de la API REST o consultarlas con PowerShell o la CLI. Las métricas de Azure Cosmos DB son valores numéricos de baja latencia, que se recopilan cada minuto de forma predeterminada. Estas métricas también se pueden agregar. Dichas métricas pueden de admitir escenarios casi en tiempo real.  

En este artículo se describen distintas métricas de Azure Cosmos DB que se pueden ver desde Azure Monitor mediante Azure Portal. Si desea conocer no solo casos de uso comunes, sino también cómo se reutilizan las métricas de Azure Cosmos DB para analizar y depurar estos problemas, consulte el artículo[Supervisión y depuración con métricas de Azure Cosmos DB](use-metrics.md). Usará una de las cuentas existentes de Azure Cosmos existentes y verá las diferentes métricas en los niveles de base de datos, contenedor, región, solicitud u operación. Por consiguiente, asegúrese de que tiene una cuenta de Azure Cosmos con datos de ejemplo y realice las operaciones de CRUD en dichos datos.

## <a name="view-metrics-from-azure-portal"></a>Visualización de las métricas desde Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Seleccione **Monitor** en la barra de navegación izquierda y, a continuación, seleccione **Métricas**.

   ![Panel Métricas en Azure Monitor](./media/cosmos-db-azure-monitor-metrics/monitor-metrics-blade.png)

1. En el panel **Métricas** panel > **Seleccionar un recurso** > elija la **suscripción**, y el **grupo de recursos** requeridos. En **Tipo de recurso**, seleccione **Cuentas de Azure Cosmos DB**, elija una de las cuentas de Azure Cosmos existentes y seleccione **Aplicar**. 

   ![Elegir una cuenta de Cosmos DB para ver las métricas](./media/cosmos-db-azure-monitor-metrics/select-cosmosdb-account.png)

1. A continuación, puede seleccionar una métrica de la lista de métricas disponibles. Puede seleccionar métricas específicas de unidades de solicitud, almacenamiento, latencia, disponibilidad, Cassandra, etc. Para conocer los detalles de todas las métricas disponibles en esta lista, consulte la sección [Métricas por categoría](#metrics-by-category) en este mismo artículo. En este ejemplo, vamos a seleccionar **Unidades de solicitud** y **Prom** como valor de agregación. 

   Además de estos detalles, también puede seleccionar los valores de **Intervalo de tiempo** y **Granularidad de tiempo** de las métricas. Como máximo, puede ver las métricas de los últimos 30 días.  Después de aplicar el filtro, se muestra un gráfico basado en dicho filtro. Puede ver el número medio de unidades de solicitud consumidas por minuto durante el período seleccionado.  

   ![Elegir una métrica en Azure Portal](./media/cosmos-db-azure-monitor-metrics/metric-types.png)

## <a name="add-filters-to-metrics"></a>Incorporación de filtros a métricas

También puede filtrar las métricas y el gráfico que se muestra por una propiedad **CollectionName**, **DatabaseName**, **OperationType**, **Region** y **StatusCode** concreta. Para filtrar las métricas, seleccione **Agregar filtro** y elija la propiedad necesaria, como por ejemplo **OperationType** y seleccione un valor como **Query**. A continuación, el gráfico muestra las unidades de solicitud consumidas por la operación de consulta durante el período seleccionado. Las operaciones ejecutadas a través del procedimiento almacenado no se registran, por lo que no están disponibles en la métrica OperationType.

![Agregar un filtro para seleccionar la granularidad de la métrica](./media/cosmos-db-azure-monitor-metrics/add-metrics-filter.png)

Para agrupar las métricas puede usar la opción **Apply splitting** (Aplicar división). Por ejemplo, puede agrupar las unidades de solicitud por tipo de operación y ver el gráfico de todas las operaciones a la vez, como se muestra en la siguiente imagen: 

![Adición del filtro para aplicar división](./media/cosmos-db-azure-monitor-metrics/apply-metrics-splitting.png)


## <a id="metrics-by-category"></a>Métricas por categoría

### <a name="request-metrics"></a>Solicitud de métricas
            
|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación) |DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---| ---|
| TotalRequests (solicitudes totales) | Count (recuento) | Número de solicitudes realizadas| DatabaseName, CollectionName, Region, StatusCode| Todo | TotalRequests, Http 2xx, Http 3xx, Http 400, Http 401, error interno del servidor, servicio no disponible, solicitudes limitadas, promedio de solicitudes por segundo | Se usa para supervisar las solicitudes por código de estado, la colección en una granularidad por minuto. Para obtener el promedio de solicitudes por segundo, utilice la agregación Count en un minuto y divida por 60. |
| MetadataRequests (solicitudes de metadatos) |Count (recuento) | Recuento de las solicitudes de metadatos. Azure Cosmos DB mantiene una colección de metadatos del sistema para cada cuenta, lo que permite enumerar las colecciones, las bases de datos, etc., así como sus configuraciones de forma gratuita. | DatabaseName, CollectionName, Region, StatusCode| Todo| |Se usa para supervisar las limitaciones debido a las solicitudes de metadatos.|
| MongoRequests (solicitudes de Mongo) | Count (recuento) | Número de solicitudes de Mongo realizadas | DatabaseName, CollectionName, Region, CommandName, ErrorCode| Todo |Tasa de solicitudes de consultas de Mongo, tasa de solicitudes de actualización de Mongo, tasa de solicitudes de eliminación de Mongo, tasa de solicitudes de inserción de Mongo y tasa de solicitudes de recuento de Mongo| Se usa para supervisar los errores de solicitud de Mongo y los usos por de comando. |

### <a name="request-unit-metrics"></a>Métricas de unidad de solicitud

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (cargo de solicitud de Mongo) | Count (total) |Unidades de la solicitud de Mongo consumidas| DatabaseName, CollectionName, Region, CommandName, ErrorCode| Todo |Cargo de solicitudes de consultas de Mongo, cargo de solicitudes de actualización de Mongo, cargo de solicitudes de eliminación de Mongo, cargo de solicitudes de inserción de Mongo y cargo de solicitudes de recuento de Mongo| Se usa para supervisar las RU de recursos de Mongo en un minuto.|
| TotalRequestUnits (unidades de solicitud totales)| Count (total) | Unidades de solicitud consumidas| DatabaseName, CollectionName, Region, StatusCode |Todo| TotalRequestUnits| Se usa para supervisar el uso de RU total en una granularidad por minuto. Para obtener el promedio de RU consumidas por segundo, utilice la agregación Total en un minuto y divida por 60.|
| ProvisionedThroughput (rendimiento aprovisionado)| Count (máximo) |Rendimiento aprovisionado en granularidad de la colección| DatabaseName, CollectionName| 5M| | Se usa para supervisar el rendimiento aprovisionado por colección.|

### <a name="storage-metrics"></a>Métricas de almacenamiento

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (almacenamiento disponible) |Bytes (total) | Almacenamiento total disponible notificada a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M| Almacenamiento disponible| Se usa para supervisar la capacidad de almacenamiento disponible (aplicable solo para las colecciones de almacenamiento fijo). La granularidad mínima debe ser de cinco minutos.| 
| DataUsage (uso de datos) |Bytes (total) |Uso total de datos notificado a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M |Tamaño de los datos | Se utiliza para supervisar el uso total de los datos en la colección y la región, la granularidad mínima debe ser cinco minutos.|
| IndexUsage (uso de índices) | Bytes (total) |Uso total del índice notificado a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M| Tamaño de índice| Se utiliza para supervisar el uso total de los datos en la colección y la región, la granularidad mínima debe ser cinco minutos. |
| DocumentQuota (cuota de documento) | Bytes (total) | Cuota total de almacenamiento notificado a una granularidad de cinco minutos por región.| DatabaseName, CollectionName, Region| 5M |Capacidad de almacenamiento| Se utiliza para supervisar la cuota total en la colección y la región. La granularidad mínima debe ser cinco minutos.|
| DocumentCount (recuento de documentos) | Count (total) |Recuento total de documentos notificado a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M |Recuento de documentos|Se utiliza para supervisar el recuento de documentos en la colección y la región. La granularidad mínima debe ser cinco minutos.|

### <a name="latency-metrics"></a>Métricas de latencia

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Uso |
|---|---|---|---| ---| ---|
| ReplicationLatency (latencia de replicación)| Milliseconds (mínimo, máximo, promedio) | Latencia de replicación P99 entre regiones de origen y destino para la cuenta habilitada geográficamente| SourceRegion, TargetRegion| Todo | Se usa para supervisar la latencia de replicación P99 entre dos regiones cualesquiera para una cuenta replicada geográficamente. |


### <a name="availability-metrics"></a>Métricas de disponibilidad

|Métrica (nombre para mostrar de la métrica) |Unidad (tipo de agregación)|DESCRIPCIÓN| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---|
| ServiceAvailability (disponibilidad del servicio)| Percent (mínimo, máximo) | Solicitudes de cuenta en una granularidad por hora| 1H | Disponibilidad del servicio | Representa el porcentaje de solicitudes totales pasadas. Se considera que una solicitud ha dado error debido a un error del sistema si el código de estado es 410, 500 o 503. Se utiliza para supervisar la disponibilidad de la cuenta en una granularidad por hora. |


### <a name="cassandra-api-metrics"></a>Métricas de Cassandra API

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Uso |
|---|---|---|---| ---| ---|
| CassandraRequests (solicitudes de Cassandra) | Count (recuento) | Número de solicitudes de Cassandra API realizadas| DatabaseName, CollectionName, ErrorCode, Region, OperationType, ResourceType| Todo| Se usa para supervisar las solicitudes de Cassandra en una granularidad por minuto. Para obtener el promedio de solicitudes por segundo, utilice la agregación Count en un minuto y divida por 60.|
| CassandraRequestsCharges (cargos de solicitudes de Cassandra) | Count (suma, mínimo, máximo, promedio) | Unidades de solicitud consumidas por solicitudes de Cassandra API| DatabaseName, CollectionName, Region, OperationType, ResourceType| Todo| Se usa para supervisar las RU por minuto por una cuenta de Cassandra API.|
| CassandraConnectionClosures (cierres de conexión de Cassandra) |Count (recuento) |Número de conexiones de Cassandra cerradas| ClosureReason, Region| Todo | Se usa para supervisar la conectividad entre los clientes y Cassandra API de Azure Cosmos DB.|

## <a name="next-steps"></a>Pasos siguientes

* [Visualización y supervisión de las métricas desde el panel de métricas de la cuenta de Azure Cosmos DB](use-metrics.md)

* [Registro de diagnóstico de Azure Cosmos DB](logging.md)
