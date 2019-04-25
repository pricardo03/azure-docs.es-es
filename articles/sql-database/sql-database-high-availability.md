---
title: 'Alta disponibilidad: servicio Azure SQL Database | Microsoft Docs'
description: Obtenga información acerca de las funcionalidades y características de alta disponibilidad de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: sashan
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 04/17/2019
ms.openlocfilehash: ec9f5aa8163ea9bb838b1a95ab8ad49233a72643
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392652"
---
# <a name="high-availability-and-azure-sql-database"></a>Alta disponibilidad y Azure SQL Database

El objetivo de la arquitectura de alta disponibilidad en Azure SQL Database es garantía de que la base de datos está en funcionamiento y ejecución del 99,99% de tiempo, sin preocuparse sobre el impacto de las operaciones de mantenimiento y las interrupciones. Azure controla automáticamente las tareas de mantenimiento críticas, como aplicación de revisiones, las copias de seguridad, las actualizaciones de Windows y SQL, así como los eventos no planeados, como errores de hardware, software o de red subyacentes.  Cuando la instancia SQL subyacente se aplican las revisiones o conmuta por error, el tiempo de inactividad no es apreciable si le [una lógica de reintento](sql-database-develop-overview.md#resiliency) en la aplicación. Azure SQL Database puede recuperarse rápidamente, incluso en las circunstancias más críticas, asegurando que los datos estén siempre disponibles.

La solución de alta disponibilidad está diseñada para garantizar que los datos confirmados nunca se pierde debido a errores, que las operaciones de mantenimiento no afectan a la carga de trabajo, y que la base de datos no será un único punto de error en la arquitectura del software. No hay ventanas de mantenimiento ni tiempos de inactividad que puedan requerir que detenga la carga de trabajo mientras se actualiza o se mantiene la base de datos. 

Hay dos modelos de arquitectura de alta disponibilidad que se usan en Azure SQL Database:

- Modelo de disponibilidad estándar que se basa en una separación de proceso y almacenamiento.  Depende de alta disponibilidad y confiabilidad de la capa de almacenamiento remoto. Esta arquitectura tiene como destino aplicaciones de negocio económicas que pueden tolerar alguna degradación del rendimiento durante las actividades de mantenimiento.
- Modelo de disponibilidad Premium que se basa en un clúster de los procesos del motor de base de datos. Se basa en el hecho de que siempre hay un quórum de nodos de motor de base de datos disponibles. Esta arquitectura tiene como destino aplicaciones de misión crítica con alto rendimiento de E/S, la tasa de transacciones alta y garantías mínimo impacto de rendimiento para la carga de trabajo durante las actividades de mantenimiento.

La base de datos de SQL Azure se ejecuta en la versión estable más reciente del motor de base de datos de SQL Server y el sistema operativo de Windows y la mayoría de los usuarios no observaría que las actualizaciones se realizan continuamente.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Disponibilidad de los niveles de servicio Básico, Estándar y Uso general

Estos niveles de servicio aprovechan la arquitectura de disponibilidad estándar. La siguiente ilustración muestra cuatro nodos diferentes con las capas de proceso y almacenamiento separados.

![Separación de proceso y almacenamiento](media/sql-database-high-availability/general-purpose-service-tier.png)

El modelo de disponibilidad estándar incluye dos capas:

- Una capa de proceso sin estado que se ejecuta el `sqlserver.exe` procesar y contiene sólo transitorios y almacenado en caché los datos en la SSD asociada, como grupo de almacenes de TempDB, base de datos modelo, la caché de planes, grupo de búferes y columna. Este nodo sin estado es operado por Azure Service Fabric que inicializa `sqlserver.exe`, controla el estado del nodo y realiza la conmutación por error a otro nodo si es necesario.
- Una capa de datos con estado con los archivos de base de datos (.mdf/.ldf) que se almacenan en Azure Blob storage. Almacenamiento de blobs de Azure tiene la característica de redundancia y disponibilidad de datos integrados. Garantiza que todos los registros en el archivo de registro o una página en el archivo de datos se conservarán incluso si se bloquea el proceso de SQL Server.

Cada vez que el motor de base de datos o el sistema operativo se actualiza o se detecta un error, Azure Service Fabric se moverá el proceso de SQL Server sin estado a otro nodo de proceso sin estado con suficiente capacidad libre. Datos en Azure Blob storage no se ve afectados por el movimiento y se adjuntan los archivos de datos o de registro para el proceso de SQL Server recién inicializado. Este proceso garantiza la disponibilidad del 99,99%, pero una gran carga de trabajo puede experimentar una degradación de rendimiento durante la transición ya que la nueva instancia de SQL Server se inicia con caché en frío.

## <a name="premium-and-business-critical-service-tier-availability"></a>Disponibilidad de los niveles de servicio Premium y Crítico para la empresa

Premium y crítico para la empresa aprovechar de los niveles de servicio el modelo de disponibilidad Premium, que se integra recursos de procesos (proceso del motor de base de datos de SQL Server) y almacenamiento (SSD conectado localmente) en un único nodo. Alta disponibilidad se logra mediante la replicación de almacenamiento y proceso para crear un clúster de tres a cuatro - nodo de nodos adicionales. 

![Clúster de nodos del motor de base de datos](media/sql-database-high-availability/business-critical-service-tier.png)

Los archivos de base de datos subyacente (.mdf/.ldf) se colocan en el almacenamiento SSD conectado para proporcionar una latencia muy baja E/S para la carga de trabajo. Alta disponibilidad se implementa mediante una tecnología similar a SQL Server [grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). El clúster incluye una única réplica principal (proceso de SQL Server) que es accesible para las cargas de trabajo de cliente de lectura y escritura y hasta tres réplicas secundarias (proceso y almacenamiento) que contiene copias de los datos. El nodo principal constantemente inserta los cambios en los nodos secundarios en orden y garantiza que los datos se sincronizan con al menos una réplica secundaria antes de confirmar cada transacción. Este proceso garantiza que si el nodo principal se bloquea por cualquier motivo, siempre hay un nodo conmute a totalmente sincronizado. Se inicia la conmutación por error mediante el Azure Service Fabric. Una vez que la réplica secundaria se convierte en el nuevo nodo principal, se crea otra réplica secundaria para asegurarse de que el clúster tiene suficientes nodos (conjunto de quórum). Una vez completada la conmutación por error, las conexiones de SQL se redirigen automáticamente al nuevo nodo principal.

Como ventaja adicional, el modelo de disponibilidad premium incluye la capacidad para redirigir las conexiones de SQL de solo lectura a una de las réplicas secundarias. Esta característica se denomina [escalado horizontal de lectura](sql-database-read-scale-out.md). Proporciona la capacidad sin costo adicional para descargar las operaciones de solo lectura, como cargas de trabajo de análisis de la réplica principal de proceso adicional al 100%.

## <a name="zone-redundant-configuration"></a>Configuración de redundancia de zona

De forma predeterminada, se crea el clúster de nodos para el modelo de disponibilidad premium en el mismo centro de datos. Con la introducción de [zonas de disponibilidad de Azure](../availability-zones/az-overview.md), base de datos SQL puede colocar las distintas réplicas del clúster para las zonas de disponibilidad diferentes en la misma región. Para eliminar un punto de error único, también se duplica el anillo de control en varias zonas como tres anillos de puerta de enlace. El enrutamiento a un anillo de puerta de enlace específico se controla mediante [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Dado que la configuración de redundancia de zona en los niveles de servicio Premium o crítico para la empresa no crea una redundancia adicional de la base de datos, puede habilitarla sin ningún costo adicional. Al seleccionar una configuración de redundancia de zona, puede hacer que las bases de datos Premium o crítico para la empresa resistentes a un conjunto mucho mayor de errores, incluidos las interrupciones de centro de datos catastrófica, sin realizar ningún cambio a la lógica de aplicación. También puede aplicar la configuración de redundancia de zona a cualquier grupo o base de datos existente del nivel Premium o Crítico para la empresa.

Dado que las bases de datos con redundancia de zona tienen réplicas en distintos centros de datos a cierta distancia entre ellos, la mayor latencia de red puede aumentar el tiempo de confirmación y, por tanto, afectar al rendimiento de algunas cargas de trabajo OLTP. Siempre puede volver a la configuración de zona única; para ello, deshabilite la configuración de redundancia de zona. Este proceso es una operación en línea similar a la actualización de nivel de servicio normal. Al final del proceso, la base de datos o el grupo se migra desde un anillo con redundancia de zona a un anillo de zona única, o viceversa.

> [!IMPORTANT]
> Las bases de datos con redundancia de zona y los grupos elásticos están actualmente solo se admite en los niveles de servicio Premium y crítico para la empresa. De forma predeterminada, las copias de seguridad y los registros de auditoría se almacenan en un almacenamiento RA-GRS y, por lo tanto, podrían no estar disponibles automáticamente en el caso de una interrupción que afecte a toda la zona. 

En el diagrama siguiente se ilustra la versión con redundancia de zona de la arquitectura de alta disponibilidad:

![arquitectura de alta disponibilidad con redundancia de zona](./media/sql-database-high-availability/zone-redundant-business-critical-service-tier.png)

## <a name="accelerated-database-recovery-adr"></a>Recuperación de base de datos acelerada (ADR)

[Acelerado de recuperación de base de datos (ADR)](sql-database-accelerated-database-recovery.md) es una nueva característica de motor de base de datos SQL que mejora considerablemente la disponibilidad de la base de datos, especialmente en presencia de larga ejecución transacciones. ADR está actualmente disponible para bases de datos únicas, grupos elásticos y Azure SQL Data Warehouse.

## <a name="conclusion"></a>Conclusión

La base de datos de SQL Azure ofrece una solución de alta disponibilidad integrada, que está totalmente integrada con la plataforma Azure. Es dependiente en Service Fabric para la recuperación y detección de errores, en el almacenamiento de blobs de Azure para la protección de datos y en las zonas de disponibilidad para mayor tolerancia a errores. Además, la base de datos SQL de Azure aprovecha la tecnología de grupo de disponibilidad AlwaysOn de SQL Server para la replicación y conmutación por error. La combinación de estas tecnologías permite a las aplicaciones se den cuenta de las ventajas de un almacenamiento mixto modelar y admiten los SLA más exigentes.

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información acerca de las [zonas de disponibilidad de Azure](../availability-zones/az-overview.md).
- Obtener más información sobre [Service Fabric](../service-fabric/service-fabric-overview.md).
- Obtener más información acerca de [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Para conocer más opciones de alta disponibilidad y recuperación ante desastres, consulte [Continuidad del negocio](sql-database-business-continuity.md).
