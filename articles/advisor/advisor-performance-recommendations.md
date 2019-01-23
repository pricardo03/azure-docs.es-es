---
title: Recomendaciones sobre rendimiento de Azure Advisor | Microsoft Docs
description: Utilice Advisor para optimizar el rendimiento de las implementaciones de Azure.
services: advisor
documentationcenter: NA
author: kasparks
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: 3caf838fec3a5c0ab847ded85b269df7a66859e0
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266752"
---
# <a name="advisor-performance-recommendations"></a>Recomendaciones sobre rendimiento de Advisor

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

Advisor identifica las máquinas virtuales que pertenecen a una cuenta de almacenamiento que está llegando a su objetivo de escalabilidad. Esto las hace susceptibles de limitaciones de E/S. Advisor le recomendará que estas máquinas virtuales usen Managed Disks para evitar la degradación del rendimiento.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Mejora del rendimiento y la confiabilidad de los discos de máquina virtual mediante Premium Storage

Advisor identifica las máquinas virtuales con discos estándar que tienen un gran volumen de transacciones en la cuenta de almacenamiento y recomienda actualizar a discos premium. 

Azure Premium Storage ofrece compatibilidad con discos de alto rendimiento y latencia baja para máquinas virtuales que ejecutan cargas de trabajo intensivas de E/S. Los discos de máquinas virtuales que usan cuentas de Premium Storage almacenan datos en unidades de estado sólido (SSD). Para lograr un rendimiento óptimo en la aplicación, se recomienda migrar todos los discos de máquinas virtuales que requieran E/S por segundo elevados a Premium Storage.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Eliminación de la asimetría de datos en la tabla del almacenamiento de datos SQL para aumentar el rendimiento de las consultas

La asimetría de datos puede provocar movimientos de datos innecesarios o cuellos de botella en los recursos al ejecutar la carga de trabajo. Advisor detectará cualquier asimetría de datos de distribución mayor del 15 % y recomendará redistribuir los datos y revisar las selecciones principales de distribución de tablas. Para más información sobre la identificación y eliminación de asimetrías, consulte [Solución de problemas de asimetría](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Creación o actualización de las estadísticas de tabla obsoletas en la tabla del almacenamiento de datos SQL para aumentar el rendimiento de las consultas

Advisor identifica las tablas que no tienen [estadísticas de tabla](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) actualizadas y recomienda crear o actualizar las estadísticas de tabla. El optimizador de consultas del almacenamiento de datos SQL usa estadísticas actualizadas para estimar la cardinalidad o el número de filas en el resultado de la consulta, lo que permite que el optimizador de consultas cree un plan de consultas de alta calidad para un rendimiento más rápido.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Escalado vertical para optimizar el uso de la memoria caché en las tablas de SQL Data Warehouse para mejorar el rendimiento de las consultas

Azure Advisor detecta si SQL Data Warehouse tiene un elevado porcentaje de memoria caché utilizada y un reducido porcentaje de aciertos. Esto indica una elevada expulsión de memoria caché que puede afectar al rendimiento de SQL Data Warehouse. Advisor le sugiere que escale verticalmente SQL Data Warehouse para asegurarse de que asigna suficiente capacidad de memoria caché a la carga de trabajo.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>Conversión de tablas de SQL Data Warehouse en tablas replicadas para aumentar el rendimiento de las consultas

Advisor identifica las tablas que no son tablas replicadas pero se beneficiarían de la conversión y le sugiere que convierta estas tablas. Las recomendaciones se basan en el tamaño de la tabla replicada, el número de columnas, el tipo de distribución de tabla y el número de particiones de la tabla de SQL Data Warehouse. Se puede proporcionar heurística adicional en la recomendación para el contexto. Para obtener más información acerca de cómo se determina esta recomendación, vea [Recomendaciones de SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Migración de la cuenta de almacenamiento a Azure Resource Manager para obtener todas las características recientes de Azure

Migre su modelo de implementación de la cuenta de almacenamiento a Azure Resource Manager (ARM) para sacar provecho de las implementaciones de plantillas, de más opciones de seguridad y de la posibilidad de actualizar a una cuenta de GPv2 para usar las características más recientes de Azure Storage. Advisor identifica las cuentas de almacenamiento independientes que utilizan el modelo de implementación clásico y recomienda migrar al modelo de implementación de ARM.

> [!NOTE]
> Se ha anunciado que las alertas clásicas de Azure Monitor se retirarán en junio de 2019; se recomienda actualizar la cuenta de almacenamiento clásica a ARM para conservar la funcionalidad de alertas con la nueva plataforma. Para más información, consulte la [retirada de alertas clásicas](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

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
