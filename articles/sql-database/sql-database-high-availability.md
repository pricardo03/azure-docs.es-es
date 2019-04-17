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
ms.author: jovanpop
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: fd6e383c2631a47daa7bf469c5bec59959453252
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59616856"
---
# <a name="high-availability-and-azure-sql-database"></a>Alta disponibilidad y Azure SQL Database

Azure SQL Database es una plataforma de base de datos altamente disponible como servicio que garantiza el funcionamiento de la base de datos un 99,99 % del tiempo, sin tener que preocuparse por el mantenimiento ni por los tiempos de inactividad. Se trata de un proceso de Motor de base de datos de SQL Server completamente administrado hospedado en la nube de Azure que garantiza que la base de datos de SQL Server esté siempre actualizada y revisada sin que afecte a la carga de trabajo. Por lo general, cuando aplique revisiones a una instancia o cuando la conmute por error, no se notará el tiempo de inactividad si [usa la lógica de reintento](sql-database-develop-overview.md#resiliency) de su aplicación. Si el tiempo para completar una conmutación por error es mayor a 60 segundos, debe abrir una incidencia de soporte técnico. Azure SQL Database puede recuperarse rápidamente, incluso en las circunstancias más críticas, asegurando que los datos estén siempre disponibles.

La plataforma de Azure administra completamente cada instancia de Azure SQL Database y garantiza un alto porcentaje de disponibilidad de los datos sin pérdida de datos. Azure controla automáticamente la aplicación de revisiones, las copias de seguridad, la replicación, la detección de errores, los posibles errores de hardware, de software o de red subyacentes, la implementación de correcciones de errores, las conmutaciones por error, las actualizaciones de base de datos y otras tareas de mantenimiento. Los ingenieros de SQL Server han implementado los procedimientos más conocidos, lo que garantiza que todas las operaciones de mantenimiento finalizan en menos de un 0,01 % del tiempo del período de vida de la base de datos. Esta arquitectura se ha diseñado para garantizar que los datos confirmados no se pierdan nunca y que las operaciones de mantenimiento se realicen sin afectar a la carga de trabajo. No hay ventanas de mantenimiento ni tiempos de inactividad que puedan requerir que detenga la carga de trabajo mientras se actualiza o se mantiene la base de datos. La alta disponibilidad integrada de Azure SQL Database garantiza que la base de datos nunca sea el único punto de error de la arquitectura de software.

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Existen dos modelos de arquitectura de alta disponibilidad que se utilizan en Azure SQL Database (ambos garantizan una disponibilidad del 99,99%):

- Modelo de nivel de servicio estándar o general que se basa en la separación del proceso y el almacenamiento. Este modelo de arquitectura se basa en la alta disponibilidad y la confiabilidad de la capa de almacenamiento, pero puede tener una posible degradación del rendimiento durante las actividades de mantenimiento.
- Modelo de nivel de servicio crítico Premium o Business que se basa en un clúster de procesos del motor de base de datos. Este modelo de arquitectura se basa en el hecho de que siempre hay un cuórum de nodos de motor de base de datos disponibles, y tiene un impacto mínimo en el rendimiento de su carga de trabajo, incluso durante las actividades de mantenimiento.

Azure actualiza y revisa los sistemas operativos subyacentes, los controladores y el Motor de base de datos de SQL Server de forma transparente con el mínimo tiempo de inactividad para los usuarios finales. Azure SQL Database se ejecuta en la versión estable más reciente del Motor de base de datos de SQL Server y del sistema operativo Windows, y la mayoría de los usuarios no se dan cuenta de que las actualizaciones se realizan continuamente.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Disponibilidad de los niveles de servicio Básico, Estándar y Uso general

La disponibilidad estándar hace referencia a un Acuerdo de Nivel de Servicio (SLA) del 99,99 % que se aplica en los niveles de servicio Básico, Estándar y Uso general. La alta disponibilidad en este modelo de arquitectura se logra mediante la separación de las capas de proceso y almacenamiento, y la replicación de datos en la capa de almacenamiento.

En la siguiente imagen se muestran cuatro nodos en el modelo de arquitectura estándar con las capas separadas de proceso y almacenamiento.

![Separación de proceso y almacenamiento](media/sql-database-managed-instance/general-purpose-service-tier.png)

En el modelo de disponibilidad estándar hay dos capas:

- Una capa de proceso sin estado que ejecuta el proceso `sqlserver.exe` y que solo contiene datos en caché y transitorios (por ejemplo, caché de planes, grupo de búferes o grupo de almacenes de columnas). Este nodo sin estado de SQL Server lo opera Azure Service Fabric, que inicializa el proceso, controla el estado del nodo y realiza la conmutación por error en otro lugar si es necesario.
- Una capa de datos con estado con archivos de base de datos (.mdf o .ldf) que se almacenan en Azure Blob Storage. Azure Blob Storage garantiza que no se pierdan los datos de ningún registro que se encuentre en un archivo de base de datos. Almacenamiento de blobs de Azure tiene disponibilidad y redundancia de datos integrada que garantiza que cada registro en el archivo de registro o página en el archivo de datos se conservarán incluso si se bloquea el proceso de SQL Server.

Siempre que se actualice un sistema operativo o un motor de base de dato, si se produce un error en la infraestructura subyacente o, si se detecta algún problema crítico en el proceso de SQL Server, Azure Service Fabric moverá el proceso sin estado de SQL Server a otro nodo de proceso sin estado. Hay un conjunto de nodos adicionales que esperan ejecutar un nuevo servicio de proceso en caso de que se produzca una conmutación por error, para minimizar el tiempo de conmutación por error. Esto no afecta a los datos de Azure Blob Storage, y los archivos de registro o de datos se anexan al proceso de SQL Server inicializado recientemente. Este proceso garantiza una disponibilidad del 99,99 %, pero podría afectar al rendimiento de grandes cargas de trabajo que se ejecutan debido al tiempo de transición y al hecho de que el nuevo nodo de SQL Server se inicia con la caché inactiva.

## <a name="premium-and-business-critical-service-tier-availability"></a>Disponibilidad de los niveles de servicio Premium y Crítico para la empresa

La disponibilidad Premium está habilitada en los niveles Premium y Crítico para la empresa y se ha diseñado para cargas de trabajo intensivas que no pueden tolerar ningún impacto en el rendimiento debido a operaciones de mantenimiento continuado.

En el modelo Premium, Azure SQL Database integra el proceso y el almacenamiento en un único nodo. La alta disponibilidad en este modelo de arquitectura se logra mediante la replicación del proceso (proceso del motor de base de datos de SQL Server) y el almacenamiento (SSD conectado localmente) implementados en un clúster de 4 nodos, con una tecnología parecida a la de los [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server.

![Clúster de nodos del motor de base de datos](media/sql-database-managed-instance/business-critical-service-tier.png)

El proceso del motor de base de datos SQL y los archivos mdf o ldf subyacentes se colocan en el mismo nodo con el almacenamiento SSD conectado localmente, lo que proporciona baja latencia para la carga de trabajo. La alta disponibilidad se implementa mediante tecnología parecida a la de los [grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server. Cada base de datos es un clúster de nodos de base de datos con una base de datos principal, a la que se puede acceder para la carga de trabajo de cliente, y a tres procesos secundarios que contienen copias de los datos. El nodo principal inserta constantemente los cambios a los nodos secundarios para garantizar que los datos estén disponibles en réplicas secundarias si el nodo principal se bloquea por cualquier motivo. Azure Service Fabric controla la conmutación por error: una réplica secundaria se convierte en el nodo principal y se crea una nueva réplica secundaria para garantizar que haya suficientes nodos en el clúster. La carga de trabajo se redirige automáticamente al nuevo nodo principal.

Además, el clúster Crítico para la empresa incorpora la funcionalidad [Escalado horizontal de lectura](sql-database-read-scale-out.md) que proporciona un nodo de solo lectura integrado gratuito que se puede usar para ejecutar consultas de solo lectura (por ejemplo, informes) que no deberían afectar al rendimiento de la carga de trabajo principal.

## <a name="zone-redundant-configuration"></a>Configuración de redundancia de zona

De forma predeterminada, se crean las réplicas del conjunto de cuórum para las configuraciones de almacenamiento local en el mismo centro de datos. Con la incorporación de las [zonas de disponibilidad de Azure](../availability-zones/az-overview.md), podrá colocar las diferentes réplicas de los conjuntos de cuórum en zonas de disponibilidad diferentes de la misma región. Para eliminar un punto de error único, también se duplica el anillo de control en varias zonas como tres anillos de puerta de enlace. El enrutamiento a un anillo de puerta de enlace específico se controla mediante [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Como la configuración de redundancia de zona no crea una redundancia de base de datos adicional, el uso de Availability Zones en los niveles de servicio Premium y Crítico para la empresa está disponible sin ningún costo adicional. Si selecciona una base de datos con redundancia de zona, puede hacer que las bases de datos de los niveles Premium o Crítico para la empresa sean capaces de resistir a un número mucho mayor de errores, como a interrupciones catastróficas de los centros de datos, sin necesidad de cambiar la lógica de la aplicación. También puede aplicar la configuración de redundancia de zona a cualquier grupo o base de datos existente del nivel Premium o Crítico para la empresa.

Como el conjunto de cuórum con redundancia de zona tiene réplicas en distintos centros de datos situados a cierta distancia entre ellos, la mayor latencia de red puede aumentar el tiempo de confirmación y, por lo tanto, afectar al rendimiento de algunas cargas de trabajo OLTP. Siempre puede volver a la configuración de zona única; para ello, deshabilite la configuración de redundancia de zona. Este proceso es una operación de tamaño de datos y es similar a la actualización del nivel de servicio normal. Al final del proceso, la base de datos o el grupo se migra desde un anillo con redundancia de zona a un anillo de zona única, o viceversa.

> [!IMPORTANT]
> Las bases de datos con redundancia de zona y los grupos elásticos actualmente solo se admiten en el nivel de servicio Premium. De forma predeterminada, las copias de seguridad y los registros de auditoría se almacenan en un almacenamiento RA-GRS y, por lo tanto, podrían no estar disponibles automáticamente en el caso de una interrupción que afecte a toda la zona. 

En el diagrama siguiente se ilustra la versión con redundancia de zona de la arquitectura de alta disponibilidad:

![arquitectura de alta disponibilidad con redundancia de zona](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="accelerated-database-recovery-adr"></a>Recuperación de base de datos acelerada (ADR)

[Recuperación de base de datos acelerada (ADR)](sql-database-accelerated-database-recovery.md) es una nueva característica de motor de base de datos SQL que mejora considerablemente la disponibilidad de la base de datos, en especial en presencia de transacciones de ejecución prolongada, al rediseñar el proceso de recuperación del motor de base de datos SQL. ADR está actualmente disponible para bases de datos únicas, grupos elásticos y Azure SQL Data Warehouse.

## <a name="conclusion"></a>Conclusión

Azure SQL Database está totalmente integrado en la plataforma Azure y depende en gran medida de Service Fabric para la detección de errores y recuperación, de Azure Blob Storage para la protección de datos, y de Availability Zones para una mayor tolerancia a errores. Al mismo tiempo, Azure SQL Database aprovecha por completo la tecnología del grupo de disponibilidad Always On del producto de paquete de SQL Server para la replicación y la conmutación por error. La combinación de estas tecnologías permite que las aplicaciones obtengan todos los beneficios de un modelo de almacenamiento mixto y puedan admitir los SLA más exigentes.

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información acerca de las [zonas de disponibilidad de Azure](../availability-zones/az-overview.md).
- Obtener más información sobre [Service Fabric](../service-fabric/service-fabric-overview.md).
- Obtener más información acerca de [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Para conocer más opciones de alta disponibilidad y recuperación ante desastres, consulte [Continuidad del negocio](sql-database-business-continuity.md).
