---
title: Base de datos (PaaS) SQL y  SQL Server en la nube en máquinas virtuales (IaaS) | Microsoft Docs
description: 'Obtenga información acerca de qué opción de SQL Server en la nube se ajusta a su aplicación: Base de datos (PaaS) SQL de Azure o SQL Server en la nube en Azure Virtual Machines.'
services: sql-database, virtual-machines
keywords: Nube de SQL Server, SQL Server en la nube, base de datos de PaaS, DBaaS
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 0b1eda89d2baaa54b17d8a7c73a2e9c987f0f8eb
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665992"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Selección de una opción de SQL Server en la nube: Base de datos (PaaS) SQL de Azure o SQL Server en máquinas virtuales de Azure (IaaS)

En Azure, puede hacer que las cargas de trabajo de SQL Server se ejecuten en una infraestructura hospedada (IaaS) o como un servicio hospedado ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

- [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): motor de base de datos de SQL basado en SQL Server Enterprise Edition y optimizado para el desarrollo de aplicaciones modernas. Azure SQL Database ofrece varias opciones de implementación:
  - Puede implementar una base de datos única a un [servidor lógico](sql-database-logical-servers.md).
  - Puede implementar un [grupo elástico](sql-database-elastic-pool.md) en un [servidor lógico](sql-database-logical-servers.md) para compartir recursos y reducir costos. 

      > [!NOTE]
      > Una base de datos Azure SQL Database que contiene bases de datos únicas y agrupadas ofrece la mayoría de las características de ámbito de base de datos de SQL Server.

      En la siguiente ilustración se muestran estas opciones de implementación:

      ![deployment-options](./media/sql-database-technical-overview/deployment-options.png) 
  - También puede implementar en una [Instancia administrada de Azure SQL Database (versión preliminar)](sql-database-managed-instance.md). 

      > [!NOTE]
      > Con ambas versiones, Azure SQL Database agrega características adicionales que no están disponibles en SQL Server, como la administración y la inteligencia integradas. Con la primera versión de Instancia administrada de Azure SQL Database, Azure SQL Database ofrece recursos compartidos para las bases de datos y otras características adicionales con ámbito de instancia. Instancia administrada de Azure SQL Database admite la migración de bases de datos con mínimos cambios en las bases de datos o ninguno.
- [SQL Server en Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server instalado y hospedado en la nube en máquinas virtuales (VM) de Windows Server o Linux que se ejecutan en Azure, conocido también como infraestructura como servicio (IaaS). SQL Server en Azure Virtual Machines es una buena opción para migrar aplicaciones y bases de datos de SQL Server locales sin realizar ningún cambio en las bases de datos. Todas las versiones y ediciones recientes de SQL Server están disponibles para la instalación en una máquina virtual de IaaS. La diferencia más importante de SQL Database es que las máquinas virtuales de SQL Server permiten un control total sobre el motor de base de datos. Puede elegir cuándo se iniciará el mantenimiento o la revisión, cambiar el modelo de recuperación de registro simple o masivo para permitir un registro sin carga más rápido, pausar o iniciar el motor cuando sea necesario y personalizar completamente el motor de base de datos de SQL Server. Este control adicional incluye la responsabilidad agregada de administrar las máquinas virtuales.

Obtenga información acerca de qué opción de implementación se ajusta a la plataforma de datos de Microsoft y consiga ayuda sobre la opción más adecuada para sus requisitos empresariales. Si asigna mayor prioridad al ahorro o bien antepone la mínima administración a todo lo demás, este artículo puede ayudarle a decidir el enfoque correcto, en función de los requisitos empresariales que más le preocupan.

## <a name="microsofts-sql-data-platform"></a>Plataforma de datos de Microsoft SQL

Una de las primeras cosas que hay que comprender al comparar Azure con bases de datos SQL Server locales es que puede usarlas todas. La plataforma de datos de Microsoft aprovecha la tecnología de SQL Server y la pone a disposición de los usuarios en máquinas físicas locales, entornos en nubes privadas, entornos en nubes privadas hospedados por terceros y la nube pública. SQL Server en máquinas virtuales de Azure permite satisfacer necesidades únicas y diversas del negocio gracias a una combinación de implementaciones locales y hospedadas en la nube, usando el mismo conjunto de productos de servidor, herramientas de desarrollo y conocimientos en todos estos entornos.

   ![Opciones de SQL Server en la nube: SQL Server en IaaS o base de datos SQL de SaaS en la nube.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Como se ve en el diagrama, cada oferta puede caracterizarse por el nivel de administración que se tiene sobre la infraestructura (en el eje X) y el grado de relación coste-eficacia obtenido mediante la automatización y la consolidación en el nivel de base de datos (en el eje Y).

Al diseñar una aplicación, existen cuatro opciones básicas para el hospedaje de la parte de SQL Server de la aplicación:

* SQL Server en máquinas físicas no virtualizadas
* SQL Server en máquinas locales virtualizadas (nube privada)
* SQL Server en máquinas virtuales de Azure (nube pública de Microsoft)
* Azure SQL Database (nube pública de Microsoft)

En las secciones siguientes, obtendrá información sobre SQL Server en la nube pública de Microsoft: Azure SQL Database y SQL Server en máquinas virtuales de Azure. Además, explorará factores de motivación comunes del negocio para determinar la opción que mejor funciona para su aplicación.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL Database y SQL Server en Azure Virtual Machines en detalle

**Azure SQL Database** es una base de datos relacional como servicio (DBaaS), hospedada en la nube de Azure, que se engloba en la categoría del sector denominada *Plataforma como servicio (PaaS)*. [Base de datos SQL](sql-database-technical-overview.md) se compila en hardware y software estandarizados que Microsoft posee, hospeda y mantiene. Con SQL Database, puede usar la funcionalidad y las características integradas que requieren configuración extensiva en SQL Server. Al utilizar SQL Database, se emplea el método de pago por uso de opciones para escalar vertical u horizontalmente a fin de aumentar la potencia de forma ininterrumpida. Azure SQL Database es el entorno ideal para desarrollar nuevas aplicaciones en la nube. Asimismo, con [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md), puede usar su propia licencia. Además, esta opción proporciona todas las ventajas de PaaS de Azure SQL Database, pero incluye funcionalidades que anteriormente solo estaban disponibles en las máquinas virtuales de SQL. Esto incluye una red virtual nativa (VNet) y un nivel próximo al 100 % de compatibilidad con la instancia local de SQL Server. [Instancia administrada](sql-database-managed-instance.md) es ideal para las migraciones de bases de datos locales a Azure con mínimos cambios necesarios. 

**SQL Server en Azure Virtual Machines** que se engloba en la categoría del sector denominada *Infraestructura como servicio (IaaS)* y permite ejecutar SQL Server en una máquina virtual en la nube. Las [máquinas virtuales de SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) también se ejecutan en hardware estandarizado que Microsoft posee, hospeda y mantiene. Cuando se usa SQL Server en una máquina virtual, puede usar la licencia de pago por uso de SQL Server ya incluida en una imagen de SQL Server o usar fácilmente una licencia existente. También puede detener o reanudar la máquina virtual según sea necesario.

Por lo general, estas dos opciones SQL se optimizan con diferentes fines:

* **Azure SQL Database** está optimizado para reducir al mínimo los costos generales de administración para el aprovisionamiento y la administración de muchas bases de datos. Reduce los costos corrientes de administración porque no es necesario administrar máquinas virtuales, el sistema operativo ni el software de base de datos. No es necesario administrar actualizaciones, alta disponibilidad ni [copias de seguridad](sql-database-automated-backups.md). Habitualmente, Azure SQL Database puede aumentar considerablemente el número de bases de datos administradas por un solo recurso de TI o de desarrollo. Los [grupos elásticos](sql-database-elastic-pool.md) también admiten arquitecturas de aplicaciones SaaS multiinquilino con características como el aislamiento de inquilinos y la capacidad de escalado para reducir costos al compartir recursos entre las bases de datos. [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md) proporciona compatibilidad con características de ámbito de instancia, lo que facilita la migración de las aplicaciones existentes, así como el uso compartido de recursos entre las bases de datos.
* **La ejecución de SQL Server en máquinas virtuales de Azure** está optimizada para migrar las aplicaciones existentes a Azure o para llevar las aplicaciones locales existentes a la nube en implementaciones híbridas. Además, puede usar SQL Server en una máquina virtual para desarrollar y probar las aplicaciones tradicionales de SQL Server. Con SQL Server en Máquinas virtuales de Azure, tiene todos los derechos administrativos sobre una instancia dedicada de SQL Server y una máquina virtual basada en la nube. Es la elección perfecta cuando una organización ya dispone de recursos de TI para mantener las máquinas virtuales. Estas funcionalidades permiten crear un sistema muy personalizado para abordar los requisitos de rendimiento y disponibilidad específicos de la aplicación.

En la siguiente tabla se resumen las principales características de SQL Database y SQL Server en Azure Virtual Machines:

| | Azure SQL Database<br>Servidores lógicos, grupos elásticos y bases de datos únicas | Azure SQL Database<br>Instancia administrada |Máquina virtual de Azure<br>SQL Server |
| --- | --- | --- |---|
| **Más adecuado para:** |Nuevas aplicaciones diseñadas para la nube que pretenden usar las características estables de SQL Server más recientes y que tienen restricciones de tiempo en las actividades de desarrollo y marketing. | Nuevas aplicaciones o aplicaciones locales existentes que pretenden usar las características estables de SQL Server más recientes y que se han migrado a la nube con cambios mínimos.  | Aplicaciones existentes que requieren una rápida migración a la nube con mínimos cambios o ninguno. Escenarios de desarrollo rápido y pruebas cuando no se desea comprar hardware de SQL Server de no producción local. |
|  | Equipos que requieren mecanismos integrados de alta disponibilidad, recuperación ante desastres y actualización. | Igual que SQL Database. | Equipos que puedan configurar, ajustar, personalizar y administrar una alta disponibilidad, la recuperación ante desastres y la aplicación de revisiones para SQL Server. Algunas funciones automatizadas ya incorporadas simplifican considerablemente estos aspectos. | |
|  | Equipos que no desean administrar el sistema operativo subyacente ni los valores de configuración. | Igual que SQL Database. | Necesita un entorno personalizado con derechos administrativos completos. | |
|  | Bases de datos de hasta 4 TB o mayores que pueden [particionarse de manera horizontal o vertical](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) con un patrón de escalado horizontal. | Igual que SQL Database. | Instancias de SQL Server con hasta 64 TB de almacenamiento. La instancia puede admitir tantas bases de datos como sea necesario. |
| **Compatibilidad** | Admite la mayoría de las funcionalidades de nivel de base de datos locales. | Admite casi todas las funcionalidades de nivel de instancia y nivel de base de datos locales. | Admite todas las funcionalidades locales. |
| **Recursos:** | No desea emplear recursos de TI para la configuración y administración de la infraestructura subyacente, sino centrarse en la capa de aplicación. | Igual que SQL Database. | Tiene algunos recursos de TI para la configuración y administración. Algunas funciones automatizadas ya incorporadas simplifican considerablemente estos aspectos. |
| **Costo total de propiedad:** | Elimina los costos de hardware y reduce los costos administrativos. | Igual que SQL Database. | Elimina costes de hardware. |
| **Continuidad del negocio:** |Además de las [funcionalidades integradas de infraestructura de tolerancia a errores](sql-database-high-availability.md), Azure SQL Database proporciona características que aumentan la continuidad empresarial, como, por ejemplo, [copias de seguridad automatizadas](sql-database-automated-backups.md), [restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore), [restauración geográfica](sql-database-recovery-using-backups.md#geo-restore), y [grupos de conmutación por error y replicación geográfica activa](sql-database-geo-replication-overview.md). Para más información, consulte [Información general: continuidad del negocio en la nube y recuperación ante desastres con la SQL Database](sql-database-business-continuity.md). | Igual que SQL Database, además de copias de seguridad iniciadas por el usuario solo de copia disponibles. | SQL Server en máquinas virtuales de Azure permite configurar una solución de recuperación ante desastres y alta disponibilidad para las necesidades específicas de su base de datos. Por consiguiente, podrá tener un sistema altamente optimizado para la aplicación. Podrá probar y ejecutar conmutaciones por error cuando sea necesario. Para más información, consulte [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Nube híbrida:** |La aplicación local puede obtener acceso a datos de Azure SQL Database. | [Implementación nativa de redes virtuales](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-vnet-configuration) y conectividad al entorno local mediante Azure ExpressRoute o VPN Gateway. | Con SQL Server en Máquinas virtuales de Azure, se pueden tener aplicaciones que se ejecuten parcialmente en la nube y parcialmente en la instalación local. Por ejemplo, se puede ampliar la red local y el Dominio de Active Directory a la nube mediante [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Además, se pueden almacenar archivos de datos locales en Azure Storage con [Archivos de datos de SQL Server en Azure](http://msdn.microsoft.com/library/dn385720.aspx). Para obtener más información, consulte [Introducción a la nube híbrida de SQL Server 2014](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Admite la [replicación transaccional de SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) como suscriptor para replicar los datos. | La replicación no es compatible con Instancia administrada de Azure SQL Database. | Es totalmente compatible con la [replicación transaccional de SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), los [grupos de disponibilidad AlwaysOn](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services y trasvase de registros para replicar los datos. Además, las copias de seguridad de SQL Server tradicionales son totalmente compatibles. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Motivaciones empresariales al elegir Azure SQL Database o SQL Server en Azure Virtual Machines
### <a name="cost"></a>Coste
Si se encuentra en una startup con falta de medios o en un equipo de una compañía bien establecida que opera con restricciones presupuestarias, los fondos limitados suelen ser el factor principal a la hora de decidir cómo hospedar la base de datos. En esta sección, obtendrá información relativa a los conceptos básicos sobre facturación y licencias en Azure con respecto a estas dos opciones de bases de datos relacionales: SQL Database y SQL Server en máquinas virtuales de Azure. También aprenderá a calcular el costo total de la aplicación.

#### <a name="billing-and-licensing-basics"></a>Conceptos básicos sobre facturación y licencias

Actualmente, **SQL Database** se vende como un servicio y está disponible con varios niveles de servicio y distintos precios para los recursos, que se facturan por hora a una tarifa fija en función del nivel de servicio y el nivel de rendimiento que se elija. Con Instancia administrada de Azure SQL Database, puede usar su propia licencia. Para obtener más información sobre la incorporación de licencias propias, consulte [Movilidad de Licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/). Además, se le facturará el tráfico saliente de Internet en las [tasas de transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/)habituales. Los niveles de servicio y de rendimiento se pueden ajustar de forma dinámica para satisfacer las necesidades variables de capacidad de proceso de la aplicación. Para acceder a la información más reciente sobre los niveles de servicio admitidos actualmente, consulte los artículos sobre el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md). También puede crear [grupos elásticos](sql-database-elastic-pool.md) para compartir recursos entre las instancias de la base de datos con la finalidad de reducir costos y admitir picos de uso.

Con **SQL Database**, Microsoft configura, revisa y actualiza automáticamente el software de base de datos, lo que reduce los costos de administración. Además, sus capacidades de [copia de seguridad integrada](sql-database-automated-backups.md) ayudan a obtener un ahorro significativo, sobre todo, cuando se tiene gran cantidad de base de datos. 

Con **SQL Server en máquinas virtuales de Azure**, también puede usar cualquiera de las imágenes de SQL Server que proporciona la plataforma (que incluye una licencia) o incorporar su licencia de SQL Server. Todas las versiones de SQL Server (2008 R2, 2012, 2014, 2016) y ediciones (Developer, Express, Web, Standard, Enterprise) compatibles están disponibles. Además, hay disponibles versiones “traiga su propia licencia” (BYOL) de las imágenes. Cuando se usan las imágenes suministradas por Azure, el costo operativo depende del tamaño de la máquina virtual, así como de la edición de SQL Server que elija. Independientemente del tamaño de la máquina virtual o la edición de SQL Server, se paga el costo de licencia por minuto de SQL Server y Windows o Linux Server, junto con el costo de Azure Storage para los discos de la máquina virtual. La opción de facturación por minuto permite utilizar SQL Server durante el tiempo que sea necesario sin comprar licencias adicionales de SQL Server. Si incorpora su propia licencia de SQL Server a Azure, solo se cobran los costos de servidor y de almacenamiento. Para obtener más información sobre la incorporación de licencias propias, consulte [Movilidad de Licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/). Además, se le facturará el tráfico saliente de Internet en las [tasas de transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/)habituales.

#### <a name="calculating-the-total-application-cost"></a>Cálculo del coste total de la aplicación
Cuando se comienza a usar una plataforma en la nube, el coste de ejecución de la aplicación incluye los costos de nuevo desarrollo y de administración continua, además de los costos de servicio de la plataforma en la nube pública.

**Cuándo se usa Azure SQL Database:**

- Costos de administración altamente minimizados
- Costos de desarrollo limitados para aplicaciones migradas
- Costos de servicio de SQL Database
- No hay costos de compras de hardware

**Cuándo se usa SQL Server en Máquinas virtuales de Azure:**

- Costos de administración más altos
- Costos de desarrollo de limitados a inexistentes para aplicaciones migradas
- Costos de servicio de Virtual Machines
- Costos de licencia de SQL Server
- No hay costos de compras de hardware

Para obtener más información sobre precios, consulte los siguientes recursos:

* [Precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
* [Precios de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/) para [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Administración
En muchas empresas, la decisión de pasar a un servicio en la nube está tan relacionada con la posibilidad de reducir la carga de complejidad de administración, como con el costo. Con IaaS y PaaS, Microsoft administra la infraestructura subyacente y replica automáticamente todos los datos para proporcionar recuperación ante desastres, configura y actualiza el software de base de datos, administra el equilibrio de carga y realiza una conmutación por error transparente en caso de error del servidor en un centro de datos. 

- Con **Azure SQL Database**, puede seguir administrando la base de datos, pero ya no necesita administrar el motor de la base de datos, el sistema operativo del servidor ni el hardware.  Las bases de datos y los inicios de sesión, el ajuste de índices y consultas, así como la auditoría y la seguridad, son ejemplos de elementos que puede seguir administrando. Además, la configuración de la alta disponibilidad en otro centro de datos requiere tareas de configuración y administración mínimas.
- Con **SQL Server en máquinas virtuales de Azure**, tiene un control completo sobre la configuración del sistema operativo y de la instancia de SQL Server. Con una máquina virtual, usted decide cuándo actualizar el software del sistema operativo y de la base de datos, y cuándo instalar cualquier otro software adicional, por ejemplo, un antivirus. Se proporcionan algunas características automatizadas para simplificar considerablemente la aplicación de revisiones, las copias de seguridad y la alta disponibilidad. Además, se puede controlar el tamaño de la máquina virtual, el número de discos y sus configuraciones de almacenamiento. Azure permite cambiar el tamaño de una máquina virtual cuando sea necesario. Para obtener más información, consulte [Tamaños de máquinas virtuales](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Contrato de nivel de servicio (SLA)
Para algunos departamentos de TI, cumplir las obligaciones de tiempo de actividad de un contrato de nivel de servicio (SLA) es una prioridad máxima. En esta sección, analizaremos a qué se aplica un contrato de nivel de servicio para cada opción de hospedaje de base de datos.

Para **SQL Database**, Microsoft proporciona un SLA del 99,99 % de disponibilidad. Para obtener la información más reciente, consulte [Contrato de nivel de servicio para Base de datos SQL](https://azure.microsoft.com/support/legal/sla/sql-database/). 

Para **Ejecución de SQL Server en Máquinas virtuales de Azure**, Microsoft proporciona un contrato de nivel de servicio de disponibilidad del 99,95 % que cubre solo la máquina virtual. Este contrato no cubre los procesos (como SQL Server) que se ejecutan en la máquina virtual y requieren que se hospeden como mínimo dos instancias de máquina virtual en un conjunto de disponibilidad. Para obtener la información más reciente, consulte [Contrato de nivel de servicio para Máquinas virtuales](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Para alta disponibilidad (HA) de base de datos en las máquinas virtuales, se debe configurar una de las opciones de alta disponibilidad admitidas en SQL Server, como [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). El uso de una opción de alta disponibilidad no proporciona un SLA adicional, pero le permite lograr una disponibilidad de base de datos del 99,99 %.

### <a name="market"></a>Es hora de cambiar a Azure
**Los servidores lógicos, los grupos elásticos y las bases de datos únicas de SQL Database** son la solución ideal para aplicaciones diseñadas en la nube cuando la productividad del desarrollador y un plazo de comercialización rápido son factores críticos. Con una funcionalidad de tipo DBA mediante programación, resulta perfecto para arquitectos y desarrolladores en la nube puesto que reduce la necesidad de administrar el sistema operativo y la base de datos subyacentes. 

**Instancia administrada de SQL Database** simplifica en gran medida la migración de aplicaciones existentes a Azure SQL Database, lo que le permite comercializar aplicaciones de base de datos rápidamente en Azure.

**SQL Server en ejecución en máquinas virtuales de Azure** es perfecto si las aplicaciones nuevas o existentes requieren bases de datos grandes o acceso a todas las características de SQL Server o Windows/Linux y quiere ahorrarse el tiempo y el gasto de adquirir nuevo hardware local. También es una buena opción si quiere migrar aplicaciones y bases de datos locales existentes a Azure tal cual, en casos en que Instancia administrada de Azure SQL Database no sea una opción adecuada. Dado que no es necesario cambiar los niveles de presentación, aplicación y datos, se ahorra tiempo y presupuesto en renovar la arquitectura de la solución existente. En su lugar, puede centrarse en migrar todas las soluciones a Azure y realizar algunas optimizaciones del rendimiento que requiere la plataforma de Azure. Para obtener más información, consulte [Procedimientos recomendadas para mejorar el rendimiento para SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Resumen
Este artículo explora SQL Database y SQL Server en Azure Virtual Machines y describe los factores de motivación empresariales comunes que pueden afectar a su decisión. A continuación, se ofrece un resumen de las sugerencias que debe tener en cuenta:

Elija **Azure SQL Database** , si:

* Está creando nuevas aplicaciones en la nube para aprovechar los ahorros de costos y la optimización del rendimiento que proporcionan los servicios en la nube. Este enfoque ofrece las ventajas de un servicio en la nube totalmente administrado, ayuda a reducir el tiempo de comercialización inicial y puede proporcionar una optimización de los costos duradera.
* Desea que Microsoft realice operaciones de administración comunes en las bases de datos y requiere contratos de nivel de servicio de disponibilidad más seguros para las bases de datos.
* Quiere migrar una aplicación existente tal cual a Instancia administrada de Azure SQL Database y aprovechar la paridad adicional con SQL Server y/o seguridad y redes avanzadas. Instancia administrada es una buena opción para aplicaciones nuevas y existentes.

Elija **SQL Server en Máquinas virtuales de Azure** si:

* Tiene aplicaciones locales existentes que desea migrar o extender a la nube, o si desea crear aplicaciones empresariales de más de 4 TB. Este enfoque ofrece las ventajas de poder usar la versión y la edición de SQL Server que elija, capacidad para bases de datos grandes, control completo sobre SQL Server y Windows/Linux, y tunelización segura a las instancias locales. Este enfoque minimiza los costos de desarrollo y modificaciones de las aplicaciones existentes.
* Ya dispone de recursos de TI existentes, y podría contar con aplicación de revisiones, copias de seguridad y alta disponibilidad de la base de datos. Tenga en cuenta que algunas características automatizadas simplifican considerablemente estas operaciones. 

## <a name="next-steps"></a>Pasos siguientes

* Consulte [cómo comenzar por primera vez con Azure SQL Database](sql-database-get-started-portal.md).
* Consulte [Precio de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* Consulte [Aprovisionamiento de una máquina virtual de SQL Server en Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) para una introducción a SQL Server en máquinas virtuales de Azure.
