---
title: 'Grupos de conmutación por error: Azure SQL Database | Microsoft Docs'
description: Los grupos de conmutación por error automática son una característica de SQL Database que le permite administrar la replicación y la conmutación por error automática o coordinada de un grupo de bases de datos en un servidor de SQL Database o de todas las bases de datos de la instancia administrada.
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
ms.date: 02/08/2019
ms.openlocfilehash: 0cffb4fdff4bddc33c6938e27425035c929808b7
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301934"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>Uso de grupos de conmutación por error automática para permitir la conmutación por error de varias bases de datos de manera transparente y coordinada

Los grupos de conmutación por error automática son una característica de SQL Database que le permite administrar la replicación y la conmutación por error de un grupo de bases de datos de un servidor de SQL Database, o todas las bases de datos de una instancia administrada, a otra región (actualmente en versión preliminar pública para Instancia administrada). Emplea la misma tecnología subyacente que la [replicación geográfica activa](sql-database-active-geo-replication.md). Puede iniciar la conmutación por error manualmente o puede delegarla en el servicio de SQL Database según una directiva definida por el usuario. La última opción le permite recuperar automáticamente varias bases de datos relacionadas en una región secundaria después de errores catastróficos u otros eventos no planeados que generen una pérdida total o parcial de la disponibilidad del servicio SQL Database en la región primaria. Además, puede usar las bases de datos secundarias legibles para descargar las cargas de trabajo de consulta de solo lectura. Debido a que los grupos de conmutación por error automática implican varias bases de datos, se deben configurar en el servidor principal. Los servidores principales y los secundarios para las bases de datos del grupo de conmutación por error deben estar en la misma suscripción. Los grupos de conmutación por error automática admiten la replicación de todas las bases de datos en el grupo solo a un servidor secundario en otra región.

> [!NOTE]
> Al trabajar con bases de datos únicas o agrupadas en un servidor de SQL Database, si quiere varias bases de datos secundarias en la misma región u otra diferente, use la [replicación geográfica activa](sql-database-active-geo-replication.md).

Cuando se usan grupos de conmutación por error automática con una directiva de conmutación por error automática, cualquier interrupción que afecte a una o varias de las bases de datos del grupo tiene como resultado la conmutación por error automática. Además, los grupos de conmutación por error automática proporcionan puntos de conexión de agentes de escucha de lectura-escritura y de solo lectura que no se modifican durante las conmutaciones por error. Ya sea que use la activación de conmutación por error automática o manual, la conmutación por error transforma todas las bases de datos secundarias del grupo en primarias. Después de que la conmutación por error de una base de datos finaliza, el registro de DNS se actualiza automáticamente para redirigir los puntos de conexión a la nueva región. Para información sobre los datos específicos de RPO y RTO, consulte [Introducción a la continuidad empresarial](sql-database-business-continuity.md).

Cuando se usan grupos de conmutación por error automática con la directiva de conmutación por error automática, cualquier interrupción que afecte a las bases de datos del servidor de SQL Database o de la instancia administrada tiene como resultado la conmutación por error automática. Puede administrar el grupo de conmutación por error automática mediante:

- [Azure Portal](sql-database-implement-geo-distributed-database.md)
- [PowerShell: grupo de conmutación por error](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [API REST: grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups)

Después de la conmutación por error, asegúrese de que los requisitos de autenticación para el servidor y la base de datos se configuran en el nuevo elemento principal. Para obtener más información, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).

Para lograr una verdadera continuidad empresarial, agregar redundancia de base de datos entre centros de datos es solo parte de la solución. Para recuperar una aplicación (un servicio) de un extremo a otro tras un error catastrófico, es necesario recuperar todos los componentes que constituyen el servicio y cualquier servicio dependiente. Algunos ejemplos de estos componentes son el software cliente (por ejemplo, un explorador con JavaScript personalizado), los front-end web, el almacenamiento y DNS. Es fundamental que todos los componentes sean resistentes a los mismos errores y que estén disponibles en el plazo del objetivo de tiempo de recuperación (RTO) de la aplicación. Por lo tanto, debe identificar todos los servicios dependientes y comprender las garantías y capacidades que ofrecen. A continuación, debe seguir los pasos adecuados para asegurarse de que el servicio funcione durante la conmutación por error de los servicios de los que depende. Para más información sobre el diseño de soluciones para la recuperación ante desastres, consulte [Diseño de soluciones de nube para la continuidad empresarial mediante la replicación geográfica activa](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="auto-failover-group-terminology-and-capabilities"></a>Funcionalidades y terminología del grupo de conmutación por error automática

- **Grupo de conmutación por error**

  Un grupo de conmutación por error es un grupo de bases de datos administradas por un único servidor de SQL Database o en una sola instancia administrada que puede conmutar por error como una unidad a otra región en caso de que algunas o todas bases de datos principales dejen de estar disponibles debido a una interrupción en la región primaria.

  - **Servidores de SQL Database**

     Con los servidores de SQL Database, algunas o todas las bases de datos de usuario de un único servidor de SQL Database pueden colocarse en un grupo de conmutación por error. Además, un servidor de SQL Database admite varios grupos de conmutación por error en un único servidor de SQL Database.

  - **Instancias administradas**
  
     Con una instancia administrada, un grupo de conmutación por error contiene todas las bases de datos de usuario de la instancia administrada y, por tanto, una instancia administrada solo admite un grupo de conmutación por error.

- **Principal**

  El servidor de SQL Database o la Instancia administrada que hospedan las bases de datos principales del grupo de conmutación por error.

- **Secundario**

  El servidor de SQL Database o la Instancia administrada que hospedan las bases de datos secundarias del grupo de conmutación por error. La base de datos secundaria no puede estar en la misma región que la principal.

- **Incorporación de bases de datos únicas a un grupo de conmutación por error**

  Puede poner varias bases de datos únicas en el mismo servidor de SQL Database e incluirlas en el mismo grupo de conmutación por error. Si agrega una base de datos única al grupo de conmutación por error, automáticamente se creará una base de datos secundaria con la misma edición y el mismo tamaño de proceso del servidor secundario.  Ese servidor es el que especificó cuando creó el grupo de conmutación por error. Si agrega una base de datos que ya tenga una base de datos secundaria en el servidor secundario, el grupo heredará el vínculo de replicación geográfica. Cuando se agrega una base de datos que ya tiene una base de datos secundaria en un servidor que no forma parte del grupo de conmutación por error, se crea otra base de datos secundaria en el servidor secundario.
  
> [!IMPORTANT]
  > En una instancia administrada, se replican todas las bases de datos de usuario. No se puede seleccionar un subconjunto de bases de datos de usuario para la replicación en el grupo de conmutación por error.

- **Incorporación de bases de datos de un grupo elástico a un grupo de conmutación por error**

  Puede poner varias bases de datos en el mismo grupo elástico e incluirlas en el mismo grupo de conmutación por error. Si la base de datos principal está en un grupo elástico, la base de datos secundaria se creará automáticamente en un grupo elástico con el mismo nombre (grupo secundario). Debe asegurarse de que el servidor secundario contiene un grupo elástico con el mismo nombre exacto y suficiente capacidad libre para hospedar las bases de datos secundarias que va a crear el grupo de conmutación por error. Si agrega una base de datos en un grupo que ya tiene una base de datos secundaria en el grupo secundario, el grupo heredará el vínculo de replicación geográfica. Cuando se agrega una base de datos que ya tiene una base de datos secundaria en un servidor que no forma parte del grupo de conmutación por error, se crea otra base de datos secundaria en el grupo secundario.
  
  - **Agente de escucha de lectura-escritura de grupo de conmutación por error**

  Un registro de CNAME de DNS formado que apunta a la dirección URL de la base de datos principal actual. Permite que las aplicaciones SQL de lectura y escritura se reconecten sin problemas a la base de datos principal cuando cambia después de la conmutación por error.

  - **Registro CNAME de DNS del servidor de SQL Database para el agente de escucha de lectura y escritura**

     En un servidor de SQL Database, el registro CNAME de DNS para el grupo de conmutación por error que apunta a la dirección URL de la base de datos principal actual tiene el formato `failover-group-name.database.windows.net`.

  - **Registro CNAME de DNS de Instancia administrada para el agente de escucha de lectura y escritura**

     En una instancia administrada, el registro CNAME de DNS para el grupo de conmutación por error que apunta a la dirección URL de la base de datos principal tiene el formato `failover-group-name.zone_id.database.windows.net`.

- **Agente de escucha de solo lectura de grupo de conmutación por error**

  Un registro CNAME de DNS formado que apunta al agente de escucha de solo lectura que apunta a la dirección URL de la base de datos secundaria. Permite que las aplicaciones SQL de solo lectura se conecten sin problemas a la base de datos secundaria mediante las reglas de equilibrio de carga especificadas.

  - **Registro CNAME de DNS del servidor de SQL Database para el agente de escucha de solo lectura**

     En un servidor de SQL Database, el registro CNAME de DNS para el agente de escucha de solo lectura que apunta a la dirección URL de base de datos secundaria tiene el formato `failover-group-name.secondary.database.windows.net`.

  - **Registro CNAME de DNS de Instancia administrada para el agente de escucha de solo lectura**

     En una instancia administrada, el registro CNAME de DNS para el agente de escucha de solo lectura que apunta a la dirección URL de base de datos secundaria tiene el formato `failover-group-name.zone_id.database.windows.net`.

- **Directiva de conmutación por error automática**

  De manera predeterminada, un grupo de conmutación por error se configura con una directiva de conmutación por error automática. El servicio SQL Database desencadena la conmutación por error después de que se detecta el error y si el período de gracia ha expirado. El sistema debe comprobar que la interrupción no se pueda mitigar mediante la [infraestructura de alta disponibilidad integrada del servicio SQL Database](sql-database-high-availability.md) debido a la escala del impacto. Si desea controlar el flujo de trabajo de la conmutación por error desde la aplicación, puede desactivar la conmutación por error automática.

- **Directiva de conmutación por error de solo lectura**

  De forma predeterminada, se deshabilita la conmutación por error del agente de escucha de solo lectura. Se asegura de que el rendimiento de la réplica principal no se ve afectado cuando la base de datos secundaria está sin conexión. En cambio, también significa que las sesiones de solo lectura no podrán conectarse hasta que se recupere la base de datos secundaria. Si no puede tolerar tiempo de inactividad en las sesiones de solo lectura y le parece correcto usar temporalmente la base de datos principal para el tráfico de solo lectura y el de lectura y escritura a costa de la posible degradación del rendimiento de esta, puede habilitar la conmutación por error para el agente de escucha de solo lectura. En ese caso, el tráfico de solo lectura se redirigirá automáticamente a la base de datos principal si la secundaria no está disponible.

- **Conmutación por error planeada**

   La conmutación por error planeada realiza una sincronización completa entre las bases de datos principales y secundarias antes de que el rol principal pase a la secundaria. De esta manera se garantiza que no hay pérdida de datos. La conmutación por error planeada se usa en los siguientes escenarios:

  - Realizar simulacros de recuperación ante desastres (DR) en producción cuando no es aceptable la pérdida de datos
  - Reubicar las bases de datos a una región diferente
  - Devolver las bases de datos a la región primaria después de que se ha corregido la interrupción (conmutación por recuperación)

- **Conmutación por error no planeada**

   La conmutación por error no planeada o forzada cambia inmediatamente el rol principal a la base de datos secundaria sin sincronización con la principal. Esta operación dará lugar a la pérdida de datos. La conmutación por error no planeada se usa como método de recuperación durante las interrupciones cuando no es posible acceder a la base de datos principal. Cuando la base de datos principal original vuelve a estar en línea, se vuelve a conectar automáticamente sin sincronización y se convierte en una nueva base de datos secundaria.

- **Conmutación por error manual**

  Puede iniciar manualmente la conmutación por error en cualquier momento, independientemente de la configuración de la conmutación por error automática. Si la directiva de conmutación por error automática no está configurada, se requiere la conmutación por error manual para recuperar las bases de datos del grupo de conmutación por error a la base de datos secundaria. Puede iniciar una conmutación por error forzada o sencilla (con sincronización total de los datos). El segundo caso se podría usar para reubicar la base de datos principal en la región secundaria. Cuando se completa la conmutación por error, los registros de DNS se actualizan automáticamente para garantizar la conectividad a la nueva base de datos principal.

- **Período de gracia con pérdida de datos**

  Como las bases de datos principal y secundaria se sincronizan con la replicación asincrónica, la conmutación por error puede provocar pérdida de datos. Puede personalizar la directiva de conmutación por error automática para que refleje la tolerancia de la aplicación a la pérdida de datos. Si configura **GracePeriodWithDataLossHours**, puede controlar durante cuánto tiempo espera el sistema antes de iniciar la conmutación por error que es probable que genere una pérdida de datos.

- **Varios grupos de conmutación por error**

  Puede configurar varios grupos de conmutación por error para el mismo par de servidores a fin de controlar la escala de conmutaciones por error. Cada grupo realiza la conmutación por error por separado. Si la aplicación multiinquilino usa grupos elásticos, puede usar esta funcionalidad para combinar bases de datos principales y secundarias en cada grupo. De este modo, puede reducir el impacto de una interrupción a solo la mitad de los inquilinos.

  > [!IMPORTANT]
  > Instancia administrada no admite varios grupos de conmutación por error.

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>Procedimientos recomendados para usar grupos de conmutación por error con bases de datos únicas y grupos elásticos

El grupo de conmutación por error automática debe estar configurado en el servidor de SQL Database principal y se conectará al servidor de SQL Database secundario de una región de Azure diferente.  Los grupos pueden incluir todas las bases de datos de estos servidores o algunas de ellas. En el siguiente diagrama se ilustra una configuración típica de una aplicación de nube con redundancia geográfica que usa varias bases de datos y un grupo de conmutación por error automática.

![conmutación por error automática](./media/sql-database-auto-failover-group/auto-failover-group.png)

Al diseñar un servicio teniendo en cuenta la continuidad empresarial, siga estas directrices:

- **Utilizar uno o varios grupos de conmutación por error para administrar la conmutación por error de varias bases de datos**

  Se pueden crear uno o más grupos de conmutación por error entre dos servidores en regiones distintas (servidor principal y servidor secundario). Cada grupo puede incluir una o varias bases de datos que se recuperan como unidad en caso de que una o todas las bases de datos principales dejen de estar disponibles debido a una interrupción en la región primaria. El grupo de conmutación por error crea la base de datos geográfica secundaria con el mismo objetivo de servicio que la principal. Si agrega una relación de replicación geográfica existente al grupo de conmutación por error, asegúrese de que la base de datos geográfica secundaria esté configurada con el mismo nivel de servicio y tamaño de proceso que la principal.

- **Utilizar el agente de escucha de lectura-escritura para la carga de trabajo de OLTP**

  Al realizar operaciones OLTP, use `failover-group-name.database.windows.net` como dirección URL del servidor y las conexiones se redirigirán automáticamente a la base de datos principal. Esta dirección URL no cambia después de la conmutación por error. Tenga en cuenta que la conmutación por error implica actualizar el registro DNS para que las conexiones de cliente se redirijan a la nueva base de datos principal cuando la caché DNS del cliente se haya actualizado.

- **Utilizar el agente de escucha de solo lectura para la carga de trabajo de solo lectura**

  Si tiene una carga de trabajo de solo lectura que es tolerante a una cierta obsolescencia de los datos, puede usar la base de datos secundaria en la aplicación. Para las sesiones de solo lectura, use `failover-group-name.secondary.database.windows.net` como dirección URL del servidor y la conexión se redirigirá automáticamente a la base de datos secundaria. También se recomienda indicar en la cadena de conexión el intento de lectura mediante **ApplicationIntent=ReadOnly**.

- **Prepararse para la degradación del rendimiento**

  La decisión de la conmutación por error de SQL es independiente del resto de la aplicación o de otros servicios usados. La aplicación puede ser una mezcla de componentes de una región y componentes de otra. Para evitar la degradación, garantice la implementación redundante de la aplicación en la región de recuperación ante desastres y siga estas [instrucciones de seguridad de red](#failover-groups-and-network-security).

  > [!NOTE]
  > La aplicación de la región de recuperación ante desastres no tiene que usar una cadena de conexión diferente.  

- **Prepararse para la pérdida de datos**

  Si se detecta una interrupción, SQL espera durante el período especificado por **GracePeriodWithDataLossHours**. El valor predeterminado es 1 hora. Si no puede permitirse perder datos, asegúrese de establecer **GracePeriodWithDataLossHours** en un número lo suficientemente grande, por ejemplo, 24 horas. Use un grupo de conmutación por error manual para conmutar por recuperación la base de datos secundaria a la principal.

> [!IMPORTANT]
> Los grupos elásticos con 800 o menos DTU y más de 250 bases de datos que utilizan la replicación geográfica pueden encontrar problemas, como conmutaciones por error planeadas más prolongadas y un menor rendimiento.  Es más probable que estos problemas sucedan con cargas de trabajo intensivas de escritura, cuando los puntos de conexión de replicación geográfica están separados por región geográfica, o cuando se utilizan varios puntos de conexión secundarias para cada base de datos.  Los síntomas de estos problemas aparecen si el intervalo de replicación geográfica aumenta con el tiempo.  Este retardo puede supervisarse con [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Si se producen estos problemas, entre las posibles soluciones está el aumento del número de DTU del grupo o la reducción del número de bases de datos con replicación geográfica en el mismo grupo.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Procedimientos recomendados para usar grupos de conmutación por error con instancias administradas

El grupo de conmutación por error automática debe estar configurado en la instancia principal y se conectará a la instancia secundaria de una región de Azure diferente.  Todas las bases de datos de la instancia se replicarán en la instancia secundaria. En el siguiente diagrama se ilustra una configuración típica de una aplicación de nube con redundancia geográfica que usa instancia administrada y un grupo de conmutación por error automática.

![conmutación por error automática](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!IMPORTANT]
> Los grupos de conmutación por error automática para Instancia administrada están en versión preliminar pública.

Si la aplicación usa Instancia administrada como capa de datos, siga estas directrices generales al diseñar para la continuidad empresarial:

- **Crear la instancia secundaria en la misma zona DNS que la principal**

  Cuando se crea una instancia, se genera automáticamente un identificador único como zona DNS y se incluye en el nombre DNS de la instancia. Además, se aprovisiona para esta instancia un certificado de varios dominios (SAN) con el campo SAN con el formato `zone_id.database.windows.net`. Este certificado puede usarse para autenticar las conexiones de cliente en una instancia de la misma zona DNS. Para garantizar la conectividad sin interrupciones a la instancia principal después de la conmutación por error, las instancias principales y secundarias deben estar en la misma zona DNS. Cuando la aplicación esté lista para la implementación en producción, cree una instancia secundaria en una región distinta y asegúrese de que comparte la zona DNS con la instancia principal. Para ello, especifique un parámetros opcional `DNS Zone Partner` mediante Azure Portal, PowerShell o la API REST.

  Para más información sobre cómo crear la instancia secundaria en la misma zona DNS que la instancia principal, consulte [Managing failover groups with Managed Instances (preview)](#powershell-managing-failover-groups-with-managed-instances-preview) (Administración de grupos de conmutación por error con instancias administradas [versión preliminar]).

- **Habilitar el tráfico de replicación entre dos instancias**

  Dado que cada instancia está aislada en su propia red virtual, se debe permitir el tráfico bidireccional entre estas redes virtuales. Consulte [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

- **Configurar un grupo de conmutación por error para administrar la conmutación por error de la instancia completa**

  El grupo de conmutación por error administrará la conmutación por error de todas las bases de datos de la instancia. Cuando se crea un grupo, cada base de datos de la instancia se replicará geográficamente de manera automática a la instancia secundaria. No se pueden usar grupos de conmutación por error para iniciar una conmutación por error parcial de un subconjunto de las bases de datos.

  > [!IMPORTANT]
  > Si se quita una base de datos de la instancia principal, también se eliminará automáticamente en la instancia secundaria de replicación geográfica.

- **Utilizar el agente de escucha de lectura-escritura para la carga de trabajo de OLTP**

  Al realizar operaciones OLTP, use `failover-group-name.zone_id.database.windows.net` como dirección URL del servidor y las conexiones se redirigirán automáticamente a la base de datos principal. Esta dirección URL no cambia después de la conmutación por error. La conmutación por error implica actualizar el registro DNS para que las conexiones de cliente se redirijan a la nueva base de datos principal cuando la caché DNS del cliente se haya actualizado. Dado que la instancia secundaria comparte la zona DNS con la principal, la aplicación cliente podrá volver a conectarse a ella con el mismo certificado de SAN.

- **Conectar directamente a la base de datos secundaria con replicación geográfica para las consultas de solo lectura**

  Si tiene una carga de trabajo de solo lectura que es tolerante a una cierta obsolescencia de los datos, puede usar la base de datos secundaria en la aplicación. Para conectarse directamente a la base de datos secundaria con replicación geográfica, use `server.secondary.zone_id.database.windows.net` como dirección URL del servidor y la conexión se realizará directamente a dicha base de datos.

  > [!NOTE]
  > En determinados niveles de servicio, Azure SQL Database admite el uso de [réplicas de solo lectura](sql-database-read-scale-out.md) para equilibrar las cargas de trabajo de consultas de solo lectura mediante la capacidad de una réplica de solo lectura y el uso del parámetro `ApplicationIntent=ReadOnly` en la cadena de conexión. Cuando se ha configurado una base de datos secundaria con replicación geográfica, puede usar esta funcionalidad para conectarse a una réplica de solo lectura de la ubicación principal o de la ubicación con replicación geográfica.
  > - Para conectarse a una réplica de solo lectura en la ubicación principal, use `failover-group-name.zone_id.database.windows.net`.
  > - Para conectarse a una réplica de solo lectura en la ubicación principal, use `failover-group-name.secondary.zone_id.database.windows.net`.

- **Prepararse para la degradación del rendimiento**

  La decisión de la conmutación por error de SQL es independiente del resto de la aplicación o de otros servicios usados. La aplicación puede ser una mezcla de componentes de una región y componentes de otra. Para evitar la degradación, garantice la implementación redundante de la aplicación en la región de recuperación ante desastres y siga estas [instrucciones de seguridad de red](#failover-groups-and-network-security).

- **Prepararse para la pérdida de datos**

  Si se detecta una interrupción, SQL desencadena automáticamente la conmutación por error de lectura/escritura si nos consta que hay cero pérdida de datos. En caso contrario, espera el período especificado por `GracePeriodWithDataLossHours`. Si especificó `GracePeriodWithDataLossHours`, prepárese para la pérdida de datos. Por lo general, durante las interrupciones, Azure favorece la disponibilidad. Si no puede permitirse perder datos, asegúrese de establecer GracePeriodWithDataLossHours en un número lo suficientemente grande, por ejemplo, 24 horas.

  La actualización de DNS del agente de escucha de lectura y escritura sucederá inmediatamente después de que se inicie la conmutación por error. Esta operación no ocasionará pérdida de datos. Sin embargo, el proceso de conmutación de roles de base de datos puede tardar hasta 5 minutos en condiciones normales. Hasta que se complete, algunas bases de datos de la nueva instancia principal seguirán siendo de solo lectura. Si se inicia la conmutación por error mediante PowerShell, toda la operación es sincrónica. Si se inicia mediante Azure Portal, la interfaz de usuario indicará el estado de finalización. Si se inicia mediante la API REST, use el mecanismo de sondeo estándar de Azure Resource Manager para supervisar la finalización.

  > [!IMPORTANT]
  > Use un grupo de conmutación por error manual para mover las bases de datos principales de vuelta a la ubicación original. Cuando se corrija la interrupción que causó la conmutación por error, puede mover las bases de datos principales a la ubicación original. Para ello, debe iniciar la conmutación por error manual del grupo.

## <a name="failover-groups-and-network-security"></a>Grupos de conmutación por error y la seguridad de red

En algunas aplicaciones, las reglas de seguridad requieren que se restrinja el acceso de red a la capa de datos para un componente específico o a componentes como una máquina virtual, un servicio web, etc. Este requisito presenta algunos desafíos para el diseño de la continuidad empresarial y el uso de grupos de conmutación por error. Debería considerar las siguientes opciones al implementar dicho acceso restringido.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Uso de grupos de conmutación por error y reglas de red virtual

Si usa [reglas y puntos de conexión de servicio Virtual Network](sql-database-vnet-service-endpoint-rule-overview.md) para restringir el acceso a la base de datos SQL, tenga en cuenta que cada punto de conexión de servicio Virtual Network solo se aplica a una región de Azure. El punto de conexión no permite que otras regiones acepten la comunicación de la subred. Por lo tanto, solo las aplicaciones de cliente implementadas en la misma región pueden conectarse a la base de datos principal. Puesto que la conmutación por error provoca que las sesiones de cliente SQL se vuelvan a enrutar al servidor en una región diferente (secundaria), estas sesiones experimentarán un error si se originaron desde un cliente de fuera de esa región. Por ese motivo, la directiva de conmutación por error automática no se puede habilitar si se incluyen los servidores implicados en las reglas de Virtual Network. Para admitir la conmutación por error manual, siga estos pasos:

1. Aprovisione las copias redundantes de los componentes front-end de la aplicación (servicio web, máquinas virtuales, etc.) en la región secundaria.
2. Configure las [reglas de red virtual](sql-database-vnet-service-endpoint-rule-overview.md) individualmente para el servidor principal y secundario
3. Habilite la [conmutación por error front-end con una configuración de Traffic Manager](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Inicie la conmutación por error manual cuando se detecte la interrupción. Esta opción está optimizada para las aplicaciones que requieren una latencia coherente entre la capa de datos y de front-end, y admite la recuperación cuando se ven afectadas por la interrupción la capa de datos, la capa front-end o ambas.

> [!NOTE]
> Si usa el **agente de escucha de solo lectura** para equilibrar una carga de trabajo de solo lectura, asegúrese de que esta carga de trabajo se ejecuta en una máquina virtual o en otro recurso en la región secundaria de modo que pueda conectarse a la base de datos secundaria.

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

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>Habilitación de la replicación geográfica entre instancias administradas y sus redes virtuales

Al configurar un grupo de conmutación por error entre instancias administradas principales y secundarias en dos regiones diferentes, cada instancia se aísla mediante una red virtual independiente. Para permitir el tráfico de replicación entre estas redes virtuales, asegúrese de que se cumplen estos requisitos previos:

1. Las dos instancias administradas deben estar en diferentes regiones de Azure.
2. La base de datos secundaria debe estar vacía (sin bases de datos de usuario).
3. Las instancias administradas principales y secundarias deben estar en el mismo grupo de recursos.
4. Las redes virtuales de las que forman parte las instancias administradas deben estar conectadas mediante una instancia de [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md). No se admite el emparejamiento de VNet global.
5. Las dos redes virtuales de instancia administrada no pueden tener direcciones IP superpuestas.
6. Debe configurar sus grupos de seguridad de red (NSG) de forma que los puertos 5022 y el intervalo 11000~12000 estén abiertos a las conexiones entrantes y salientes desde la otra subred con instancia administrada. Esto es para permitir el tráfico de replicación entre las instancias.

    > [!IMPORTANT]
    > Las reglas de seguridad de NSG mal configuradas dan lugar a operaciones de copia de base de datos bloqueadas.

7. Deberá configurar el asociado de zona DNS en la instancia secundaria. Una zona DNS es una propiedad de una instancia administrada. Representa la parte del nombre de host que sigue al nombre de instancia administrada y va delante del prefijo `.database.windows.net`. Se genera como cadena aleatoria durante la creación de la primera instancia administrada de cada red virtual. La zona DNS no se puede modificar tras la creación de la instancia administrada, y todas las instancias administradas dentro de la misma subred comparten el mismo valor de zona DNS. Para configurar el grupo de conmutación por error de Instancia administrada, la instancia administrada principal y la instancia administrada secundaria deben compartir el mismo valor de zona DNS. Para lograr esto, se especifica el parámetro DnsZonePartner al crear la instancia administrada secundaria. La propiedad de asociado de zona DNS define la instancia administrada con la que compartir un grupo de conmutación por error de la instancia. Al pasar el identificador de recurso de otra instancia administrada como entrada de DnsZonePartner, la instancia administrada que se crea actualmente hereda el mismo valor de zona DNS de la instancia administrada del asociado.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Actualización o degradación de una base de datos principal

Puede actualizar o degradar una base de datos principal a un tamaño de proceso diferente (en el mismo nivel de servicio, no entre De uso general y Crítico para la empresa) sin desconectar las bases de datos secundarias. Al actualizar, se recomienda que actualice la base de datos secundaria en primer lugar y, a continuación, la principal. Al degradar, invierta el orden: degrade primero la base de datos principal y, después, la secundaria. Cuando actualiza la base de datos a un nivel de servicio diferente, o la cambia a una versión anterior, se aplica esta recomendación.

> [!NOTE]
> Si creó una base de datos secundaria como parte de la configuración del grupo de conmutación por error, no se recomienda que la degrade. De este modo, se garantiza que la capa de datos tiene la capacidad suficiente para procesar la carga de trabajo habitual una vez que se activa la conmutación por error.

## <a name="preventing-the-loss-of-critical-data"></a>Evitar la pérdida de datos críticos

Debido a la elevada latencia de las redes de área extensa, la copia continua usa un mecanismo de replicación asincrónica. La replicación asincrónica hace inevitable la pérdida de cierta cantidad datos si se produce un error. Sin embargo, es posible que algunas aplicaciones exijan que no se pierdan datos. Para proteger estas actualizaciones críticas, un desarrollador de aplicaciones puede llamar al procedimiento del sistema [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) inmediatamente después de confirmar la transacción. La llamada a **sp_wait_for_database_copy_sync** bloquea el subproceso de llamada hasta que se transmite la última transacción confirmada en la base de datos secundaria. Sin embargo, no espera para que las transacciones transmitidas se reproduzcan y se confirmen en la base de datos secundaria. **sp_wait_for_database_copy_sync** está dirigida a un vínculo de copia continua específico. Cualquier usuario con derechos de conexión para la base de datos principal puede llamar a este procedimiento.

> [!NOTE]
> **sp_wait_for_database_copy_sync** evita la pérdida de datos después de la conmutación por error, pero no garantiza la sincronización completa para el acceso de lectura. El retraso causado por una llamada al procedimiento **sp_wait_for_database_copy_sync** puede ser considerable y depende del tamaño del registro de transacciones en el momento de la llamada.

## <a name="failover-groups-and-point-in-time-restore"></a>Grupos de conmutación por error y restauración a un momento dado

Para información sobre el uso de la restauración a un momento dado con grupos de conmutación por error, consulte [Recuperación a un momento dado (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="programmatically-managing-failover-groups"></a>Administración mediante programación de grupos de conmutación por error

Como se ha mencionado antes, los grupos de conmutación automática por error y la replicación geográfica activa también pueden administrarse mediante programación con Azure PowerShell y la API REST. En las tablas siguientes se describe el conjunto de comandos disponibles. La replicación geográfica activa incluye un conjunto de API de Azure Resource Manager para la administración, en el que se incluyen la [API REST de Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) y los [cmdlets de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). Estas API requieren que se usen grupos de recursos y admiten la seguridad basada en roles (RBAC). Para más información sobre cómo implementar los roles de acceso, consulte [Control de acceso basado en roles de Azure](../role-based-access-control/overview.md).

### <a name="powershell-manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>PowerShell: Administración de la conmutación por error de bases de datos SQL con bases de datos únicas y grupos elásticos

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
| [New-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Este comando crea un grupo de conmutación por error y lo registra en el servidor principal y el servidor secundario|
| [Remove-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Quita el grupo de conmutación por error del servidor y elimina todas las bases de datos secundarias incluidas en el grupo |
| [Get-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Recupera la configuración del grupo de conmutación por error |
| [Set-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Modifica la configuración del grupo de conmutación por error |
| [Switch-AzureRMSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Desencadena la conmutación por error del grupo de conmutación por error al servidor secundario |
| [Add-AzureRmSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/add-azurermsqldatabasetofailovergroup)|Agrega una o varias bases de datos a un grupo de conmutación por error de Azure SQL Database.|
|  | |

> [!IMPORTANT]
> Para ver un script de ejemplo, consulte [Configurar y conmutar por error un grupo de conmutación por error para una sola base de datos](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

### <a name="powershell-managing-failover-groups-with-managed-instances-preview"></a>PowerShell: Administración de grupos de conmutación por error con instancias administradas (versión preliminar)

#### <a name="install-the-newest-pre-release-version-of-powershell"></a>Instalación de la versión preliminar más reciente de PowerShell

1. Actualice el módulo powershellget a 1.6.5 (o una versión preliminar más reciente). Visite el [sitio de la versión preliminar de PowerShell](https://www.powershellgallery.com/packages/AzureRM.Sql/4.11.6-preview).

   ```Powershell
      install-module PowerShellGet -MinimumVersion 1.6.5 -force
   ```

2. En una nueva ventana de PowerShell, ejecute los siguientes comandos:

   ```Powershell
      import-module PowerShellGet
      get-module PowerShellGet #verify version is 1.6.5 (or newer)
      install-module azurerm.sql -RequiredVersion 4.5.0-preview -AllowPrerelease –Force
      import-module azurerm.sql
   ```

#### <a name="powershell-commandlets-to-create-an-instance-failover-group"></a>Cmdlets de PowerShell para crear un grupo de conmutación por error de instancia

| API | DESCRIPCIÓN |
| --- | --- |
| New-AzureRmSqlDatabaseInstanceFailoverGroup |Este comando crea un grupo de conmutación por error y lo registra en el servidor principal y el servidor secundario|
| Set-AzureRmSqlDatabaseInstanceFailoverGroup |Modifica la configuración del grupo de conmutación por error|
| Get-AzureRmSqlDatabaseInstanceFailoverGroup |Recupera la configuración del grupo de conmutación por error|
| Switch-AzureRmSqlDatabaseInstanceFailoverGroup |Desencadena la conmutación por error del grupo de conmutación por error al servidor secundario|
| Remove-AzureRmSqlDatabaseInstanceFailoverGroup | Quita un grupo de conmutación por error.|

### <a name="rest-api-manage-sql-database-failover-groups-with-single-and-pooled-databases"></a>API REST: Administración de grupos de conmutación por error de base de datos SQL con bases de datos únicas y agrupadas

| API | DESCRIPCIÓN |
| --- | --- |
| [Crear o actualizar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Crea o actualiza un grupo de conmutación por error. |
| [Eliminar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Quita el grupo de conmutación por error del servidor. |
| [Conmutación por error (planeada)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Conmuta por error desde el servidor principal actual en este servidor. |
| [Forzar la conmutación por error ocasiona la pérdida de datos](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |Conmuta por error desde el servidor principal actual en este servidor. Esta operación puede ocasionar pérdida de datos. |
| [Obtener grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Obtiene un grupo de conmutación por error. |
| [Enumerar grupos de conmutación por error por servidor](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Enumera los grupos de conmutación por error de un servidor. |
| [Actualizar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Actualiza un grupo de conmutación por error. |
|  | |

### <a name="rest-api-manage-failover-groups-with-managed-instances-preview"></a>API REST: Administración de grupos de conmutación por error con instancias administradas (versión preliminar)

| API | DESCRIPCIÓN |
| --- | --- |
| [Crear o actualizar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Crea o actualiza un grupo de conmutación por error. |
| [Eliminar grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | Quita el grupo de conmutación por error del servidor. |
| [Conmutación por error (planeada)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Conmuta por error desde el servidor principal actual en este servidor. |
| [Forzar la conmutación por error ocasiona la pérdida de datos](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) |Conmuta por error desde el servidor principal actual en este servidor. Esta operación puede ocasionar pérdida de datos. |
| [Obtener grupo de conmutación por error](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | Obtiene un grupo de conmutación por error. |
| [Lista de grupos de conmutación por error: por ubicación](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Enumera los grupos de conmutación por error de una ubicación. |

## <a name="next-steps"></a>Pasos siguientes

- Para los scripts de ejemplo, vea:
  - [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Configuración y conmutación por error de un grupo de conmutación por error para una sola base de datos](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Continuidad empresarial con Base de datos SQL de Azure](sql-database-business-continuity.md)
- Para saber en qué consisten las copias de seguridad automatizadas de Azure SQL Database, consulte [Información general: copias de seguridad automatizadas de SQL Database](sql-database-automated-backups.md).
- Si quiere saber cómo usar las copias de seguridad automatizadas para procesos de recuperación, consulte [Recuperación de una base de datos a partir de copias de seguridad iniciadas por un servicio](sql-database-recovery-using-backups.md).
- Para obtener información acerca de los requisitos de autenticación para un nuevo servidor principal y la base de datos, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).
