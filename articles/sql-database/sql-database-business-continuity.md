---
title: 'Continuidad del negocio en la nube: recuperación de la base de datos (SQL Database) | Microsoft Docs'
description: Obtenga información acerca de cómo Azure SQL Database permite la continuidad del negocio en la nube y la recuperación de la base de datos, y ayuda a que las aplicaciones críticas de la nube se sigan ejecutando.
keywords: continuidad del negocio, continuidad del negocio en la nube, recuperación de desastres de la base de datos, recuperación de la base de datos
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: On Demand
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: ce0684f9ab06b5362ccdf25aeaff15ea668ce96c
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444155"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Introducción a la continuidad empresarial con Azure SQL Database

Azure SQL Database es una implementación del motor de base de datos de SQL Server estable más reciente configurada y optimizada para el entorno en la nube de Azure que proporciona [alta disponibilidad](sql-database-high-availability.md) y resistencia frente a los errores que podrían afectar a los procesos empresariales. **Continuidad empresarial** en Azure SQL Database hace referencia a los mecanismos, directivas y procedimientos que permiten a una empresa seguir funcionando en caso de interrupciones, especialmente en su infraestructura informática.  En la mayoría de los casos, Azure SQL Database controlará los eventos de interrupción que puedan ocurrir en el entorno en la nube y permitirá que se sigan ejecutando los procesos empresariales. No obstante, hay algunos eventos de interrupción que no se pueden controlar mediante SQL Database como, por ejemplo:
 - El usuario eliminó o actualizó accidentalmente una fila de una tabla.
 - Un atacante malintencionado consiguió eliminar datos o quitar una base de datos.
 - Un terremoto provocó un corte del suministro eléctrico y deshabilitó temporalmente el centro de datos.
 
Estos casos no se pueden controlar en Azure SQL Database, por lo que necesitaría usar las características de continuidad empresarial de SQL Database que le permiten recuperar los datos y mantener sus aplicaciones en funcionamiento.

En este artículo de introducción se describen las funcionalidades de continuidad empresarial y recuperación ante desastres que proporciona Azure SQL Database. Conozca más información sobre opciones, recomendaciones y tutoriales para recuperarse de eventos de interrupción que podrían provocar la pérdida de información o que su base de datos y aplicación dejen de estar disponibles. Sepa qué hacer en caso de que un error del usuario o la aplicación afecte a la integridad de los datos, se produzca una interrupción en una región de Azure o su aplicación requiera mantenimiento.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Características de SQL Database que puede utilizar para proporcionar continuidad empresarial

Desde la perspectiva de una base de datos, hay cuatro escenarios principales de posibles interrupciones:
- **Errores de hardware o software locales** que afectan al nodo de la base de datos como, por ejemplo, un error de la unidad de disco.
- **Daños o eliminación de datos**: suelen deberse a un error de la aplicación o a un error humano.  Estos errores son intrínsecamente específicos de la aplicación y, por regla general, no se pueden detectar ni mitigar automáticamente mediante la infraestructura.
- **Interrupción del centro de datos**, provocado posiblemente por un desastre natural.  Este escenario requiere cierto nivel de redundancia geográfica con conmutación por error de la aplicación en un centro de datos alternativo.
- **Errores de actualización o mantenimiento**: problemas imprevistos que se producen durante las actualizaciones o el mantenimiento de una aplicación o base de datos y que pueden requerir una rápida reversión a un estado anterior de la base de datos.

SQL Database ofrece diversas funcionalidades de continuidad empresarial, incluidas las copias de seguridad automatizadas y la replicación de base de datos opcional que pueden mitigar estos escenarios. En primer lugar, debe comprender cómo la [arquitectura de alta disponibilidad](sql-database-high-availability.md) de SQL Database proporciona una disponibilidad del 99,99% y resistencia a algunos eventos de interrupción que pueden afectar a sus procesos empresariales.
A continuación, puede obtener información acerca de los mecanismos adicionales que puede usar para recuperarse de los eventos de interrupción que no pueden controlarse mediante la arquitectura de alta disponibilidad de SQL Database, tales como:
 - [Tablas temporales](sql-database-temporal-tables.md) que le permiten restaurar versiones de fila desde cualquier momento dado.
 - Las [copias de seguridad automatizadas integradas](sql-database-automated-backups.md) y la [restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore) le permiten restaurar la base de datos completa a un momento dado de los últimos 35 días.
 - Puede [restaurar una base de datos eliminada](sql-database-recovery-using-backups.md#deleted-database-restore) al momento en que se eliminó si el **servidor lógico no se ha eliminado**.
 - La [retención de copia de seguridad a largo plazo](sql-database-long-term-retention.md) le permite conservar las copias de seguridad hasta 10 años.
 - La [replicación geográfica](sql-database-geo-replication-overview.md) permite que la aplicación realice una rápida recuperación ante desastres en el caso de que se produzca una interrupción a escala del centro de datos.

Cada una de ellas posee distintas características que abarcan los conceptos de tiempo de recuperación calculado (ERT) y pérdida de datos potencial de transacciones recientes. Cuando entienda estas opciones, puede elegir las que más relevantes considere y, en la mayoría de los escenarios, utilizarlas juntas con distintos objetivos. A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que la aplicación se recupere totalmente se conoce como el objetivo de tiempo de recuperación (RTO). También debe conocer el período máximo de actualizaciones de datos recientes (intervalo de tiempo) que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. El período de tiempo de las actualizaciones que se puede permitir perder se conoce como objetivo de punto de recuperación (RPO).

En la tabla siguiente se comparan los valores de ERT y RPO de cada nivel de servicio en los tres escenarios más comunes.

| Capacidad | Básica | Estándar | Premium  | Uso general | Crítico para la empresa
| --- | --- | --- | --- |--- |--- |
| Restauración a un momento dado a partir de una copia de seguridad |Cualquier punto de restauración en 7 días |Cualquier punto de restauración en 35 días |Cualquier punto de restauración en 35 días |Cualquier punto de restauración en el período configurado (hasta 35 días)|Cualquier punto de restauración en el período configurado (hasta 35 días)|
| Restauración geográfica de las copias de seguridad con replicación geográfica |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h|ERT < 12 h, RPO < 1 h|
| Restauración desde la retención a largo plazo de SQL |ERT < 12 h, RPO < 1 sem. |ERT < 12 h, RPO < 1 sem. |ERT < 12 h, RPO < 1 sem. |ERT < 12 h, RPO < 1 sem.|ERT < 12 h, RPO < 1 sem.|
| Replicación geográfica activa |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s|ERT < 30 s, RPO < 5 s|

## <a name="recover-a-database-to-the-existing-server"></a>Recuperación de una base de datos en el servidor existente

SQL Database realiza automáticamente una combinación de copias de seguridad completas semanales, copias de seguridad diferenciales cada hora y copias de seguridad del registro de transacciones cada 5 o 10 minutos con el fin de proteger su empresa contra la pérdida de datos. Las copias de seguridad se almacenan en un almacenamiento con redundancia geográfica con acceso de lectura durante 35 días para todos los niveles de servicio, excepto los niveles de servicio de DTU básico en el que las copias de seguridad se almacenan durante 7 días. Para más información, consulte [copias de seguridad automáticas de bases de datos](sql-database-automated-backups.md). Puede restaurar una base de datos existente a partir de las copias de seguridad automatizadas a un momento anterior en el tiempo como una nueva base de datos en el mismo servidor lógico con Azure Portal, PowerShell o la API REST. Para más información, consulte [Restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore).

Si el período de retención de la restauración a un punto del tiempo (PITR) máximo admitido no es suficiente para su aplicación, puede ampliarlo mediante la configuración de una directiva de retención a largo plazo (LTR) para las bases de datos. Para obtener más información, vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

Puede utilizar este tipo de copia de seguridad para recuperar una base de datos después de que se produzcan diferentes eventos de interrupción tanto en su centro de datos como en otro. Con las copias de seguridad automáticas de bases de datos, el tiempo estimado de recuperación depende de varios factores, como el número total de bases de datos que se están recuperando a la vez en la misma región, el tamaño de estas, el tamaño del registro de transacciones y el ancho de banda de red. Normalmente, el tiempo de recuperación es inferior a 12 horas. La recuperación de una base de datos muy grande o activa puede tardar más tiempo. Para más información sobre el tiempo de recuperación, consulte el apartado sobre el [tiempo de recuperación de bases de datos](sql-database-recovery-using-backups.md#recovery-time). Cuando se lleva a cabo un proceso de recuperación en otra región de datos, la posible pérdida de datos solo es de 1 hora gracias al almacenamiento con redundancia geográfica de las copias de seguridad diferenciales de bases de datos que se realizan cada hora.

Utilice las copias de seguridad automatizadas y la [restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore) como mecanismo de recuperación y de continuidad empresarial si se cumplen los siguientes requisitos en su aplicación:

* No se considera crítica.
* No tiene un Acuerdo de Nivel de Servicio vinculante, por lo que no incurrirá en responsabilidades financieras en caso de tiempos de inactividad de 24 horas o más.
* Tiene una tasa de cambio de datos reducida (bajo número de transacciones por hora) y se tolera perder hasta una hora de datos.
* Los costos son un factor fundamental.

Si necesita recuperaciones más rápidas, utilice la [replicación geográfica activa](sql-database-geo-replication-overview.md) (se describe a continuación). Si necesita recuperar datos de un período anterior a 35 días, use la [retención a largo plazo](sql-database-long-term-retention.md). 

## <a name="recover-a-database-to-another-region"></a>Recuperación de una base de datos en otra región
<!-- Explain this scenario -->

Aunque es poco habitual, en los centros de datos de Azure pueden producirse interrupciones. Cuando esto ocurre, provoca también una interrupción en el negocio que podría extenderse solo unos pocos minutos o, incluso, horas.

* Una opción consiste en esperar a que la base de datos vuelva a estar en línea cuando termine la interrupción del centro de datos. Esto puede hacerse con las aplicaciones que pueden permitirse que la base de datos esté desconectada. Por ejemplo, los proyectos de desarrollo o las pruebas gratuitas no tienen que estar en funcionamiento constantemente. Cuando se produce una interrupción en un centro de datos, no sabe cuánto durará, por lo que esta opción solo es útil si no necesita utilizar la base de datos durante un tiempo.
* Otra opción consiste en restaurar una base de datos en cualquier servidor de cualquier región de Azure mediante [copias de seguridad de base de datos con redundancia geográfica](sql-database-recovery-using-backups.md#geo-restore) (restauración geográfica). La funcionalidad de restauración geográfica utiliza una copia de seguridad con redundancia geográfica como origen y se puede usar para recuperar una base de datos, aunque no se pueda acceder a dicha base de datos o al centro de datos debido a una interrupción.
* Por último, puede promocionar rápidamente una base de datos secundaria en otra región de datos para que se convierta en principal (proceso también denominado conmutación por error) y configurar las aplicaciones para que se conecten a la base de datos promocionada principal si va a usar la replicación geográfica activa. Puede que se pierda una pequeña cantidad de datos en las transacciones muy recientes debido a la naturaleza de la replicación asincrónica. Con el uso de grupos de conmutación automática por error, puede personalizar la directiva de conmutación por error para minimizar la posible pérdida de datos. En todos los casos, los usuarios experimentan un tiempo de inactividad reducido y tienen que volver a conectarse. La conmutación por error solo lleva unos segundos, mientras que la recuperación de una base de datos a partir de copias de seguridad dura horas.

Para conmutar por error a otra región, puede usar la [replicación geográfica activa](sql-database-geo-replication-overview.md) para hacer que una base de datos tenga un máximo de cuatro bases de datos secundarias legibles en las regiones que prefiera. Estas bases de datos secundarias se mantienen sincronizadas con la principal a través de un mecanismo de replicación asincrónica. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>


> [!IMPORTANT]
> Para usar grupos de conmutación automática por error y la replicación geográfica activa, debe ser el propietario de la suscripción o tener permisos administrativos en SQL Server. Puede realizar tareas de configuración y conmutación por error mediante Azure Portal, PowerShell o la API de REST con los permisos de la suscripción de Azure, o bien mediante Transact-SQL con los permisos de SQL Server.
> 

Esta característica se utiliza para proteger su negocio de interrupciones en el centro de datos o durante una actualización de la aplicación. Para habilitar la conmutación por error automática y transparente debe organizar las bases de datos de replicación geográfica en grupos mediante la característica [auto-failover group](sql-database-geo-replication-overview.md) de grupos de conmutación automática por error de SQL Database. Use la replicación geográfica activa y los grupos de conmutación automática por error si su aplicación cumple alguno de estos criterios:

* Es crítica.
* Tiene un SLA que no se permite que se produzcan tiempos de inactividad de más de 24 horas.
* El tiempo de inactividad incurrirá en responsabilidades financieras.
* Tiene una tasa de cambio de datos elevada y no se acepta perder una hora de datos.
* El costo adicional por utilizar la replicación geográfica activa es menor que el de la posible responsabilidad financiera y la pérdida de negocio asociada que habría que asumir.

Cuando pase a la acción, el tiempo que se tarde en recuperar las bases de datos y la cantidad de información que se pierde en el caso de una interrupción en el centro de datos dependerá de cómo decida usar las características de continuidad empresarial de la aplicación. De hecho, puede decidir usar una combinación de copias de seguridad de bases de datos y la replicación geográfica activa según los requisitos de la aplicación. Para ver una explicación de las consideraciones de diseño de las aplicaciones para bases de datos independientes y grupos elásticos con estas características de continuidad empresarial, consulte [Diseño de una aplicación para la recuperación ante desastres](sql-database-designing-cloud-solutions-for-disaster-recovery.md) y [Estrategias de recuperación ante desastres para aplicaciones que usan el grupo elástico](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

En las siguientes secciones se ofrece información general de los pasos para realizar tareas de recuperación mediante copias de seguridad de bases de datos o la replicación geográfica activa. Para ver los pasos detallados, como el planeamiento de los requisitos, los pasos posteriores a la recuperación y los detalles sobre cómo simular una interrupción para llevar a cabo tareas de recuperación ante desastres, consulte el artículo [Recuperación de una instancia de SQL Database después de una interrupción](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Preparativos para interrupciones
Con independencia de la característica de continuidad empresarial que use, debe hacer lo siguiente:

* Identificar y preparar el servidor de destino, incluidas las reglas de firewall de nivel de servidor, los inicios de sesión y los permisos de nivel de base de datos maestra.
* Determinar cómo se redirigirán los clientes y las aplicaciones cliente al nuevo servidor
* Documentar otras dependencias, como las alertas y la configuración de auditoría

Si no se prepara correctamente, el proceso de conectar las aplicaciones después de una conmutación por error o una recuperación de base de datos llevará más tiempo y, probablemente, también haya que solucionar problemas en momentos de estrés, por lo que no es nada recomendable.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Conmutación por error de la base de datos secundaria de replicación geográfica
Si usa grupos de conmutación automática por error y la replicación geográfica activa como el mecanismo de recuperación, puede configurar una directiva de conmutación automática por error o usar la [conmutación por error manual](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Tras iniciar la conmutación por error, la base de datos secundaria pasa a ser la principal y está lista para registrar nuevas transacciones y responder a consultas, con una pérdida mínima de los datos que aún no se han replicado. Para obtener información sobre el diseño del proceso de conmutación por error, vea [Diseño de aplicaciones para la recuperación ante desastres en la nube](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Cuando el centro de datos vuelve a estar en línea, las bases de datos principales anteriores se vuelven a conectar automáticamente con la nueva base de datos principal y se convierten en bases de datos secundarias. Si necesita reubicar la base de datos principal de nuevo en la región original, puede iniciar una conmutación por error manual planificada (conmutación por recuperación). 
> 

### <a name="perform-a-geo-restore"></a>Realización de restauraciones geográficas
Si utiliza copias de seguridad automatizadas con replicación de almacenamiento con redundancia geográfica como mecanismo de recuperación, [inicie una recuperación de base de datos mediante la restauración geográfica](sql-database-disaster-recovery.md#recover-using-geo-restore). El proceso de recuperación suele durar 12 horas y hay una pérdida de datos de hasta una hora en función de cuándo se realizó y replicó la última copia de seguridad diferencial que se ejecuta cada hora. Hasta que no se complete la recuperación, la base de datos no puede registrar transacciones ni responder a las consultas. Aunque esto permite restaurar una base de datos hasta el último punto disponible en el tiempo, la restauración de una base de datos geográfica secundaria a cualquier punto en el tiempo no se admite actualmente.

> [!NOTE]
> Si el centro de datos vuelve a estar en línea antes de cambiar la aplicación en la base de datos recuperada, puede cancelar el proceso de recuperación.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Realización de tareas posteriores a la recuperación y conmutación por error
Cuando efectúe la recuperación con cualquiera de los mecanismos para llevarla a cabo, debe realizar las siguientes tareas adicionales antes de que los usuarios y las aplicaciones vuelvan a conectarse:

* Redirija los clientes y las aplicaciones cliente al nuevo servidor y a la base de datos restaurada.
* Asegúrese de aplicar reglas de firewall de nivel de servidor adecuadas para que se conecten los usuarios (o use [firewalls de nivel de base de datos](sql-database-firewall-configure.md#creating-and-managing-firewall-rules)).
* No se olvide de emplear permisos de nivel de base de datos maestra e inicios de sesión apropiados (o use [usuarios contenidos](https://msdn.microsoft.com/library/ff929188.aspx)).
* Configure la auditoría según corresponda.
* Configure las alertas según corresponda.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Actualice una aplicación con el mínimo tiempo de inactividad posible.
A veces, debe desconectar una aplicación debido a un mantenimiento planeado, por ejemplo, para actualizarla. En el artículo [Administración de actualizaciones graduales de aplicaciones](sql-database-manage-application-rolling-upgrade.md) se explica cómo utilizar la replicación geográfica activa para poder actualizar gradualmente su aplicación en la nube con el objetivo de minimizar el tiempo de inactividad durante este proceso. Además, se describe una ruta de recuperación para cuando se empiece a detectar errores. 

## <a name="next-steps"></a>Pasos siguientes
Para ver una explicación de las consideraciones de diseño de las aplicaciones para bases de datos independientes y grupos elásticos, consulte [Diseño de una aplicación para la recuperación ante desastres en la nube](sql-database-designing-cloud-solutions-for-disaster-recovery.md) y [Estrategias de recuperación ante desastres para aplicaciones que usan el grupo elástico](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
