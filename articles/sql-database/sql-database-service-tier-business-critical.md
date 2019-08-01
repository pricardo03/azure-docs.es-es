---
title: 'Nivel de servicio Crítico para la empresa: Azure SQL Database | Microsoft Docs'
description: Obtenga información sobre el nivel de servicio Crítico para la empresa de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: 208224e10c3acfb17bc5fd89d2d66152943811dc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566753"
---
# <a name="business-critical-tier---azure-sql-database"></a>Nivel de servicio Crítico para la empresa: Azure SQL Database

> [!NOTE]
> El nivel Crítico para la empresa se denomina Premium en el modelo de compra basado en la unidad de transacción de base de datos. Para ver una comparación entre el modelo de compra basado en núcleo virtual y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-purchase-models.md).

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Hay tres modelos de arquitectura que se usan en Azure SQL Database:
- De uso general/Estándar 
- Crítico para la empresa/Premium
- Hiperescala

El modelo de nivel de servicio Crítico para la empresa/Premium se basa en un clúster de procesos del motor de base de datos. Este modelo de arquitectura se basa en el hecho de que siempre hay un cuórum de nodos de motor de base de datos disponibles, y tiene un impacto mínimo en el rendimiento de su carga de trabajo, incluso durante las actividades de mantenimiento.

Azure actualiza y revisa los sistemas operativos subyacentes, los controladores y el Motor de base de datos de SQL Server de forma transparente con el mínimo tiempo de inactividad para los usuarios finales. 

La disponibilidad Premium está habilitada en los niveles Premium y Crítico para la empresa, y se ha diseñado para cargas de trabajo intensivas que no pueden tolerar ningún impacto en el rendimiento debido a operaciones de mantenimiento continuado.

En el modelo Premium, Azure SQL Database integra el proceso y el almacenamiento en un único nodo. La alta disponibilidad en este modelo de arquitectura se logra mediante la replicación del proceso (proceso del motor de base de datos de SQL Server) y el almacenamiento (SSD conectado localmente) implementados en un clúster de cuatro nodos, con una tecnología parecida a la de los [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server.

![Clúster de nodos del motor de base de datos](media/sql-database-managed-instance/business-critical-service-tier.png)

El proceso del motor de base de datos SQL y los archivos mdf o ldf subyacentes se colocan en el mismo nodo con el almacenamiento SSD conectado localmente, lo que proporciona baja latencia para la carga de trabajo. La alta disponibilidad se implementa mediante tecnología parecida a la de los [grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server. Cada base de datos es un clúster de nodos de base de datos con una base de datos principal, a la que se puede acceder para la carga de trabajo de cliente, y a tres procesos secundarios que contienen copias de los datos. El nodo principal inserta constantemente los cambios a los nodos secundarios para garantizar que los datos estén disponibles en réplicas secundarias si el nodo principal se bloquea por cualquier motivo. El Motor de base de datos de SQL Server controla la conmutación por error: una réplica secundaria se convierte en el nodo principal y se crea una nueva réplica secundaria para garantizar que hay suficientes nodos en el clúster. La carga de trabajo se redirige automáticamente al nuevo nodo principal.

Además, el clúster Crítico para la empresa incorpora la funcionalidad [Escalado horizontal de lectura](sql-database-read-scale-out.md) que proporciona un nodo de solo lectura integrado gratuito que se puede usar para ejecutar consultas de solo lectura (por ejemplo, informes) que no deberían afectar al rendimiento de la carga de trabajo principal.

## <a name="when-to-choose-this-service-tier"></a>¿Cuándo elegir este nivel de servicio?

El nivel de servicio Crítico para la empresa está diseñado para las aplicaciones que requieren respuestas de baja latencia desde el almacenamiento SSD subyacente (1 o 2 ms como promedio), recuperación rápida si se produce un error en la infraestructura subyacente o necesidad de descargar informes, análisis y consultas de solo lectura en la versión gratuita de la réplica secundaria legible de la base de datos principal.

## <a name="next-steps"></a>Pasos siguientes

- Encontrar características de recursos (número de núcleos, E/S, memoria) del nivel crítico para la empresa en la [instancia administrada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), la base de datos única en el [modelo de núcleos virtuales](sql-database-vcore-resource-limits-single-databases.md#business-critical-service-tier-for-provisioned-compute-tier) o el [modelo DTU](sql-database-dtu-resource-limits-single-databases.md#premium-service-tier), o en el grupo elástico en el [modelo de núcleos virtuales](sql-database-vcore-resource-limits-elastic-pools.md#business-critical-service-tier-storage-sizes-and-compute-sizes) y el [modelo DTU](sql-database-dtu-resource-limits-elastic-pools.md#premium-elastic-pool-limits).
- Obtenga información sobre los niveles [De uso general](sql-database-service-tier-general-purpose.md) e [Hiperescala](sql-database-service-tier-hyperscale.md).
- Obtenga información sobre [Service Fabric](../service-fabric/service-fabric-overview.md).
- Para conocer más opciones de alta disponibilidad y recuperación ante desastres, consulte [Continuidad del negocio](sql-database-business-continuity.md).
