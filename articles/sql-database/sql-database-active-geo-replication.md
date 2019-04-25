---
title: 'Replicación geográfica activa: Azure SQL Database | Microsoft Docs'
description: Use la replicación geográfica activa para crear bases de datos secundarias legibles de bases de datos individuales en el mismo centro de datos u otro diferente (región).
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/26/2019
ms.openlocfilehash: ca53f4bfa80d6fdead24dc7d562c2240bb3fa86d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387458"
---
# <a name="creating-and-using-active-geo-replication"></a>Creación y uso de replicación geográfica activa

La replicación geográfica activa es la característica de Azure SQL Database que permite crear bases de datos secundarias legibles de bases de datos individuales en un servidor de SQL Database en el mismo centro de datos u otro diferente (región).

> [!NOTE]
> La replicación geográfica activa no es compatible con Instancia administrada. Para la conmutación por error geográfica de las instancias administradas, use [Grupos de conmutación por error automática](sql-database-auto-failover-group.md).

La replicación geográfica activa se ha diseñado como solución de continuidad empresarial que permite que la aplicación realice una rápida recuperación ante desastres de bases de datos individuales en el caso de que se produzca un desastre regional o una interrupción a gran escala. Si la replicación geográfica está habilitada, la aplicación puede iniciar la conmutación por error en una base de datos secundaria de otra región de Azure. Se admiten hasta cuatro bases de datos secundarias en las mismas o en otras regiones, y las secundarias también se pueden usar para las consultas de acceso de solo lectura. La aplicación o el usuario deben iniciar manualmente la conmutación por error. Después de la conmutación por error, el nuevo elemento principal tiene un punto de conexión diferente. En el siguiente diagrama se ilustra una configuración típica de una aplicación de nube con redundancia geográfica mediante la replicación geográfica activa.

![replicación geográfica activa](./media/sql-database-active-geo-replication/geo-replication.png )

> [!IMPORTANT]
> SQL Database también admite grupos de conmutación por error automática. Para más información, consulte cómo se usan los [grupos de conmutación por error automática](sql-database-auto-failover-group.md). Además, la replicación geográfica activa no se admite para bases de datos creadas dentro de una instancia administrada. Considere la posibilidad de usar [grupos de conmutación por error](sql-database-auto-failover-group.md) con instancias administradas.

Si, por cualquier motivo, se produce un error en la base de datos principal o, simplemente, debe desconectarse, puede iniciar la conmutación por error a cualquiera de las bases de datos secundarias. Cuando se activa la conmutación por error a una de las bases de datos secundarias, las demás bases de datos secundarias se vinculan automáticamente a la nueva base de datos principal.

Con la replicación geográfica activa puede administrar la replicación y la conmutación por error de una base de datos individual o de un conjunto de bases de datos en un servidor o en un grupo elástico. Para ello, puede utilizar:

- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShell: base de datos única](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: grupo elástico](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [Transact-SQL: base de datos única o grupo elástico](/sql/t-sql/statements/alter-database-azure-sql-database)
- [API REST: base de datos única](https://docs.microsoft.com/rest/api/sql/replicationlinks)

Después de la conmutación por error, asegúrese de que los requisitos de autenticación para el servidor y la base de datos se configuran en el nuevo elemento principal. Para obtener más información, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).

La replicación geográfica activa aprovecha la tecnología [AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server para replicar de forma asincrónica las transacciones confirmadas en la base de datos principal a una base de datos secundaria mediante aislamiento instantáneo de instantáneas. Los grupos de conmutación por error automática proporcionan la semántica de grupo sobre la replicación geográfica activa, pero se usa el mismo mecanismo de replicación asincrónico. Mientras que, en cualquier momento dado, la base de datos secundaria puede ir ligeramente por detrás de la base de datos principal, se garantiza que los datos secundarios nunca tengan transacciones parciales. La redundancia entre regiones permite que las aplicaciones se recuperen rápidamente de la pérdida permanente de todo un centro de datos, o de partes de él, causada por desastres naturales, errores humanos catastróficos o actos malintencionados. Los datos específicos de RPO se encuentran en [Introducción a la continuidad empresarial](sql-database-business-continuity.md).

> [!NOTE]
> Si se produce un error de conexión entre dos regiones, intentamos volver a establecer las conexiones cada 10 segundos.
> [!IMPORTANT]
> Para garantizar que un cambio importante en la base de datos principal se replique en una secundaria antes de la conmutación por error, puede forzar la sincronización para garantizar la replicación de cambios importantes (por ejemplo, las actualizaciones de contraseñas). La sincronización forzada afecta al rendimiento al bloquear el subproceso de llamada hasta que todas las transacciones confirmadas se replican. Para más información, consulte [sp_wait_for_database_copy_sync](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync). Para supervisar el retraso de replicación entre la base de datos principal y la base de datos secundaria geográficamente, consulte [sys.dm_geo_replication_link_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).

En la siguiente ilustración, se muestra un ejemplo de replicación geográfica activa configurada con una principal en la región centro-norte de EE. UU. y una secundaria en la región centro-sur de EE. UU.

![Relación de replicación geográfica](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Debido a que las bases de datos secundarias son legibles, se pueden usar para descargar cargas de trabajo de solo lectura como trabajos de informes. Si usa la replicación geográfica activa, la base de datos secundaria se puede crear en la misma región que la base de datos primaria; sin embargo, esto no aumenta la resistencia de la aplicación ante errores catastróficos. Si usa grupos de conmutación por error automática, las bases de datos secundarias siempre se crean en otra región.

Además de para la recuperación ante desastres, la replicación geográfica activa se puede usar en los escenarios siguientes:

- **Migración de base de datos**: puede usar la replicación geográfica activa para migrar una base de datos de un servidor a otro en línea con un tiempo de inactividad mínimo.
- **Actualizaciones de aplicaciones**: puede crear una base de datos secundaria adicional como copia de conmutación por recuperación durante las actualizaciones de aplicaciones.

Para lograr una verdadera continuidad empresarial, agregar redundancia de base de datos entre centros de datos es solo parte de la solución. Para recuperar una aplicación (un servicio) de un extremo a otro tras un error catastrófico, es necesario recuperar todos los componentes que constituyen el servicio y cualquier servicio dependiente. Algunos ejemplos de estos componentes son el software cliente (por ejemplo, un explorador con JavaScript personalizado), los front-end web, el almacenamiento y DNS. Es fundamental que todos los componentes sean resistentes a los mismos errores y que estén disponibles en el plazo del objetivo de tiempo de recuperación (RTO) de la aplicación. Por lo tanto, debe identificar todos los servicios dependientes y comprender las garantías y capacidades que ofrecen. A continuación, debe seguir los pasos adecuados para asegurarse de que el servicio funcione durante la conmutación por error de los servicios de los que depende. Para más información sobre el diseño de soluciones para la recuperación ante desastres, consulte [Diseño de soluciones de nube para la continuidad empresarial mediante la replicación geográfica activa](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-terminology-and-capabilities"></a>Funcionalidades y terminología de la replicación geográfica activa

- **Replicación asincrónica automática**

  Solo se puede crear una base de datos secundaria mediante su adición a una existente. La base de datos secundaria se puede crear en cualquier servidor de Azure SQL Database. Una vez creada, la base de datos secundaria se rellena con los datos copiados de la base de datos principal. Este proceso se conoce como propagación. Después de crear y propagar la base de datos secundaria, las actualizaciones de la base de datos principal se replican asincrónicamente en la base de datos secundaria de forma automática. La replicación asincrónica significa que las transacciones se confirman en la base de datos principal antes de replicarse en la secundaria.

- **Bases de datos secundarias legibles**

  Una aplicación puede acceder a una base de datos secundaria para operaciones de solo lectura con las mismas entidades de seguridad que las usadas para tener acceso a la base de datos principal u otras diferentes. Las bases de datos secundarias funcionan en el modo de aislamiento de instantánea para asegurarse de que no se retrasa la replicación de las actualizaciones de la base de datos principal (reproducción de registro) por las consultas ejecutadas en la secundaria.

> [!NOTE]
> La reproducción de registros se retrasa en la base de datos secundaria si hay actualizaciones del esquema en el servidor principal. Lo último requiere un bloqueo del esquema en la base de datos secundaria.
> [!IMPORTANT]
> Puede usar replicación geográfica para crear una base de datos secundaria en la misma región que la réplica principal. Puede usar esta base de datos secundaria para las cargas de trabajo de equilibrio de carga de solo lectura en la misma región. Sin embargo, una base de datos secundaria en la misma región no proporciona resistencia adicional de errores y, por tanto, no es un destino de conmutación por error adecuado para la recuperación ante desastres. También no garantizará avaialability aislamiento de zona. Usar crítico para la empresa o nivel de servicio Premium con [configuración de redundancia de zona](sql-database-high-availability.md#zone-redundant-configuration) y así conseguir aislamiento de zona avaialability.   
>

- **Conmutación por error planeada**

  La conmutación por error planeada realiza una sincronización completa entre las bases de datos principales y secundarias antes de que el rol principal pase a la secundaria. De esta manera se garantiza que no hay pérdida de datos. La conmutación por error planeada se usa en los siguientes escenarios: (a) para realizar simulacros de recuperación ante desastres en producción cuando la pérdida de datos no es aceptable; (b) para reubicar la base de datos en una región diferente y (c) para devolver la base de datos a la región primaria después de que se ha mitigado la interrupción (conmutación por recuperación).

- **Conmutación por error no planeada**

  La conmutación por error no planeada o forzada cambia inmediatamente el rol principal a la base de datos secundaria sin sincronización con la principal. Esta operación dará lugar a la pérdida de datos. La conmutación por error no planeada se usa como método de recuperación durante las interrupciones cuando no es posible acceder a la base de datos principal. Cuando la base de datos principal original vuelve a estar en línea, se vuelve a conectar automáticamente sin sincronización y se convierte en una nueva base de datos secundaria.

- **Varias bases de datos secundarias**

  Pueden crearse hasta cuatro bases de datos secundarias por cada principal. Si solo hay una base de datos secundaria y se produce un error, la aplicación está expuesta a un mayor riesgo hasta que se crea una nueva base de datos secundaria. Si existen varias bases de datos secundarias, la aplicación sigue estando protegida incluso si se produce un error en una de ellas. También se pueden usar bases de datos secundarias adicionales para escalar horizontalmente las cargas de trabajo de solo lectura

  > [!NOTE]
  > Si usa la replicación geográfica activa para compilar una aplicación distribuida globalmente y necesita proporcionar acceso de solo lectura a los datos en más de cuatro regiones, puede crear una base de datos secundaria a partir de otra base de datos secundaria (este proceso se conoce como encadenamiento). De este modo, puede alcanzar una escala prácticamente ilimitada de replicación de la base de datos. Además, el encadenamiento disminuye la sobrecarga de la replicación desde la base de datos principal. El inconveniente es el mayor retraso de replicación en las últimas bases de datos secundarias.

- **Replicación geográfica de bases de datos en un grupo elástico**

  Cada base de datos secundaria puede participar por separado en un grupo elástico o no estar en ningún grupo elástico. La opción de grupo de cada base de datos secundaria es independiente y no depende de la configuración de ninguna otra base de datos secundaria (ya sea principal o secundaria). Cada grupo elástico se encuentra dentro de una única región; por lo tanto, varias bases de datos secundarias de la misma topología nunca pueden compartir un grupo elástico.

- **Tamaño de proceso configurable de la base de datos secundaria**

  Es necesario que tanto la base de datos principal como las secundarias tengan el mismo nivel de servicio. También se recomienda que la base de datos secundaria se cree con el mismo tamaño de proceso (unidades de transacción de base de datos o núcleos virtuales) que la principal. Una base de datos secundaria con un tamaño de proceso inferior corre el riesgo de sufrir mayor retraso en la replicación o posible no disponibilidad y, por consiguiente, corre el riesgo de pérdida de datos considerable después de una conmutación por error. Como resultado, el RPO publicado de 5 s no se puede garantizar. El otro riesgo es que después de la conmutación por error, el rendimiento de la aplicación se verá afectado debido a una falta de capacidad de proceso de la nueva base de datos principal hasta que se actualice a un tamaño de proceso superior. El tiempo de la actualización depende del tamaño de la base de datos. Además, actualmente, dicha actualización requiere que tanto la base de datos principal como la secundaria estén en línea y, por lo tanto, no se puede completar hasta que se reduzca la interrupción. Si decide crear la base de datos secundaria con un tamaño de proceso más bajo, el gráfico de porcentaje de E/S de registro en Azure Portal proporciona un buen método para estimar el tamaño de proceso mínimo de la base de datos secundaria que es necesario para mantener la carga de replicación. Por ejemplo, si la base de datos principal es P6 (1000 DTU) y su porcentaje de E/S de registro es 50 %, la base de datos secundaria debe ser al menos P4 (500 DTU). También puede recuperar los datos de E/S de registro mediante las vistas de base de datos [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) o [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database).  Para más información sobre los tamaños de proceso de SQL Database, consulte [¿Qué son los niveles de servicio de SQL Database?](sql-database-purchase-models.md)

- **Conmutación por error y conmutación por recuperación controladas por el usuario**

  La aplicación o el usuario puede cambiar explícitamente una base de datos secundaria al rol principal en cualquier momento. Durante una interrupción real, debe utilizarse la opción "no planeada", que promueve inmediatamente una base de datos secundaria a principal. Cuando la base de datos principal que generó el error se recupera y vuelve a estar disponible, el sistema la marca automáticamente como secundaria y la pone al día con la nueva base de datos principal. Por la naturaleza asincrónica de la replicación, se puede perder una pequeña cantidad de datos durante las conmutaciones por error no planeadas antes de que se repliquen los cambios más recientes a la base de datos secundaria. Cuando una base de datos principal con varias secundarias conmuta por error, el sistema vuelve a configurar automáticamente las relaciones de replicación y vincula las bases de datos secundarias restantes a la principal recién promovida sin necesidad de que intervenga el usuario. Una vez que se mitiga la interrupción que causó la conmutación por error, sería conveniente devolver la aplicación a la región primaria. Para hacer esto, se debe invocar el comando de conmutación por error con la opción "planeada".

- **Mantenimiento de las credenciales y las reglas de firewall sincronizadas**

Se recomienda usar [reglas de firewall IP de nivel de base de datos](sql-database-firewall-configure.md) con replicación geográfica las bases de datos para estas reglas se pueden replicar con la base de datos para asegurarse de que todas las bases de datos secundarias tienen las mismas reglas de firewall IP que la réplica principal. Este enfoque elimina la necesidad de que los clientes configuren y mantengan manualmente las reglas de firewall en los servidores que hospedan tanto la base de datos principal como las secundarias. Igualmente, la utilización de [usuarios de base de datos independiente](sql-database-manage-logins.md) para el acceso a los datos garantiza que la base de datos principal y las secundarias tengan siempre las mismas credenciales de usuario. Por tanto, durante una conmutación por error, no hay interrupciones debidas a discrepancias en los inicios de sesión y las contraseñas. Con la adición de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), los clientes pueden administrar el acceso de usuarios a la base de datos principal y a las secundarias, por lo que ya no es necesario administrar credenciales en las bases de datos.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Actualización o degradación de una base de datos principal

Puede actualizar o degradar una base de datos principal a un tamaño de proceso diferente (en el mismo nivel de servicio, no entre De uso general y Crítico para la empresa) sin desconectar las bases de datos secundarias. Al actualizar, se recomienda que actualice la base de datos secundaria en primer lugar y, a continuación, la principal. Al degradar, invierta el orden: degrade primero la base de datos principal y, después, la secundaria. Cuando actualiza la base de datos a un nivel de servicio diferente, o la cambia a una versión anterior, se aplica esta recomendación.

> [!NOTE]
> Si creó una base de datos secundaria como parte de la configuración del grupo de conmutación por error, no se recomienda que la degrade. De este modo, se garantiza que la capa de datos tiene la capacidad suficiente para procesar la carga de trabajo habitual una vez que se activa la conmutación por error.

> [!IMPORTANT]
> No se puede escalar la base de datos principal en un grupo de conmutación por error a un nivel superior, a menos que la base de datos secundaria en primer lugar se escala al nivel superior. Si intenta escalar la base de datos principal antes de que se escala la base de datos secundaria, puede recibir el siguiente error:
>
> `Error message: The source database 'Primaryserver.DBName' cannot have higher edition than the target database 'Secondaryserver.DBName'. Upgrade the edition on the target before upgrading the source.`
>

## <a name="preventing-the-loss-of-critical-data"></a>Evitar la pérdida de datos críticos

Debido a la elevada latencia de las redes de área extensa, la copia continua usa un mecanismo de replicación asincrónica. La replicación asincrónica hace inevitable la pérdida de cierta cantidad datos si se produce un error. Sin embargo, es posible que algunas aplicaciones exijan que no se pierdan datos. Para proteger estas actualizaciones críticas, un desarrollador de aplicaciones puede llamar al procedimiento del sistema [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) inmediatamente después de confirmar la transacción. La llamada a **sp_wait_for_database_copy_sync** bloquea el subproceso de llamada hasta que se transmite la última transacción confirmada en la base de datos secundaria. Sin embargo, no espera para que las transacciones transmitidas se reproduzcan y se confirmen en la base de datos secundaria. **sp_wait_for_database_copy_sync** está dirigida a un vínculo de copia continua específico. Cualquier usuario con derechos de conexión para la base de datos principal puede llamar a este procedimiento.

> [!NOTE]
> **sp_wait_for_database_copy_sync** evita la pérdida de datos después de la conmutación por error, pero no garantiza la sincronización completa para el acceso de lectura. El retraso causado por una llamada al procedimiento **sp_wait_for_database_copy_sync** puede ser considerable y depende del tamaño del registro de transacciones en el momento de la llamada.

## <a name="monitoring-geo-replication-lag"></a>Supervisión de intervalo de replicación geográfica

Para supervisar el retraso con respecto a RPO, use *replication_lag_sec* columna de [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) en la base de datos principal. Muestra retardo en segundos entre las transacciones confirmadas en el servidor principal y son persistentes en la base de datos secundaria. Por ejemplo, Si el valor del lapso de tiempo es de 1 segundo, significa que si el servidor principal se ve afectado por una interrupción en este momento y conmutación por error está iniciado, no se guardarán 1 segundo de la transtions más reciente. 

Para medir el retraso con respecto a los cambios en la base de datos principal que se han aplicado en la secundaria, es decir, disponible para su lectura desde la base de datos secundaria, comparar *last_commit* tiempo en la base de datos secundaria con el mismo valor en el servidor principal base de datos.

> [!NOTE]
> A veces *replication_lag_sec* en la base de datos principal tiene un valor NULL, lo que significa que la réplica principal no actualmente saber lo lejos la secundaria.   Esto suele suceder después de proceso se reinicia y debe ser una condición temporal. Considere la posibilidad de la aplicación de alertas si el *replication_lag_sec* devuelve NULL para un período prolongado de tiempo. Podría indicar que la base de datos secundaria no puede comunicarse con la principal debido a un error de conectividad permanente. También hay situaciones que podrían causar la diferencia entre *last_commit* tiempo en la base de datos secundaria y en la base de datos principal sea muy grande. Por ejemplo, Si se realiza una confirmación en el servidor principal tras un largo período de ningún cambio, la diferencia se desplazará hasta un valor grande antes de volver rápidamente a 0. Considérelo una condición de error cuando la diferencia entre estos dos valores permanece grande durante mucho tiempo.


## <a name="programmatically-managing-active-geo-replication"></a>Administración mediante programación de la replicación geográfica activa

Como se dijo antes, la replicación geográfica activa también puede administrarse mediante programación con Azure PowerShell y la API REST. En las tablas siguientes se describe el conjunto de comandos disponibles. La replicación geográfica activa incluye un conjunto de API de Azure Resource Manager para la administración, en el que se incluyen la [API REST de Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) y los [cmdlets de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). Estas API requieren que se usen grupos de recursos y admiten la seguridad basada en roles (RBAC). Para más información sobre cómo implementar los roles de acceso, consulte [Control de acceso basado en roles de Azure](../role-based-access-control/overview.md).

### <a name="t-sql-manage-failover-of-single-and-pooled-databases"></a>T-SQL: Administración de la conmutación por error de bases de datos únicas y agrupadas

> [!IMPORTANT]
> Estos comandos de Transact-SQL solo se aplican a la replicación geográfica activa, no a los grupos de conmutación por error. Por lo tanto, tampoco se aplican a las instancias administradas, ya que solo admiten grupos de conmutación por error.

| Get-Help | DESCRIPCIÓN |
| --- | --- |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Se utiliza el argumento ADD SECONDARY ON SERVER a fin de crear una base de datos secundaria para una base de datos existente e iniciar la replicación de datos |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Se utiliza FAILOVER o FORCE_FAILOVER_ALLOW_DATA_LOSS para cambiar una base de datos de secundaria a principal e iniciar la conmutación por error. |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Se utiliza REMOVE SECONDARY ON SERVER para finalizar una replicación de datos entre una instancia de SQL Database y la base de datos secundaria especificada. |
| [sys.geo_replication_links](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Devuelve información sobre todos los vínculos de replicación existentes para cada base de datos del servidor de Azure SQL Database. |
| [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Obtiene la hora de la última replicación, el retraso de la última replicación y otro tipo de información sobre el vínculo de replicación para una base de datos SQL determinada. |
| [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Muestra el estado de todas las operaciones de base de datos, incluido el estado de los vínculos de replicación. |
| [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |Hace que la aplicación espere a que se repliquen todas las transacciones confirmadas y a que las reconozca la base de datos secundaria activa. |
|  | |

### <a name="powershell-manage-failover-of-single-and-pooled-databases"></a>PowerShell: Administración de la conmutación por error de bases de datos únicas y agrupadas

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) |Obtiene una o más bases de datos. |
| [New-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasesecondary) |Crea una base de datos secundaria para una base de datos existente e inicia la replicación de datos. |
| [Set-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesecondary) |Convierte una base de datos secundaria en principal para iniciar la conmutación por error. |
| [Remove-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesecondary) |Finaliza una replicación de datos entre SQL Database y la base de datos secundaria especificada. |
| [Get-AzSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasereplicationlink) |Obtiene los vínculos de replicación geográfica entre una instancia de Azure SQL Database y un grupo de recursos o SQL Server. |
|  | |

> [!IMPORTANT]
> Si desea scripts de ejemplo, consulte [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) y [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md).

### <a name="rest-api-manage-failover-of-single-and-pooled-databases"></a>API REST: Administración de la conmutación por error de bases de datos únicas y agrupadas

| API | DESCRIPCIÓN |
| --- | --- |
| [Crear o actualizar base de datos (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Crea, actualiza o restaura una base de datos principal o secundaria. |
| [Obtener el estado de creación o actualización de la base de datos](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Devuelve el estado durante una operación de creación. |
| [Establecer la base de datos secundaria como principal (conmutación por error planeada)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failover) |Define qué base de datos secundaria es principal mediante la conmutación por error desde la base de datos principal. **Esta opción no se admite para la instancia administrada.**|
| [Establecer la base de datos secundaria como principal (conmutación por error no planeada)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failoverallowdataloss) |Define qué base de datos secundaria es principal mediante la conmutación por error desde la base de datos principal. Esta operación puede ocasionar pérdida de datos. **Esta opción no se admite para la instancia administrada.**|
| [Obtener vínculo de replicación](https://docs.microsoft.com/rest/api/sql/replicationlinks/get) |Obtiene un vínculo de replicación específico para una Base de datos SQL determinada en una asociación de replicación geográfica. Recupera la información visible en la vista de catálogo sys.geo_replication_links. **Esta opción no se admite para la instancia administrada.**|
| [Vínculos de replicación: lista por base de datos](https://docs.microsoft.com/rest/api/sql/replicationlinks/listbydatabase) | Obtiene todos los vínculos de replicación para una Base de datos SQL determinada en una asociación de replicación geográfica. Recupera la información visible en la vista de catálogo sys.geo_replication_links. |
| [Eliminar vínculo de replicación](https://docs.microsoft.com/rest/api/sql/replicationlinks/delete) | Elimina un vínculo de replicación de base de datos. No se puede realizar durante la conmutación por error. |
|  | |

## <a name="next-steps"></a>Pasos siguientes

- Para los scripts de ejemplo, vea:
  - [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- SQL Database también admite grupos de conmutación por error automática. Para más información, consulte cómo se usan los [grupos de conmutación por error automática](sql-database-auto-failover-group.md).
- Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Continuidad empresarial con Base de datos SQL de Azure](sql-database-business-continuity.md)
- Para saber en qué consisten las copias de seguridad automatizadas de Azure SQL Database, consulte [Información general: copias de seguridad automatizadas de SQL Database](sql-database-automated-backups.md).
- Si quiere saber cómo usar las copias de seguridad automatizadas para procesos de recuperación, consulte [Recuperación de una base de datos a partir de copias de seguridad iniciadas por un servicio](sql-database-recovery-using-backups.md).
- Para obtener información acerca de los requisitos de autenticación para un nuevo servidor principal y la base de datos, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).
