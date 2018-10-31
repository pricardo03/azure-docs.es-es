---
title: 'Grupos de conmutación por error y replicación geográfica activa: Azure SQL Database | Microsoft Docs'
description: Use los grupos de conmutación por error automática con la replicación geográfica activa y habilite la conmutación por error automática por si se produce una interrupción del servicio.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/19/2018
ms.openlocfilehash: 9332bcb5849deacde13d67e4a18ea19af8af2977
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471482"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>Información general: grupos de conmutación por error automática y replicación geográfica activa

La replicación geográfica activa es la característica de Azure SQL Database que permite crear réplicas legibles de la base de datos del mismo centro de datos (región) o de otro.

![Replicación geográfica](./media/sql-database-geo-replication-failover-portal/geo-replication.png )

La replicación geográfica activa se ha diseñado como solución de continuidad empresarial que permite que la aplicación realice una rápida recuperación ante desastres en el caso de que se produzca una interrupción a escala del centro de datos. Si la replicación geográfica está habilitada, la aplicación puede iniciar la conmutación por error en una base de datos secundaria de otra región de Azure. Se admiten hasta cuatro bases de datos secundarias en las mismas o en otras regiones, y las secundarias también se pueden usar para las consultas de acceso de solo lectura. La aplicación o el usuario deben iniciar manualmente la conmutación por error. Después de la conmutación por error, el nuevo elemento principal tiene un punto de conexión diferente.

> [!NOTE]
> La replicación geográfica activa está disponible para todas las bases de datos en todos los niveles de servicio de todas las regiones.
> La replicación geográfica activa no está disponible en Instancia administrada.

Los grupos de conmutación por error automática son una extensión de la replicación geográfica activa. Están diseñados para administrar simultáneamente la conmutación por error de múltiples bases de datos con replicación geográfica mediante una conmutación por error iniciada por una aplicación o delegando la conmutación por error para que la lleve a cabo el servicio SQL Database según los criterios definidos por el usuario. La última opción le permite recuperar automáticamente varias bases de datos relacionadas en una región secundaria después de errores catastróficos u otros eventos no planeados que generen una pérdida total o parcial de la disponibilidad del servicio SQL Database en la región primaria. Además, puede usar las bases de datos secundarias legibles para descargar las cargas de trabajo de consulta de solo lectura. Debido a que los grupos de conmutación por error automática implican varias bases de datos, se deben configurar en el servidor principal. Los servidores principales y los secundarios para las bases de datos del grupo de conmutación por error deben estar en la misma suscripción. Los grupos de conmutación por error automática admiten la replicación de todas las bases de datos en el grupo solo a un servidor secundario en otra región.

> [!NOTE]
> Utilice la replicación geográfica activa si se requieren varios secundarios.

Si usa la replicación geográfica activa y, por cualquier motivo, se produce un error en la base de datos principal o, simplemente, debe desconectarse, puede iniciar la conmutación por error a cualquiera de las bases de datos secundarias. Cuando se activa la conmutación por error a una de las bases de datos secundarias, las demás bases de datos secundarias se vinculan automáticamente a la nueva base de datos principal. Si usa los grupos de conmutación por error automática para administrar la recuperación de bases de datos y cualquier interrupción que afecta a una o varias de las bases de datos del grupo se realizará la conmutación por error automática. Puede configurar la directiva de conmutación por error automática que mejor se adapte a las necesidades de la aplicación, o bien puede decidir no usarla y, en su lugar, usar la activación manual. Además, los grupos de conmutación por error automática proporcionan puntos de conexión de agentes de escucha de lectura-escritura y de solo lectura que no se modifican durante las conmutaciones por error. Ya sea que use la activación de conmutación por error automática o manual, la conmutación por error transforma todas las bases de datos secundarias del grupo en primarias. Una vez que la conmutación por error de una base de datos se completa, el registro de DNS se actualiza automáticamente para redirigir los puntos de conexión a la región nueva.

Con la replicación geográfica activa puede administrar la replicación y la conmutación por error de una base de datos individual o de un conjunto de bases de datos en un servidor o en un grupo elástico. Para ello, puede utilizar:

- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShell: Base de datos única](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Grupo elástico](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Grupo de conmutación por error](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: Base de datos individual o grupo elástico](/sql/t-sql/statements/alter-database-azure-sql-database)
- [API de REST: Base de datos única](https://docs.microsoft.com/rest/api/sql/replicationlinks)
- [API de REST: Grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups).

Después de la conmutación por error, asegúrese de que los requisitos de autenticación para el servidor y la base de datos se configuran en el nuevo elemento principal. Para obtener más información, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md). Esto se aplica a la replicación geográfica activa y a los grupos de conmutación por error automática.

La replicación geográfica activa aprovecha la tecnología [AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server para replicar de forma asincrónica las transacciones confirmadas en la base de datos principal a una base de datos secundaria mediante aislamiento instantáneo de instantáneas. Los grupos de conmutación por error automática proporcionan la semántica de grupo sobre la replicación geográfica activa, pero se usa el mismo mecanismo de replicación asincrónico. Mientras que, en cualquier momento dado, la base de datos secundaria puede ir ligeramente por detrás de la base de datos principal, se garantiza que los datos secundarios nunca tengan transacciones parciales. La redundancia entre regiones permite que las aplicaciones se recuperen rápidamente de la pérdida permanente de todo un centro de datos, o de partes de él, causada por desastres naturales, errores humanos catastróficos o actos malintencionados. Los datos específicos de RPO se encuentran en [Introducción a la continuidad empresarial](sql-database-business-continuity.md).

En la siguiente ilustración, se muestra un ejemplo de replicación geográfica activa configurada con una principal en la región centro-norte de EE. UU. y una secundaria en la región centro-sur de EE. UU.

![Relación de replicación geográfica](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Debido a que las bases de datos secundarias son legibles, se pueden usar para descargar cargas de trabajo de solo lectura como trabajos de informes. Si usa la replicación geográfica activa, la base de datos secundaria se puede crear en la misma región que la base de datos primaria; sin embargo, esto no aumenta la resistencia de la aplicación ante errores catastróficos. Si usa grupos de conmutación por error automática, las bases de datos secundarias siempre se crean en otra región.

Además de para la recuperación ante desastres, la replicación geográfica activa se puede usar en los escenarios siguientes:

- **Migración de base de datos**: puede usar la replicación geográfica activa para migrar una base de datos de un servidor a otro en línea con un tiempo de inactividad mínimo.
- **Actualizaciones de aplicaciones**: puede crear una base de datos secundaria adicional como copia de conmutación por recuperación durante las actualizaciones de la aplicación.

Para lograr una verdadera continuidad empresarial, agregar redundancia de base de datos entre centros de datos es solo parte de la solución. Para recuperar una aplicación (un servicio) de un extremo a otro tras un error catastrófico, es necesario recuperar todos los componentes que constituyen el servicio y cualquier servicio dependiente. Algunos ejemplos de estos componentes son el software cliente (por ejemplo, un explorador con JavaScript personalizado), los front-end web, el almacenamiento y DNS. Es fundamental que todos los componentes sean resistentes a los mismos errores y que estén disponibles en el plazo del objetivo de tiempo de recuperación (RTO) de la aplicación. Por lo tanto, debe identificar todos los servicios dependientes y comprender las garantías y capacidades que ofrecen. A continuación, debe seguir los pasos adecuados para asegurarse de que el servicio funcione durante la conmutación por error de los servicios de los que depende. Para más información sobre el diseño de soluciones para la recuperación ante desastres, consulte [Diseño de soluciones de nube para la continuidad empresarial mediante la replicación geográfica activa](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Funcionalidades de replicación geográfica activa

La característica de replicación geográfica activa ofrece las siguientes funcionalidades esenciales:

- **Replicación asincrónica automática**

 Solo se puede crear una base de datos secundaria mediante su adición a una existente. La base de datos secundaria se puede crear en cualquier servidor de Azure SQL Database. Una vez creada, la base de datos secundaria se rellena con los datos copiados de la base de datos principal. Este proceso se conoce como propagación. Después de crear y propagar la base de datos secundaria, las actualizaciones de la base de datos principal se replican asincrónicamente en la base de datos secundaria de forma automática. La replicación asincrónica significa que las transacciones se confirman en la base de datos principal antes de replicarse en la secundaria.

- **Bases de datos secundarias legibles**

  Una aplicación puede acceder a una base de datos secundaria para operaciones de solo lectura con las mismas entidades de seguridad que las usadas para tener acceso a la base de datos principal u otras diferentes. Las bases de datos secundarias funcionan en el modo de aislamiento de instantánea para asegurarse de que no se retrasa la replicación de las actualizaciones de la base de datos principal (reproducción de registro) por las consultas ejecutadas en la secundaria.

  > [!NOTE]
  > La reproducción de registros se retrasa en la base de datos secundaria si hay actualizaciones del esquema en el servidor principal. Lo último requiere un bloqueo del esquema en la base de datos secundaria.

- **Varias bases de datos secundarias**

  Dos o más bases de datos secundarias aumentan la redundancia y el nivel de protección de la base de datos principal y la aplicación. Si existen varias bases de datos secundarias, la aplicación sigue estando protegida incluso si se produce un error en una de ellas. Si solo hay una base de datos secundaria y se produce un error, la aplicación está expuesta a un mayor riesgo hasta que se crea una nueva base de datos secundaria.

  > [!NOTE]
  > Si usa la replicación geográfica activa para compilar una aplicación distribuida globalmente y necesita proporcionar acceso de solo lectura a los datos en más de cuatro regiones, puede crear una base de datos secundaria a partir de otra base de datos secundaria (este proceso se conoce como encadenamiento). De este modo, puede alcanzar una escala prácticamente ilimitada de replicación de la base de datos. Además, el encadenamiento disminuye la sobrecarga de la replicación desde la base de datos principal. El inconveniente es el mayor retraso de replicación en las últimas bases de datos secundarias.

- **Compatibilidad de bases de datos del grupo elástico**

  Cada réplica puede participar por separado en un grupo elástico o no estar en ningún grupo elástico. La opción del grupo de cada réplica es independiente y no depende de la configuración de ninguna otra réplica (ya sea principal o secundaria). Cada grupo elástico se encuentra dentro de una región individual, por lo que varias réplicas de la misma topología no pueden compartir un grupo elástico.

- **Tamaño de proceso configurable de la base de datos secundaria**

  Es necesario que tanto la base de datos principal como las secundarias tengan el mismo nivel de servicio. También se recomienda que la base de datos secundaria se cree con el mismo tamaño de proceso (unidades de transacción de base de datos o núcleos virtuales) que la principal. Una base de datos secundaria con un tamaño de proceso inferior corre el riesgo de sufrir mayor retraso en la replicación o posible no disponibilidad y, por consiguiente, corre el riesgo de pérdida de datos considerable después de una conmutación por error. Como resultado, el RPO publicado de 5 s no se puede garantizar. El otro riesgo es que después de la conmutación por error, el rendimiento de la aplicación se verá afectado debido a una capacidad de proceso insuficiente de la nueva base de datos principal hasta que se actualice a un tamaño de proceso superior. El tiempo de la actualización depende del tamaño de la base de datos. Además, actualmente, dicha actualización requiere que tanto la base de datos principal como la secundaria estén en línea y, por lo tanto, no se puede completar hasta que se reduzca la interrupción. Si decide crear la base de datos secundaria con un tamaño de proceso más bajo, el gráfico de porcentaje de E/S de registro en Azure Portal proporciona un buen método para estimar el tamaño de proceso mínimo de la base de datos secundaria que es necesario para mantener la carga de replicación. Por ejemplo, si la base de datos principal es P6 (1000 DTU) y su porcentaje de E/S de registro es 50 %, la base de datos secundaria debe ser al menos P4 (500 DTU). También puede recuperar los datos de E/S de registro mediante las vistas de base de datos [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) o [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database).  Para más información sobre los tamaños de proceso de SQL Database, consulte [¿Qué son los niveles de servicio de SQL Database?](sql-database-service-tiers.md)

- **Conmutación por error y conmutación por recuperación controladas por el usuario**

  La aplicación o el usuario puede cambiar explícitamente una base de datos secundaria al rol principal en cualquier momento. Durante una interrupción real, debe utilizarse la opción "no planeada", que promueve inmediatamente una base de datos secundaria a principal. Cuando la base de datos principal que generó el error se recupera y vuelve a estar disponible, el sistema la marca automáticamente como secundaria y la pone al día con la nueva base de datos principal. Por la naturaleza asincrónica de la replicación, se puede perder una pequeña cantidad de datos durante las conmutaciones por error no planeadas antes de que se repliquen los cambios más recientes a la base de datos secundaria. Cuando una base de datos principal con varias secundarias conmuta por error, el sistema vuelve a configurar automáticamente las relaciones de replicación y vincula las bases de datos secundarias restantes a la principal recién promovida sin necesidad de que intervenga el usuario. Una vez que se mitiga la interrupción que causó la conmutación por error, sería conveniente devolver la aplicación a la región primaria. Para hacer esto, se debe invocar el comando de conmutación por error con la opción "planeada".

- **Mantenimiento de las credenciales y las reglas de firewall sincronizadas**

  Se recomienda usar [reglas de firewall de base de datos](sql-database-firewall-configure.md) para las bases de datos con replicación geográfica. Así, estas reglas se pueden replicar con la base de datos para garantizar que todas las bases de datos secundarias tengan las mismas reglas de firewall que la principal. Este enfoque elimina la necesidad de que los clientes configuren y mantengan manualmente las reglas de firewall en los servidores que hospedan tanto la base de datos principal como las secundarias. Igualmente, la utilización de [usuarios de base de datos independiente](sql-database-manage-logins.md) para el acceso a los datos garantiza que la base de datos principal y las secundarias tengan siempre las mismas credenciales de usuario. Por tanto, durante una conmutación por error, no hay interrupciones debidas a discrepancias en los inicios de sesión y las contraseñas. Con la adición de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), los clientes pueden administrar el acceso de usuarios a la base de datos principal y a las secundarias, por lo que ya no es necesario administrar credenciales en las bases de datos.

## <a name="auto-failover-group-capabilities"></a>Funcionalidades de los grupos de conmutación por error automática

La característica de grupos de conmutación por error automática proporciona una abstracción eficaz de la replicación geográfica activa mediante la compatibilidad de la conmutación por error automática y la replicación a nivel de grupo. Además, elimina la necesidad de cambiar la cadena de conexión de SQL después de la conmutación por error al proporcionar los puntos de conexión adicionales del agente de escucha.

> [!NOTE]
> La conmutación por error automática no está disponible en la instancia administrada.
>  

- **Grupo de conmutación por error**

  Se pueden crear uno o más grupos de conmutación por error entre dos servidores en regiones distintas (servidor principal y servidor secundario). Cada grupo puede incluir una o varias bases de datos que se recuperan como unidad en caso de que una o todas las bases de datos principales dejen de estar disponibles debido a una interrupción en la región primaria.  

- **Servidor principal**

  Un servidor que hospeda las bases de datos principales en el grupo de conmutación por error.

- **Servidor secundario**

  Un servidor que hospeda las bases de datos secundarias en el grupo de conmutación por error. El servidor secundario no puede estar en la misma región que el servidor principal.

- **Incorporación de bases de datos a un grupo de conmutación por error**

  Puede colocar varias bases de datos dentro de un servidor o dentro de un grupo elástico en el mismo grupo de conmutación por error. Si agrega una única base de datos al grupo, se crea automáticamente una base de datos secundaria con la misma edición y el mismo tamaño de proceso. Si la base de datos principal está en un grupo elástico, la base de datos secundaria se crea automáticamente en el grupo elástico con el mismo nombre. Si agrega una base de datos que ya tenga una base de datos secundaria en el servidor secundario, el grupo hereda esa replicación geográfica.

  > [!NOTE]
  > Cuando se agrega una base de datos que ya tiene una base de datos secundaria en un servidor que no forma parte del grupo de conmutación por error, se crea una base de datos secundaria nueva en el servidor secundario.

- **Agente de escucha de lectura-escritura de grupo de conmutación por error**

  Un registro CNAME de DNS con el formato **&lt;nombre-grupo-conmutaciónPorError&gt;.baseDeDatos.windows.net** que apunta a la dirección URL del servidor principal actual. Permite que las aplicaciones SQL de lectura y escritura se reconecten sin problemas a la base de datos principal cuando cambia después de la conmutación por error.

- **Agente de escucha de solo lectura de grupo de conmutación por error**

  Un registro CNAME de DNS con el formato **&lt;nombre-grupo-conmutaciónPorError.&gt;baseDeDatos.secundaria.windows.net** que apunta a la dirección URL del servidor secundario. Permite que las aplicaciones SQL de solo lectura se conecten sin problemas a la base de datos secundaria mediante las reglas especificadas de equilibrio de carga.

- **Directiva de conmutación por error automática**

  De manera predeterminada, el grupo de conmutación por error se configura con una directiva de conmutación por error automática. El sistema desencadena la conmutación por error una vez detectado el error y si el período de gracia ha expirado. El sistema debe comprobar que la interrupción no se pueda mitigar mediante la infraestructura de alta disponibilidad integrada del servicio SQL Database debido a la escala del impacto. Si desea controlar el flujo de trabajo de la conmutación por error desde la aplicación, puede desactivar la conmutación por error automática.

- **Directiva de conmutación por error de solo lectura**

  De forma predeterminada, se deshabilita la conmutación por error del agente de escucha de solo lectura. Se asegura de que el rendimiento de la réplica principal no se ve afectado cuando la base de datos secundaria está sin conexión. En cambio, también significa que las sesiones de solo lectura no podrán conectarse hasta que se recupere la base de datos secundaria. Si no puede tolerar un tiempo de inactividad para las sesiones de solo lectura y le parece correcto usar temporalmente la réplica principal para el tráfico de solo lectura y el de lectura y escritura a costa de la degradación del rendimiento potencial de la réplica principal, puede habilitar la conmutación por error para el agente de escucha de solo lectura. En ese caso, el tráfico de solo lectura se redirigirá automáticamente al servidor principal si el servidor secundario no está disponible.  

- **Conmutación por error manual**

  Puede iniciar manualmente la conmutación por error en cualquier momento, independientemente de la configuración de la conmutación por error automática. Si la directiva de conmutación automática por error no está configurada, se requiere la conmutación por error manual para recuperar las bases de datos en el grupo de conmutación por error. Puede iniciar una conmutación por error forzada o sencilla (con sincronización total de los datos). La última se podría usar para reubicar el servidor activo en la región primaria. Cuando se completa la conmutación por error, los registros DNS se actualizan automáticamente para garantizar la conectividad con el servidor correcto.

- **Período de gracia con pérdida de datos**

  Como las bases de datos principal y secundaria se sincronizan con la replicación asincrónica, la conmutación por error puede provocar pérdida de datos. Puede personalizar la directiva de conmutación por error automática para que refleje la tolerancia de la aplicación a la pérdida de datos. Si configura **GracePeriodWithDataLossHours**, puede controlar durante cuánto tiempo espera el sistema antes de iniciar la conmutación por error que es probable que genere una pérdida de datos.

  > [!NOTE]
  > Cuando el sistema detecta que las bases de datos del grupo siguen en línea (por ejemplo, si la interrupción solo afectó al plano de control del servicio), activa inmediatamente la conmutación por error con la sincronización total de los datos (conmutación por error sencilla) independientemente del valor establecido por **GracePeriodWithDataLossHours**. Este comportamiento garantiza que no haya ninguna pérdida de datos durante la recuperación. El período de gracia solo entra en vigor cuando no se puede realizar una conmutación por error sencilla. Si la interrupción se mitiga antes de que expire el período de gracia, no se activa la conmutación por error.

- **Varios grupos de conmutación por error**

  Puede configurar varios grupos de conmutación por error para el mismo par de servidores a fin de controlar la escala de conmutaciones por error. Cada grupo realiza la conmutación por error por separado. Si la aplicación multiinquilino usa grupos elásticos, puede usar esta funcionalidad para combinar bases de datos principales y secundarias en cada grupo. De este modo, puede reducir el impacto de una interrupción a solo la mitad de los inquilinos.

## <a name="best-practices-of-using-failover-groups-for-business-continuity"></a>Procedimientos recomendados para usar grupos de conmutación por error para la continuidad empresarial

Al diseñar un servicio con la continuidad empresarial en mente, debe seguir estas directrices:

- **Utilizar uno o varios grupos de conmutación por error para administrar la conmutación por error de varias bases de datos**

  Se pueden crear uno o más grupos de conmutación por error entre dos servidores en regiones distintas (servidor principal y servidor secundario). Cada grupo puede incluir una o varias bases de datos que se recuperan como unidad en caso de que una o todas las bases de datos principales dejen de estar disponibles debido a una interrupción en la región primaria. El grupo de conmutación por error crea la base de datos geográfica secundaria con el mismo objetivo de servicio que la principal. Si agrega una relación de replicación geográfica existente al grupo de conmutación por error, asegúrese de que la base de datos geográfica secundaria esté configurada con el mismo nivel de servicio y tamaño de proceso que la principal.

- **Utilizar el agente de escucha de lectura-escritura para la carga de trabajo de OLTP**

  Al realizar operaciones OLTP, use **&lt;nombre-grupo-conmutaciónPorError&gt;.baseDeDatos.windows.net** como la dirección URL del servidor y las conexiones se redirigen automáticamente a la base de datos principal. Esta dirección URL no cambia después de la conmutación por error. Tenga en cuenta que la conmutación por error implica actualizar el registro DNS para que las conexiones de cliente se redirijan a la nueva base de datos principal cuando la caché DNS del cliente se haya actualizado.

- **Utilizar el agente de escucha de solo lectura para la carga de trabajo de solo lectura**

  Si tiene una carga de trabajo de solo lectura que es tolerante a una cierta obsolescencia de los datos, puede usar la base de datos secundaria en la aplicación. Para las sesiones de solo lectura, use **&lt;nombre-grupo-conmutaciónPorError&gt;baseDeDatos.secundaria.windows.net** como la dirección URL del servidor y la conexión se redirigirá automáticamente a la base de datos secundaria. También se recomienda indicar en la cadena de conexión el intento de lectura mediante **ApplicationIntent=ReadOnly**.

- **Prepararse para la degradación del rendimiento**

  La decisión de la conmutación por error de SQL es independiente del resto de la aplicación o de otros servicios usados. La aplicación puede ser una mezcla de componentes de una región y componentes de otra. Para evitar la degradación, garantice la implementación redundante de la aplicación en la región de DR y siga las instrucciones de este artículo. Tenga en cuenta que la aplicación de la región de DR no tiene que usar una cadena de conexión diferente.  

- **Prepararse para la pérdida de datos**

  Si se detecta una interrupción, SQL desencadena automáticamente la conmutación por error de lectura/escritura si nos consta que hay cero pérdida de datos. De lo contrario, espera el período especificado por **GracePeriodWithDataLossHours**. Si especificó **GracePeriodWithDataLossHours**, prepárese para la pérdida de datos. Por lo general, durante las interrupciones, Azure favorece la disponibilidad. Si no puede permitirse perder datos, asegúrese de establecer **GracePeriodWithDataLossHours** en un número lo suficientemente grande, por ejemplo, 24 horas.

> [!IMPORTANT]
> Los grupos elásticos con 800 o menos DTU y más de 250 bases de datos que utilizan la replicación geográfica pueden encontrar problemas, como conmutaciones por error planeadas más prolongadas y un menor rendimiento.  Es más probable que estos problemas sucedan con cargas de trabajo intensivas de escritura, cuando los puntos de conexión de replicación geográfica están separados por región geográfica, o cuando se utilizan varios puntos de conexión secundarias para cada base de datos.  Los síntomas de estos problemas aparecen si el intervalo de replicación geográfica aumenta con el tiempo.  Este retardo puede supervisarse con [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Si se producen estos problemas, entre las posibles soluciones está el aumento del número de DTU del grupo o la reducción del número de bases de datos con replicación geográfica en el mismo grupo.

## <a name="failover-groups-and-network-security"></a>Grupos de conmutación por error y la seguridad de red

En algunas aplicaciones, las reglas de seguridad requieren que se restrinja el acceso de red a la capa de datos para un componente específico o a componentes como una máquina virtual, un servicio web, etc. Este requisito presenta algunos desafíos para el diseño de la continuidad empresarial y el uso de grupos de conmutación por error. Debería considerar las siguientes opciones al implementar dicho acceso restringido.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Uso de grupos de conmutación por error y reglas de red virtual

Si usa [reglas y puntos de conexión de servicio Virtual Network](sql-database-vnet-service-endpoint-rule-overview.md) para restringir el acceso a la base de datos SQL, tenga en cuenta que cada punto de conexión de servicio Virtual Network solo se aplica a una región de Azure. El punto de conexión no permite que otras regiones acepten la comunicación de la subred. Por lo tanto, solo las aplicaciones de cliente implementadas en la misma región pueden conectarse a la base de datos principal. Puesto que la conmutación por error provoca que las sesiones de cliente SQL se vuelvan a enrutar al servidor en una región diferente (secundaria), estas sesiones experimentarán un error si se originaron desde un cliente de fuera de esa región. Por ese motivo, la directiva de conmutación por error automática no se puede habilitar si se incluyen los servidores implicados en las reglas de Virtual Network. Para admitir la conmutación por error manual, siga estos pasos:

1. Aprovisione las copias redundantes de los componentes front-end de la aplicación (servicio web, máquinas virtuales, etc.) en la región secundaria
2. Configure las [reglas de red virtual](sql-database-vnet-service-endpoint-rule-overview.md) individualmente para el servidor principal y secundario
3. Habilite la [conmutación por error front-end con una configuración de Traffic Manager](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Inicie la conmutación por error manual cuando se detecte la interrupción

Esta opción está optimizada para las aplicaciones que requieren una latencia coherente entre la capa de datos y de front-end, y admite la recuperación cuando se ven afectadas por la interrupción la capa de datos, la capa front-end o ambas.

> [!NOTE]
> Si usa la **escucha de solo lectura** para equilibrar la carga de una carga de trabajo de solo lectura, asegúrese de que esta carga de trabajo se ejecuta en una máquina virtual o en otro recurso en la región secundaria de modo que pueda conectarse a la base de datos secundaria.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Uso de reglas de firewall de base de datos SQL y grupos de conmutación por error

Si el plan de continuidad empresarial requiere el uso de un proceso de conmutación por error mediante grupos con conmutación por error, puede restringir el acceso a la base de datos SQL usando las reglas de firewall tradicionales.  Para admitir la conmutación por error automática, siga estos pasos:

1. [Cree una dirección IP pública](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address).
2. [Cree un equilibrador de carga público](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) y asígnele la dirección IP pública.
3. [Cree una red virtual y las máquinas virtuales](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) para los componentes de front-end.
4. [Cree un grupo de seguridad de red](../virtual-network/security-overview.md) y configure las conexiones entrantes.
5. Asegúrese de que las conexiones salientes están abiertas para la base de datos SQL de Azure usando 'Sql' [etiqueta de servicio](../virtual-network/security-overview.md#service-tags).
6. Cree una [regla de firewall de base de datos SQL](sql-database-firewall-configure.md) para permitir el tráfico entrante desde la dirección IP pública que se crea en el paso 1.

Para más información acerca de cómo configurar el acceso de salida y qué dirección IP usar en las reglas de firewall, consulte [Conexiones salientes del equilibrador de carga](../load-balancer/load-balancer-outbound-connections.md).

La configuración anterior garantizará que la conmutación por error automática no bloqueará las conexiones desde los componentes front-end y se da por supuesto que la aplicación puede tolerar la mayor latencia entre la capa de datos y el front-end.

> [!IMPORTANT]
> Para garantizar la continuidad empresarial ante interrupciones regionales, debe garantizar la redundancia geográfica tanto para los componentes front-end como para las bases de datos.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Actualización o degradación de una base de datos principal

Puede actualizar o degradar una base de datos principal a un tamaño de proceso diferente (en el mismo nivel de servicio) sin desconectar las bases de datos secundarias. Al actualizar, se recomienda que actualice la base de datos secundaria en primer lugar y, a continuación, la principal. Al degradar, invierta el orden: degrade primero la base de datos principal y, después, la secundaria. Cuando actualiza la base de datos a un nivel de servicio diferente, o la cambia a una versión anterior, se aplica esta recomendación.

> [!NOTE]
> Si creó una base de datos secundaria como parte de la configuración del grupo de conmutación por error, no se recomienda que la degrade. De este modo, se garantiza que la capa de datos tiene la capacidad suficiente para procesar la carga de trabajo habitual una vez que se activa la conmutación por error.

## <a name="preventing-the-loss-of-critical-data"></a>Evitar la pérdida de datos críticos

Debido a la elevada latencia de las redes de área extensa, la copia continua usa un mecanismo de replicación asincrónica. La replicación asincrónica hace inevitable la pérdida de cierta cantidad datos si se produce un error. Sin embargo, es posible que algunas aplicaciones exijan que no se pierdan datos. Para proteger estas actualizaciones críticas, un desarrollador de aplicaciones puede llamar al procedimiento del sistema [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) inmediatamente después de confirmar la transacción. La llamada a **sp_wait_for_database_copy_sync** bloquea el subproceso de llamada hasta que se transmite la última transacción confirmada en la base de datos secundaria. Sin embargo, no espera para que las transacciones transmitidas se reproduzcan y se confirmen en la base de datos secundaria. **sp_wait_for_database_copy_sync** está dirigida a un vínculo de copia continua específico. Cualquier usuario con derechos de conexión para la base de datos principal puede llamar a este procedimiento.

> [!NOTE]
> **sp_wait_for_database_copy_sync** evita la pérdida de datos después de la conmutación por error, pero no garantiza la sincronización completa para el acceso de lectura. El retraso causado por una llamada al procedimiento **sp_wait_for_database_copy_sync** puede ser considerable y depende del tamaño del registro de transacciones en el momento de la llamada.

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Administración mediante programación de los grupos de conmutación por error y de la replicación geográfica activa

Como se ha mencionado antes, los grupos de conmutación automática por error y la replicación geográfica activa también pueden administrarse mediante programación con Azure PowerShell y la API REST. En las tablas siguientes se describe el conjunto de comandos disponibles. La replicación geográfica activa incluye un conjunto de API de Azure Resource Manager para la administración, en el que se incluyen la [API REST de Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) y los [cmdlets de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). Estas API requieren que se usen grupos de recursos y admiten la seguridad basada en roles (RBAC). Para más información sobre cómo implementar los roles de acceso, consulte [Control de acceso basado en roles de Azure](../role-based-access-control/overview.md).

### <a name="manage-sql-database-failover-using-transact-sql"></a>Administración de la conmutación por error de SQL Database mediante Transact-SQL

| Get-Help | DESCRIPCIÓN |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Se utiliza el argumento ADD SECONDARY ON SERVER a fin de crear una base de datos secundaria para una base de datos existente e iniciar la replicación de datos |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Se utiliza FAILOVER o FORCE_FAILOVER_ALLOW_DATA_LOSS para cambiar una base de datos de secundaria a principal e iniciar la conmutación por error. |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Se utiliza REMOVE SECONDARY ON SERVER para finalizar una replicación de datos entre una instancia de SQL Database y la base de datos secundaria especificada. |
| [sys.geo_replication_links (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Devuelve información sobre todos los vínculos de replicación existentes para cada base de datos del servidor lógico de Azure SQL Database. |
| [sys.dm_geo_replication_link_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Obtiene la hora de la última replicación, el retraso de la última replicación y otro tipo de información sobre el vínculo de replicación para una base de datos SQL determinada. |
| [sys.dm_operation_status (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Muestra el estado de todas las operaciones de base de datos, incluido el estado de los vínculos de replicación. |
| [sp_wait_for_database_copy_sync (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |Hace que la aplicación espere a que se repliquen todas las transacciones confirmadas y a que las reconozca la base de datos secundaria activa. |
|  | |

### <a name="manage-sql-database-failover-using-powershell"></a>Administración de la conmutación por error de SQL Database mediante PowerShell

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Obtiene una o más bases de datos. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Crea una base de datos secundaria para una base de datos existente e inicia la replicación de datos. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Convierte una base de datos secundaria en principal para iniciar la conmutación por error. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Finaliza una replicación de datos entre SQL Database y la base de datos secundaria especificada. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Obtiene los vínculos de replicación geográfica entre una instancia de Azure SQL Database y un grupo de recursos o SQL Server. |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Este comando crea un grupo de conmutación por error y lo registra en el servidor principal y el servidor secundario|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Quita el grupo de conmutación por error del servidor y elimina todas las bases de datos secundarias incluidas en el grupo |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Recupera la configuración del grupo de conmutación por error |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Modifica la configuración del grupo de conmutación por error |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Desencadena la conmutación por error del grupo de conmutación por error al servidor secundario |
|  | |

> [!IMPORTANT]
> Para los scripts de ejemplo, vea [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md) y [Configuración y conmutación por error de un grupo de conmutación por error para una única base de datos](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

### <a name="manage-sql-database-failover-using-the-rest-api"></a>Administración de la conmutación por error de SQL Database mediante API de REST

| API | DESCRIPCIÓN |
| --- | --- |
| [Crear o actualizar base de datos (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Crea, actualiza o restaura una base de datos principal o secundaria. |
| [Obtener el estado de creación o actualización de la base de datos](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Devuelve el estado durante una operación de creación. |
| [Establecer la base de datos secundaria como principal (conmutación por error planeada)](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_failover) |Define qué base de datos de réplica es la principal mediante la conmutación por error desde la base de datos de réplica principal actual. |
| [Establecer la base de datos secundaria como principal (conmutación por error no planeada)](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_failoverallowdataloss) |Define qué base de datos de réplica es la principal mediante la conmutación por error desde la base de datos de réplica principal actual. Esta operación puede ocasionar pérdida de datos. |
| [Obtener vínculo de replicación](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_get) |Obtiene un vínculo de replicación específico para una Base de datos SQL determinada en una asociación de replicación geográfica. Recupera la información visible en la vista de catálogo sys.geo_replication_links. |
| [Vínculos de replicación: lista por base de datos](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_listbydatabase) | Obtiene todos los vínculos de replicación para una Base de datos SQL determinada en una asociación de replicación geográfica. Recupera la información visible en la vista de catálogo sys.geo_replication_links. |
| [Eliminar vínculo de replicación](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_delete) | Elimina un vínculo de replicación de base de datos. No se puede realizar durante la conmutación por error. |
| [Crear o actualizar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_createorupdate) | Crea o actualiza un grupo de conmutación por error. |
| [Eliminar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_delete) | Quita el grupo de conmutación por error del servidor. |
| [Conmutación por error (planeada)](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_failover) | Conmuta por error desde el servidor principal actual en este servidor. |
| [Forzar la conmutación por error ocasiona la pérdida de datos](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_forcefailoverallowdataloss) |Conmuta por error desde el servidor principal actual en este servidor. Esta operación puede ocasionar pérdida de datos. |
| [Obtener grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_get) | Obtiene un grupo de conmutación por error. |
| [Enumerar grupos de conmutación por error por servidor](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_listbyserver) | Enumera los grupos de conmutación por error de un servidor. |
| [Actualizar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_update) | Actualiza un grupo de conmutación por error. |
|  | |

## <a name="next-steps"></a>Pasos siguientes

- Para los scripts de ejemplo, vea:
  - [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Configuración y conmutación por error de un grupo de conmutación por error para una sola base de datos](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Continuidad empresarial con Base de datos SQL de Azure](sql-database-business-continuity.md)
- Para saber en qué consisten las copias de seguridad automatizadas de Azure SQL Database, consulte [Información general: copias de seguridad automatizadas de SQL Database](sql-database-automated-backups.md).
- Si quiere saber cómo usar las copias de seguridad automatizadas para procesos de recuperación, consulte [Recuperación de una base de datos a partir de copias de seguridad iniciadas por un servicio](sql-database-recovery-using-backups.md).
- Para obtener información acerca de los requisitos de autenticación para un nuevo servidor principal y la base de datos, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).
