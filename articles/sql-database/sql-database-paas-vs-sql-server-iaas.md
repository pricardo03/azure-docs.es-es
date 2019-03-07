---
title: Selección de las opciones de SQL en Azure | Microsoft Docs
description: Obtenga información sobre cómo elegir entre las opciones de implementación en Azure SQL Database y entre Azure SQL Database y SQL Server en máquinas virtuales de Azure
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
keywords: Nube de SQL Server, SQL Server en la nube, base de datos de PaaS, DBaaS
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: ff0589b87ae031db5bda96dc238359bf9cfb3b11
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55751606"
---
# <a name="choose-the-right-sql-server-option-in-azure"></a>Selección de la opción de SQL Server correcta en Azure

En Azure, puede hacer que las cargas de trabajo de SQL Server se ejecuten en una infraestructura hospedada (IaaS) o como un servicio hospedado ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)). En PaaS, tiene varias opciones de implementación y niveles de servicio dentro de cada opción de implementación. La pregunta clave que debe hacerse al decidir entre PaaS o IaaS es si quiere administrar su base de datos, aplicar parches o realizar copias de seguridad usted mismo; o si quiere delegar estas operaciones a Azure.
Dependiendo de la respuesta, tiene las siguientes opciones:

- [Azure SQL Database](sql-database-technical-overview.md): Un motor de SQL Database totalmente administrado, basado en la última edición Enterprise estable de SQL Server. Se trata de una base de datos relacional como servicio (DBaaS), hospedada en la nube de Azure, que se engloba en la categoría del sector denominada *Plataforma como servicio (PaaS)*. La base de datos SQL tiene varias opciones de implementación, cada una de las cuales se compila en hardware y software estandarizados que Microsoft posee, hospeda y mantiene. Con SQL Database, puede usar la funcionalidad y las características integradas que requieren configuración extensiva en SQL Server (ya sea en el entorno local o en una máquina virtual de Azure). Al utilizar SQL Database, se emplea el método de pago por uso de opciones para escalar vertical u horizontalmente a fin de aumentar la potencia de forma ininterrumpida. SQL Database incluye características adicionales que no están disponibles en SQL Server, como la alta disponibilidad, la administración y la inteligencia integradas. Azure SQL Database ofrece las siguientes opciones de implementación:
  
  - Como una [base de datos única](sql-database-single-database.md) con su propio conjunto de recursos administrados mediante un servidor de SQL Database. Una base de datos única es similar a una [base de datos independiente](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) en SQL Server. Esta opción está optimizada para el desarrollo de las nuevas aplicaciones modernas procedentes de la nube.
  - Un [grupo elástico](sql-database-elastic-pool.md), que es una colección de bases de datos con un conjunto compartido de recursos administrados a través de un servidor de SQL Database. Las bases de datos únicas se pueden mover dentro y fuera de un grupo elástico. Esta opción está optimizada para el desarrollo de las nuevas aplicaciones modernas procedentes de la nube mediante la aplicación SaaS multiinquilino.
  - [Instancia administrada](sql-database-managed-instance.md), que es una colección de bases de datos de usuario y del sistema con un conjunto compartido de recursos. Una instancia administrada es similar a una instancia del [motor de base de datos de Microsoft SQL Server], por lo que ofrece recursos compartidos para las bases de datos y funciones de ámbito de instancia adicionales. La Instancia administrada admite la migración de bases de datos desde instancias locales con mínimos cambios en las bases de datos o ninguno. Esta opción proporciona todas las ventajas de PaaS de Azure SQL Database, pero incluye funcionalidades que anteriormente solo estaban disponibles en las máquinas virtuales de SQL. Esto incluye una red virtual nativa (VNet) y un nivel próximo al 100 % de compatibilidad con la instancia local de SQL Server.
- [SQL Server en Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) que se engloba en la categoría del sector denominada *Infraestructura como servicio (IaaS)* y permite ejecutar SQL Server en una máquina virtual totalmente administrada en la nube de Azure. Las [máquinas virtuales de SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) también se ejecutan en hardware estandarizado que Microsoft posee, hospeda y mantiene. Cuando se usa SQL Server en una máquina virtual, puede usar la licencia de pago por uso de SQL Server ya incluida en una imagen de SQL Server o usar fácilmente una licencia existente. También puede detener o reanudar la máquina virtual según sea necesario. SQL Server instalado y hospedado en la nube en máquinas virtuales (VM) de Windows Server o Linux que se ejecutan en Azure, conocido también como infraestructura como servicio (IaaS). SQL Server en Azure Virtual Machines es una buena opción para migrar aplicaciones y bases de datos de SQL Server locales sin realizar ningún cambio en las bases de datos. Todas las versiones y ediciones recientes de SQL Server están disponibles para la instalación en una máquina virtual de IaaS. La diferencia más importante de SQL Database es que las máquinas virtuales de SQL Server permiten un control total sobre el motor de base de datos. Puede elegir cuándo se iniciará el mantenimiento o la revisión, cambiar el modelo de recuperación de registro simple o masivo para permitir un registro sin carga más rápido, pausar o iniciar el motor cuando sea necesario y personalizar completamente el motor de base de datos de SQL Server. Este control adicional incluye la responsabilidad agregada de administrar las máquinas virtuales.

Las principales diferencias entre estas opciones se mencionan en la siguiente tabla:

| SQL Server en máquinas virtuales | Instancia administrada en SQL Database | Base de datos única y grupo de bases de datos elásticas en SQL Database |
| --- | --- | --- |
|Tiene control total sobre el motor de SQL Server.<br/>Disponibilidad de hasta un 99,95 %.<br/>Paridad completa con la versión correspondiente de una instancia local de SQL Server.<br/>Versión del motor de base de datos corregida y conocida.<br/>Migración sencilla desde una instancia local de SQL Server.<br/>Dirección IP privada dentro de la red virtual de Azure.<br/>Tiene la capacidad de implementar aplicaciones o servicios en el host donde se sitúa la instancia de SQL Server.| Alta compatibilidad con instancias locales de SQL Server.<br/>99,99 % de disponibilidad garantizada.<br/>Copias de seguridad, revisiones y acciones de recuperación integradas.<br/>Versión más reciente y estable del motor de base de datos.<br/>Migración sencilla desde SQL Server.<br/>Dirección IP privada dentro de la red virtual de Azure.<br/>Seguridad e inteligencia avanzadas e integradas.<br/>Cambio en línea de recursos (CPU y almacenamiento).|Las características de SQL Server más utilizadas están disponibles.<br/>99,99 % de disponibilidad garantizada.<br/>Copias de seguridad, revisiones y acciones de recuperación integradas.<br/>Versión más reciente y estable del motor de base de datos.<br/>Capacidad para asignar los recursos necesarios (CPU y almacenamiento) a bases de datos individuales.<br/>Seguridad e inteligencia avanzadas e integradas.<br/>Cambio en línea de recursos (CPU y almacenamiento).|
|Deberá administrar usted mismo las copias de seguridad y revisiones.<br>Deberá implementar su propia solución de alta disponibilidad.<br/>Existe un tiempo de inactividad mientras se cambian los recursos (CPU y almacenamiento)|Sigue habiendo un número mínimo de características de SQL Server que no están disponibles.<br/>El tiempo exacto de mantenimiento no se garantiza (pero es casi transparente).<br/>La compatibilidad con la versión de SQL Server se puede lograr solo con los niveles de compatibilidad de la base de datos.|La migración desde SQL Server puede resultar difícil.<br/>Algunas características de SQL Server no están disponibles.<br/>El tiempo exacto de mantenimiento no se garantiza (pero es casi transparente).<br/>La compatibilidad con la versión de SQL Server se puede lograr solo con los niveles de compatibilidad de la base de datos.<br/>No se puede asignar ninguna dirección IP privada (puede limitar el acceso mediante reglas de firewall).|

Obtenga información acerca de qué opción de implementación se ajusta a la plataforma de datos de Microsoft y consiga ayuda sobre la opción más adecuada para sus requisitos empresariales. Si asigna mayor prioridad al ahorro o bien antepone la mínima administración a todo lo demás, este artículo puede ayudarle a decidir el enfoque correcto, en función de los requisitos empresariales que más le preocupan.

## <a name="microsofts-sql-data-platform"></a>Plataforma de datos de Microsoft SQL

Una de las primeras cosas que hay que comprender al comparar Azure con bases de datos SQL Server locales es que puede usarlas todas. La plataforma de datos de Microsoft aprovecha la tecnología de SQL Server y la pone a disposición de los usuarios en máquinas físicas locales, entornos en nubes privadas, entornos en nubes privadas hospedados por terceros y la nube pública. SQL Server en máquinas virtuales de Azure permite satisfacer necesidades únicas y diversas del negocio gracias a una combinación de implementaciones locales y hospedadas en la nube, usando el mismo conjunto de productos de servidor, herramientas de desarrollo y conocimientos en todos estos entornos.

   ![Opciones de SQL Server en la nube: SQL Server en IaaS o SQL Database de SaaS en la nube.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Como se ve en el diagrama, cada oferta puede caracterizarse por el nivel de administración que se tiene sobre la infraestructura (en el eje X) y el grado de relación coste-eficacia obtenido mediante la automatización y la consolidación en el nivel de base de datos (en el eje Y).

Al diseñar una aplicación, existen cuatro opciones básicas para el hospedaje de la parte de SQL Server de la aplicación:

- SQL Server en máquinas físicas no virtualizadas
- SQL Server en máquinas locales virtualizadas (nube privada)
- SQL Server en máquinas virtuales de Azure (nube pública de Microsoft)
- Azure SQL Database (nube pública de Microsoft)

En las secciones siguientes, obtendrá información sobre SQL Server en la nube pública de Microsoft: Azure SQL Database y SQL Server en VM de Azure. Además, explorará factores de motivación comunes del negocio para determinar la opción que mejor funciona para su aplicación.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL Database y SQL Server en Azure Virtual Machines en detalle

Por lo general, estas dos opciones SQL se optimizan con diferentes fines:

- **Azure SQL Database**

Optimizado para reducir al mínimo los costos generales de administración para el aprovisionamiento y la administración de muchas bases de datos. Reduce los costos corrientes de administración porque no es necesario administrar máquinas virtuales, el sistema operativo ni el software de base de datos. No es necesario administrar actualizaciones, alta disponibilidad ni [copias de seguridad](sql-database-automated-backups.md). Habitualmente, Azure SQL Database puede aumentar considerablemente el número de bases de datos administradas por un solo recurso de TI o de desarrollo. Los [grupos elásticos](sql-database-elastic-pool.md) también admiten arquitecturas de aplicaciones SaaS multiinquilino con características como el aislamiento de inquilinos y la capacidad de escalado para reducir costos al compartir recursos entre las bases de datos. [Instancia administrada ](sql-database-managed-instance.md) proporciona compatibilidad con características de ámbito de instancia, lo que facilita la migración de las aplicaciones existentes, así como el uso compartido de recursos entre las bases de datos.

- **Ejecución de SQL Server en Azure Virtual Machines**

Optimizada para migrar las aplicaciones existentes a Azure o para llevar las aplicaciones locales existentes a la nube en implementaciones híbridas. Además, puede usar SQL Server en una máquina virtual para desarrollar y probar las aplicaciones tradicionales de SQL Server. Con SQL Server en Azure Virtual Machines, tiene todos los derechos administrativos sobre una instancia dedicada de SQL Server y una máquina virtual basada en la nube. Es la elección perfecta cuando una organización ya dispone de recursos de TI para mantener las máquinas virtuales. Estas funcionalidades permiten crear un sistema muy personalizado para abordar los requisitos de rendimiento y disponibilidad específicos de la aplicación.

En la siguiente tabla se resumen las principales características de SQL Database y SQL Server en Azure Virtual Machines:

| | Bases de datos únicas y grupos elásticos de SQL Database | Instancias administradas de SQL Database |Máquinas virtuales de Azure con SQL Server |
| --- | --- | --- |---|
| **Más adecuado para:** |Nuevas aplicaciones diseñadas para la nube que pretenden usar las características estables de SQL Server más recientes y que tienen restricciones de tiempo en las actividades de desarrollo y marketing. | Nuevas aplicaciones o aplicaciones locales existentes que pretenden usar las características estables de SQL Server más recientes y que se han migrado a la nube con cambios mínimos.  | Aplicaciones existentes que requieren una rápida migración a la nube con mínimos cambios o ninguno. Escenarios de desarrollo rápido y pruebas cuando no se desea comprar hardware de SQL Server de no producción local. |
|  | Equipos que requieren mecanismos integrados de alta disponibilidad, recuperación ante desastres y actualización. | Lo mismo que las bases de datos agrupadas y únicas de SQL Database. | Equipos que puedan configurar, ajustar, personalizar y administrar una alta disponibilidad, la recuperación ante desastres y la aplicación de revisiones para SQL Server. Algunas funciones automatizadas ya incorporadas simplifican considerablemente estos aspectos. | |
|  | Equipos que no desean administrar el sistema operativo subyacente ni los valores de configuración. | Lo mismo que las bases de datos agrupadas y únicas de SQL Database. | Necesita un entorno personalizado con derechos administrativos completos. | |
|  | Bases de datos de hasta 100 TB. | Hasta 8 TB. | Instancias de SQL Server con hasta 64 TB de almacenamiento. La instancia puede admitir tantas bases de datos como sea necesario. |
| **Compatibilidad** | Admite la mayoría de las funcionalidades de nivel de base de datos locales. | Admite casi todas las funcionalidades de nivel de instancia y nivel de base de datos locales. | Admite todas las funcionalidades locales. |
| **Recursos:** | No desea emplear recursos de TI para la configuración y administración de la infraestructura subyacente, sino centrarse en el nivel de aplicación. | Lo mismo que las bases de datos agrupadas y únicas de SQL Database. | Tiene algunos recursos de TI para la configuración y administración. Algunas funciones automatizadas ya incorporadas simplifican considerablemente estos aspectos. |
| **Costo total de propiedad:** | Elimina los costos de hardware y reduce los costos administrativos. | Lo mismo que las bases de datos agrupadas y únicas de SQL Database. | Elimina costes de hardware. |
| **Continuidad del negocio:** |Además de las [funcionalidades integradas de infraestructura de tolerancia a errores](sql-database-high-availability.md), Azure SQL Database proporciona características que aumentan la continuidad empresarial, como, por ejemplo, [copias de seguridad automatizadas](sql-database-automated-backups.md), [restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore), [restauración geográfica](sql-database-recovery-using-backups.md#geo-restore), [replicación geográfica activa](sql-database-active-geo-replication.md) y [grupos de conmutación por error](sql-database-auto-failover-group.md). Para más información, consulte [Información general: continuidad del negocio en la nube y recuperación ante desastres con la SQL Database](sql-database-business-continuity.md). | Lo mismo que las bases de datos agrupadas y únicas de SQL Database; además, están disponibles las copias de seguridad que inició el usuario y de solo copia. | SQL Server en máquinas virtuales de Azure permite configurar una solución de recuperación ante desastres y alta disponibilidad para las necesidades específicas de su base de datos. Por consiguiente, podrá tener un sistema altamente optimizado para la aplicación. Podrá probar y ejecutar conmutaciones por error cuando sea necesario. Para más información, consulte [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Nube híbrida:** |La aplicación local puede obtener acceso a datos de Azure SQL Database. | [Implementación nativa de redes virtuales](sql-database-managed-instance-vnet-configuration.md) y conectividad al entorno local mediante Azure ExpressRoute o VPN Gateway. | Con SQL Server en Máquinas virtuales de Azure, se pueden tener aplicaciones que se ejecuten parcialmente en la nube y parcialmente en la instalación local. Por ejemplo, se puede ampliar la red local y el Dominio de Active Directory a la nube mediante [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Para obtener más información sobre las soluciones de la nube híbrida, consulte [Ampliación de las soluciones de datos locales a la nube](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud). |
|  | Admite la [replicación transaccional de SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) como suscriptor para replicar los datos. | La replicación es compatible con la Instancia administrada como característica en vista previa (GB). | Es totalmente compatible con la [replicación transaccional de SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), los [grupos de disponibilidad AlwaysOn](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services y trasvase de registros para replicar los datos. Además, las copias de seguridad de SQL Server tradicionales son totalmente compatibles. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Motivaciones empresariales al elegir Azure SQL Database o SQL Server en Azure Virtual Machines

Hay varios factores que pueden influir a la hora de elegir PaaS o IaaS para hospedar las bases de datos SQL:

- [Costo](#cost): ambas opciones, PaaS e IaaS, incluyen un precio base que cubre la infraestructura subyacente y la concesión de licencias. Sin embargo, con la opción IaaS deberá invertir más tiempo y recursos para administrar la base de datos, mientras que con PaaS podrá disfrutar de estas características de administración por el mismo precio. La opción IaaS permite cerrar los recursos cuando no los esté usando para reducir el costo, mientras que la versión de PaaS se ejecutará siempre, a menos que quite los recursos y vuelva a crearlos según sea necesario.
- [Administración](#administration): las opciones PaaS reducen la cantidad de tiempo que necesita invertir para administrar la base de datos. Sin embargo, también impiden realizar algunas tareas de administración personalizadas que podrían mejorar el rendimiento de su carga de trabajo.
- [Acuerdo de nivel de servicio](#service-level-agreement-sla): tanto IaaS como PaaS proporcionan SLA altos y estándar en el sector. La opción PaaS garantiza un SLA del 99,99 %, mientras que IaaS garantiza un SLA del 99,95 % para la infraestructura, lo que significa que deberá implementar mecanismos adicionales para garantizar la disponibilidad de las bases de datos. En el caso extremo, si quiere implementar una solución de alta disponibilidad que coincida con PaaS, deberá crear una instancia adicional de SQL Server en la máquina virtual y configurar grupos de disponibilidad AlwaysOn, que podrían duplicar el costo de la base de datos.
- [Hora de pasar a Azure](#market): SQL Server en la máquina virtual de Azure es la coincidencia exacta del entorno, por lo que la migración desde una instancia local a la máquina virtual de SQL Azure no difiere de mover las bases de datos de un servidor local a otro. Instancia administrada también permite una migración muy sencilla. Sin embargo, puede que sea necesario aplicar algunos cambios antes de migrar a una instancia administrada.

Estos factores se tratarán con más detalle en las secciones siguientes.

### <a name="cost"></a>Coste

Si se encuentra en una startup con falta de medios o en un equipo de una compañía bien establecida que opera con restricciones presupuestarias, los fondos limitados suelen ser el factor principal a la hora de decidir cómo hospedar la base de datos. En esta sección, obtendrá información relativa a los conceptos básicos sobre facturación y licencias en Azure con respecto a estas dos opciones de bases de datos relacionales: Azure SQL Database y SQL Server en VM de Azure. También aprenderá a calcular el costo total de la aplicación.

#### <a name="billing-and-licensing-basics"></a>Conceptos básicos sobre facturación y licencias

Actualmente, **SQL Database** se vende como un servicio y está disponible con varias opciones de implementación y en varios niveles de servicio con distintos precios para los recursos, que se facturan por hora a una tarifa fija en función del nivel de servicio y el tamaño de proceso que se elija. Para acceder a la información más reciente sobre los niveles de servicio, los tamaños de proceso y las cantidades de almacenamiento admitidos actualmente, consulte los artículos sobre el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).

- Con una base de datos única de SQL Database, puede elegir un nivel de servicio que se adapte a sus necesidades de entre una amplia gama de precios a partir de 5 USD/mes para el nivel básico.
- Puede crear [grupos elásticos](sql-database-elastic-pool.md) para compartir recursos entre las instancias de la base de datos con la finalidad de reducir costos y admitir picos de uso.
- Puede usar su propia licencia con la instancia administrada de Azure SQL Database. Para obtener más información sobre la incorporación de licencias propias, consulte [Movilidad de licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/) o use la [calculadora de Ventaja híbrida de Azure ](https://azure.microsoft.com/pricing/hybrid-benefit/#sql-database) para saber cómo **ahorrar hasta un 40 %**.

Además, se le facturará el tráfico saliente de Internet en las [tasas de transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/)habituales. Los niveles de servicio y los tamaños de proceso se pueden ajustar de forma dinámica para satisfacer las necesidades variables de rendimiento de la aplicación.

Con **SQL Database**, Microsoft configura, revisa y actualiza automáticamente el software de base de datos, lo que reduce los costos de administración. Además, sus capacidades de [copia de seguridad integrada](sql-database-automated-backups.md) ayudan a obtener un ahorro significativo, sobre todo, cuando se tiene gran cantidad de base de datos.

Con **SQL Server en máquinas virtuales de Azure**, también puede usar cualquiera de las imágenes de SQL Server que proporciona la plataforma (que incluye una licencia) o incorporar su licencia de SQL Server. Todas las versiones de SQL Server (2008 R2, 2012, 2014, 2016) y ediciones (Developer, Express, Web, Standard, Enterprise) compatibles están disponibles. Además, hay disponibles versiones “traiga su propia licencia” (BYOL) de las imágenes. Cuando se usan las imágenes suministradas por Azure, el costo operativo depende del tamaño de la máquina virtual, así como de la edición de SQL Server que elija. Independientemente del tamaño de la máquina virtual o la edición de SQL Server, se paga el costo de licencia por minuto de SQL Server y Windows o Linux Server, junto con el costo de Azure Storage para los discos de la máquina virtual. La opción de facturación por minuto permite utilizar SQL Server durante el tiempo que sea necesario sin comprar licencias adicionales de SQL Server. Si incorpora su propia licencia de SQL Server a Azure, solo se cobran los costos de servidor y de almacenamiento. Para obtener más información sobre la incorporación de licencias propias, consulte [Movilidad de Licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/). Además, se le facturará el tráfico saliente de Internet en las [tasas de transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/)habituales.

#### <a name="calculating-the-total-application-cost"></a>Cálculo del coste total de la aplicación

Cuando se comienza a usar una plataforma en la nube, el coste de ejecución de la aplicación incluye los costos de nuevo desarrollo y de administración continua, además de los costos de servicio de la plataforma en la nube pública.

**Cuándo se usa Azure SQL Database:**

- Costos de administración altamente minimizados
- Costos de desarrollo limitados para aplicaciones migradas (instancias administradas)
- Costos de servicio de SQL Database
- No hay costos de compras de hardware

**Cuándo se usa SQL Server en Máquinas virtuales de Azure:**

- Costos de administración más altos
- Costos de desarrollo de limitados a inexistentes para aplicaciones migradas
- Costos de servicio de Virtual Machines
- No hay costos de compras de hardware

Para obtener más información sobre precios, consulte los siguientes recursos:

- [Precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
- [Precios de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/) para [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Administración

En muchas empresas, la decisión de pasar a un servicio en la nube está tan relacionada con la posibilidad de reducir la carga de complejidad de administración, como con el costo. Con IaaS y PaaS, Microsoft administra la infraestructura subyacente y replica automáticamente todos los datos para proporcionar recuperación ante desastres, configura y actualiza el software de base de datos, administra el equilibrio de carga y realiza una conmutación por error transparente en caso de error del servidor en un centro de datos.

- Con **Azure SQL Database**, puede seguir administrando la base de datos, pero ya no necesita administrar el motor de la base de datos, el sistema operativo ni el hardware. Las bases de datos y los inicios de sesión, el ajuste de índices y consultas, así como la auditoría y la seguridad, son ejemplos de elementos que puede seguir administrando. Además, la configuración de la alta disponibilidad en otro centro de datos requiere tareas de configuración y administración mínimas.
- Con **SQL Server en máquinas virtuales de Azure**, tiene un control completo sobre la configuración del sistema operativo y de la instancia de SQL Server. Con una máquina virtual, usted decide cuándo actualizar el software del sistema operativo y de la base de datos, y cuándo instalar cualquier otro software adicional, por ejemplo, un antivirus. Se proporcionan algunas características automatizadas para simplificar considerablemente la aplicación de revisiones, las copias de seguridad y la alta disponibilidad. Además, se puede controlar el tamaño de la máquina virtual, el número de discos y sus configuraciones de almacenamiento. Azure permite cambiar el tamaño de una máquina virtual cuando sea necesario. Para obtener más información, consulte [Tamaños de máquinas virtuales](../virtual-machines/windows/sizes.md).

### <a name="service-level-agreement-sla"></a>Contrato de nivel de servicio (SLA)

Para algunos departamentos de TI, cumplir las obligaciones de tiempo de actividad de un contrato de nivel de servicio (SLA) es una prioridad máxima. En esta sección, analizaremos a qué se aplica un contrato de nivel de servicio para cada opción de hospedaje de base de datos.

Para **SQL Database**, Microsoft proporciona un SLA del 99,99 % de disponibilidad. Para obtener la información más reciente, consulte [Contrato de nivel de servicio para Base de datos SQL](https://azure.microsoft.com/support/legal/sla/sql-database/).

Para **Ejecución de SQL Server en Máquinas virtuales de Azure**, Microsoft proporciona un contrato de nivel de servicio de disponibilidad del 99,95 % que cubre solo la máquina virtual. Este contrato no cubre los procesos (como SQL Server) que se ejecutan en la máquina virtual y requieren que se hospeden como mínimo dos instancias de máquina virtual en un conjunto de disponibilidad. Para obtener la información más reciente, consulte [Contrato de nivel de servicio para Máquinas virtuales](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Para alta disponibilidad (HA) de base de datos en las máquinas virtuales, se debe configurar una de las opciones de alta disponibilidad admitidas en SQL Server, como [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). El uso de una opción de alta disponibilidad no proporciona un SLA adicional, pero le permite lograr una disponibilidad de base de datos del 99,99 %.

### <a name="market"></a>Es hora de cambiar a Azure

**Las bases de datos únicas o los grupos elásticos de SQL Database** son la solución ideal para aplicaciones diseñadas en la nube cuando la productividad del desarrollador y un plazo de comercialización rápido son factores críticos. Con una funcionalidad de tipo DBA mediante programación, resulta perfecto para arquitectos y desarrolladores en la nube puesto que reduce la necesidad de administrar el sistema operativo y la base de datos subyacentes.

**Instancia administrada de SQL Database** simplifica en gran medida la migración de aplicaciones existentes a Azure SQL Database, lo que le permite comercializar aplicaciones de base de datos migradas rápidamente en Azure.

**SQL Server en ejecución en Azure Virtual Machines** es perfecto si las aplicaciones nuevas o existentes requieren bases de datos grandes o acceso a todas las características de SQL Server o Windows/Linux, y quiere ahorrarse el tiempo y el gasto de adquirir nuevo hardware local. También es una buena opción si quiere migrar aplicaciones y bases de datos locales existentes a Azure tal cual, en casos en que la instancia administrada de Azure SQL Database no sea una opción adecuada. Dado que no es necesario cambiar los niveles de presentación, aplicación y datos, se ahorra tiempo y presupuesto en renovar la arquitectura de la solución existente. En su lugar, puede centrarse en migrar todas las soluciones a Azure y realizar algunas optimizaciones del rendimiento que requiere la plataforma de Azure. Para obtener más información, consulte [Procedimientos recomendadas para mejorar el rendimiento para SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="next-steps"></a>Pasos siguientes

- Consulte [cómo comenzar por primera vez con Azure SQL Database](sql-database-single-database-get-started.md).
- Consulte [Precio de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Consulte [Aprovisionamiento de una máquina virtual de SQL Server en Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) para una introducción a SQL Server en máquinas virtuales de Azure.
