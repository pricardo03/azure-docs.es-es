---
title: Mejora del rendimiento de aplicaciones de Azure con Azure Advisor | Microsoft Docs
description: Utilice Advisor para optimizar el rendimiento de las implementaciones de Azure.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: kasparks
ms.openlocfilehash: 8fdae1e12e56dcbcb56941726b0c089ad59b8fc8
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254652"
---
# <a name="improve-performance-of-azure-applications-with-azure-advisor"></a>Mejora del rendimiento de las aplicaciones de Azure con Azure Advisor

Las recomendaciones sobre rendimiento de Azure Advisor ayudan a mejorar la velocidad y la capacidad de respuesta de las aplicaciones empresariales críticas. Puede obtener las recomendaciones sobre rendimiento de Advisor en la pestaña **Rendimiento** del panel de Advisor.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Reducción del período de vida de DNS en el perfil de Traffic Manager para conmutar por error a puntos de conexión en buen estado con más rapidez

La [configuración del período de vida (TTL)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) del perfil de Traffic Manager permite especificar la rapidez con la que cambian los puntos de conexión si un punto de conexión determinado deja de responder a las consultas. La reducción de los valores de TTL suponen que se enrutará más rápido a los clientes a los puntos de conexión en funcionamiento.

Azure Advisor identifica los perfiles de Traffic Manager con un TTL más largo configurado y recomienda configurar el TTL en 20 segundos o 60 segundos, dependiendo de si el perfil está configurado para [Fast Failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>Mejora del rendimiento de la base de datos con SQL DB Advisor

Advisor proporciona una vista coherente y consolidada de recomendaciones para todos los recursos de Azure. Se integra con SQL Database Advisor para ofrecer recomendaciones y mejorar el rendimiento de la base de datos de SQL Azure. SQL Database Advisor evalúa el rendimiento de las bases de datos SQL Azure mediante el análisis del historial de utilización. Después, ofrece las recomendaciones más adecuadas para ejecutar la carga de trabajo habitual de la base de datos.

> [!NOTE]
> Para obtener recomendaciones, es preciso que una base de datos lleve usándose aproximadamente una semana y que, dentro de esa semana, muestre alguna actividad coherente. SQL Database Advisor puede optimizar los patrones de consultas coherentes con más facilidad que en el caso de ráfagas aleatorias de actividad.

Para más información acerca de SQL Database Advisor, consulte [SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-app-service-performance-and-reliability"></a>Mejora de la confiabilidad y el rendimiento de App Service

Azure Advisor integra las sugerencias de los procedimientos recomendados para mejorar su experiencia de App Services y para descubrir las funciones de la plataforma adecuada. Ejemplos de recomendaciones de App Services:
* Detección de instancias en las que los tiempos de ejecución de las aplicaciones con opciones de mitigación agotan los recursos de la memoria o la CPU.
* Detección de instancias donde la colocación de recursos como aplicaciones web y bases de datos puede mejorar el rendimiento y reducir el costo.

Para obtener más información acerca de las recomendaciones de App Services, consulte los [procedimientos recomendados para Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Uso de Managed Disks para evitar limitaciones de E/S del disco

Advisor identifica las máquinas virtuales que pertenecen a una cuenta de almacenamiento que está llegando a su objetivo de escalabilidad. Esta condición hace que las máquinas virtuales sean susceptibles a las limitaciones de E/S. Advisor recomendará que estas máquinas virtuales utilicen Managed Disks para evitar la degradación del rendimiento.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Mejora del rendimiento y la confiabilidad de los discos de máquina virtual mediante Premium Storage

Advisor identifica las máquinas virtuales con discos estándar que tienen un gran volumen de transacciones en la cuenta de almacenamiento y recomienda actualizar a discos premium. 

Azure Premium Storage ofrece compatibilidad con discos de alto rendimiento y latencia baja para máquinas virtuales que ejecutan cargas de trabajo intensivas de E/S. Los discos de máquinas virtuales que usan cuentas de Premium Storage almacenan datos en unidades de estado sólido (SSD). Para lograr un rendimiento óptimo en la aplicación, se recomienda migrar todos los discos de máquinas virtuales que requieran E/S por segundo elevados a Premium Storage.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Eliminación de la asimetría de datos en la tabla del almacenamiento de datos SQL para aumentar el rendimiento de las consultas

La asimetría de datos puede provocar movimientos de datos innecesarios o cuellos de botella en los recursos al ejecutar la carga de trabajo. Advisor detectará cualquier asimetría de datos de distribución mayor del 15 % y recomendará redistribuir los datos y revisar las selecciones principales de distribución de tablas. Para más información sobre la identificación y eliminación de asimetrías, consulte [Solución de problemas de asimetría](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Creación o actualización de las estadísticas de tabla obsoletas en la tabla del almacenamiento de datos SQL para aumentar el rendimiento de las consultas

Advisor identifica las tablas que no tienen [estadísticas de tabla](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) actualizadas y recomienda crear o actualizar las estadísticas de tabla. El optimizador de consultas de SQL Data Warehouse usa estadísticas actualizadas para estimar la cardinalidad o el número de filas del resultado de la consulta, lo que permite que el optimizador de consultas pueda crear un plan de consultas de alta calidad para proporcionar un rendimiento más rápido.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Escalado vertical para optimizar el uso de la memoria caché en las tablas de SQL Data Warehouse para mejorar el rendimiento de las consultas

Azure Advisor detecta si SQL Data Warehouse tiene un elevado porcentaje de memoria caché utilizada y un reducido porcentaje de aciertos. Esta condición indica una elevada expulsión de la memoria caché, lo que puede afectar al rendimiento de SQL Data Warehouse. Advisor le sugiere que escale verticalmente SQL Data Warehouse para asegurarse de que asigna suficiente capacidad de memoria caché a la carga de trabajo.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>Conversión de tablas de SQL Data Warehouse en tablas replicadas para aumentar el rendimiento de las consultas

Advisor identifica las tablas que no son tablas replicadas pero se beneficiarían de la conversión y le sugiere que convierta estas tablas. Las recomendaciones se basan en el tamaño de la tabla replicada, el número de columnas, el tipo de distribución de tabla y el número de particiones de la tabla de SQL Data Warehouse. Se puede proporcionar heurística adicional en la recomendación para el contexto. Para obtener más información acerca de cómo se determina esta recomendación, vea [Recomendaciones de SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Migración de la cuenta de almacenamiento a Azure Resource Manager para obtener todas las características recientes de Azure

Migre el modelo de implementación de la cuenta de almacenamiento a Azure Resource Manager (Resource Manager) para sacar provecho de las implementaciones de plantillas, de otras opciones de seguridad y de la posibilidad de actualizar a una cuenta de GPv2 para usar las características más recientes de Azure Storage. Advisor identificará las cuentas de almacenamiento independientes que utilizan el modelo de implementación clásico y recomendará migrar al modelo de implementación de Resource Manager.

> [!NOTE]
> Está previsto que las alertas clásicas de Azure Monitor se retiren en junio de 2019. Le recomendamos que actualice la cuenta de almacenamiento clásica y empiece a utilizar Resource Manager para conservar la funcionalidad de alertas con la nueva plataforma. Para más información, consulte la [retirada de alertas clásicas](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Diseñar las cuentas de almacenamiento para evitar llegar al límite máximo de suscripciones

Una región de Azure puede admitir un máximo de 250 cuentas de almacenamiento por suscripción. Una vez que se alcanza el límite, podrá crear las cuentas de almacenamiento más en esa combinación de región o suscripción. Advisor comprobará sus suscripciones y recomendaciones para diseñar menos las cuentas de almacenamiento para cualquier usuario que están a punto de alcanzar el límite máximo.

## <a name="optimize-the-performance-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers"></a>Optimizar el rendimiento de los servidores de Azure MySQL, PostgreSQL de Azure y Azure MariaDB 

### <a name="fix-the-cpu-pressure-of-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-with-cpu-bottlenecks"></a>Corregir la presión de CPU de los servidores de Azure MySQL, PostgreSQL de Azure y Azure MariaDB con cuellos de botella de CPU
Un uso muy elevado de la CPU durante un período prolongado puede provocar un rendimiento lento de las consultas para la carga de trabajo. Aumentar el tamaño de la CPU ayudará a optimizar el tiempo de ejecución de las consultas de base de datos y mejorar el rendimiento general. Azure Advisor identificará los servidores con un uso elevado de CPU que es probable que ejecutan cargas de trabajo de CPU limitada y recomienda escalar la capacidad de proceso.

### <a name="reduce-memory-constraints-on-your-azure-mysql-azure-postgresql-and-azure-mariadb-servers-or-move-to-a-memory-optimized-sku"></a>Reducir las restricciones de memoria en los servidores de Azure MySQL, PostgreSQL de Azure y Azure MariaDB o mover a una memoria optimizada de SKU
Puede dar lugar a una frecuencia de aciertos de caché baja en el rendimiento de las consultas más lento y una mayor IOPS. Esto podría ser debido a un plan de consulta incorrecta o ejecutar una carga de trabajo con uso intensivo de memoria. Corregir el plan de consulta o [aumentar la memoria](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers) de la base de datos de Azure para el servidor de base de datos de PostgreSQL, servidor de base de datos de Azure MySQL o MariaDB Azure server le ayudará a optimizar la ejecución de la carga de trabajo de la base de datos. Azure Advisor identifica los servidores afectados debido a esta renovación de grupo de búfer es elevada y recomienda la corrección del plan de consulta, se mueven a una SKU superior con más memoria, o aumentar el tamaño de almacenamiento para obtener más IOPS.

### <a name="use-a-azure-mysql-or-azure-postgresql-read-replica-to-scale-out-reads-for-read-intensive-workloads"></a>Use un Azure MySQL o una réplica de lectura de Azure PostgreSQL para escalar horizontalmente lecturas para cargas de trabajo intensivas de lecturas
Azure Advisor aprovecha la heurística basada en la carga de trabajo como la proporción de lecturas, escrituras en el servidor durante los últimos siete días para identificar las cargas de trabajo intensivas de lectura. La de Azure database for PostgreSQL recursos o de Azure database for MySQL recursos con una proporción de lecturas y escrituras muy alto puede dar lugar a contenciones de recursos de CPU y memoria conduce a ralentizar el rendimiento de la consulta. Agregar un [réplica](https://docs.microsoft.com/azure/postgresql/howto-read-replicas-portal) le ayudará a escalado horizontal de lecturas en el servidor de réplica, evitar las restricciones de recursos de CPU y memoria en el servidor principal. Advisor identificará los servidores con tales altas cargas de trabajo de lectura intensiva y recomienda agregar un [leer réplica](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) para descargar parte de las cargas de trabajo de lecturas.


### <a name="scale-your-azure-mysql-azure-postgresql-or-azure-mariadb-server-to-a-higher-sku-to-prevent-connection-constraints"></a>Escalar el servidor Azure MySQL, PostgreSQL de Azure o Azure MariaDB a una SKU superior para evitar las restricciones de conexión
Cada nueva conexión al servidor de base de datos ocupa la parte de la memoria. Degrada el rendimiento del servidor de base de datos si se producen errores en las conexiones al servidor porque un [límite](https://docs.microsoft.com/azure/postgresql/concepts-limits) en memoria. Azure Advisor identificará los servidores que ejecutan con muchos errores de conexión y recomendamos que actualice los límites de conexiones del servidor para proporcionar más memoria al servidor de escalado de proceso o el uso de SKU optimizadas de memoria, lo que tiene más proceso por núcleo.

## <a name="scale-your-cache-to-a-different-size-or-sku-to-improve-cache-and-application-performance"></a>Escalar la memoria caché a un tamaño diferente o SKU para mejorar la memoria caché y rendimiento de la aplicación

Las instancias de caché tienen un mejor rendimiento cuando no se está ejecutando bajo presión de memoria, carga elevada en el servidor o ancho de banda de red elevado lo cual puede provocar que deje de responder, sufrir una pérdida de datos o dejan de estar disponibles. Advisor identificará las instancias de caché en estas condiciones y recomienda aplicar las prácticas recomendadas para reducir la presión de memoria, la carga del servidor o el ancho de banda de red o el escalado a un tamaño diferente o SKU con más capacidad.

## <a name="add-regions-with-traffic-to-your-azure-cosmos-db-account"></a>Agregar regiones con el tráfico a su cuenta de Azure Cosmos DB

Advisor detectará las cuentas de Azure Cosmos DB que tienen tráfico desde una región que no está configurada actualmente y recomienda agregar esa región. Esto mejorará la latencia para las solicitudes procedentes de esa región y asegurará la disponibilidad en caso de interrupciones de la región. [Más información sobre la distribución de datos global con Azure Cosmos DB](https://aka.ms/cosmos/globaldistribution)

## <a name="configure-your-azure-cosmos-db-indexing-policy-with-customer-included-or-excluded-paths"></a>Configurar Azure Cosmos DB la directiva de indexación con el cliente a incluir o excluir rutas de acceso

Azure Advisor identificará los contenedores de Cosmos DB que usan la directiva de indexación predeterminada, pero podrían beneficiarse de una directiva de indexación personalizada basada en el patrón de carga de trabajo. El valor predeterminado de directiva de indexación indexa todas las propiedades, pero mediante una directiva de indexación personalizada con incluida o excluidas rutas de acceso explícitas utilizadas en los filtros de consulta puede reducir el RU y el almacenamiento usado para la indización. [Más información acerca de cómo modificar las directivas de índice](https://aka.ms/cosmosdb/modify-index-policy)

## <a name="configure-your-azure-cosmos-db-query-page-size-maxitemcount-to--1"></a>Configurar el tamaño de página de consulta (MaxItemCount) de Azure Cosmos DB en -1 

Azure Advisor identificará los contenedores de Azure Cosmos DB que usan el tamaño de página de consulta de 100 y se recomienda usar un tamaño de página de -1 para exámenes más rápidos. [Más información sobre el número máximo de elementos](https://aka.ms/cosmosdb/sql-api-query-metrics-max-item-count)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Obtención de acceso a las recomendaciones sobre rendimiento en Advisor

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

2.  En el panel de Advisor, haga clic en la pestaña **Rendimiento**.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más sobre las recomendaciones de Advisor, consulte:

* [Introducción a Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre alta disponibilidad de Advisor](advisor-high-availability-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
