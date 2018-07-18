---
title: Introducción a la migración de Contoso en Azure | Microsoft Docs
description: Se proporciona una introducción a la estrategia y los escenarios de migración que Contoso usa para migrar su centro de datos local a Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: ec0308bb2e39c3801305748f19783e70d58b0b7e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232215"
---
# <a name="contoso-migration-overview"></a>Migración de Contoso: introducción


Este es el primer artículo de una serie en la que se muestra cómo la organización ficticia Contoso va a migrar su infraestructura local a la nube [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/). 

En la serie de artículos se proporcionan implementaciones de ejemplo que le ayudarán a comprender y probar distintas opciones para la migración a Azure y para trabajar en un entorno híbrido. En los artículos se muestra cómo la empresa Contoso completa su misión de migración, y también se proporcionan consejos de lectura general e instrucciones específicas a lo largo de la serie.

## <a name="introduction"></a>Introducción

Azure proporciona acceso a un conjunto completo de servicios en la nube. Los desarrolladores y profesionales de TI pueden usar estos servicios para compilar, implementar y administrar aplicaciones en una variedad de herramientas y marcos, a través de una red mundial de centros de datos. A medida que su negocio se enfrenta a los desafíos asociados con el cambio digital, la nube de Azure le ayuda a encontrar la forma de optimizar los recursos y las operaciones, a comprometerse con sus clientes y empleados, y a transformar sus productos.

Sin embargo, Azure reconoce que incluso con todas las ventajas que la nube proporciona en términos de velocidad y flexibilidad, costos minimizados, rendimiento y confiabilidad, muchas organizaciones necesitarán ejecutar centros de datos locales durante algún tiempo. En respuesta a las barreras de adopción de la nube, Azure proporciona una estrategia híbrida de nube que construye puentes entre sus centros de datos locales y la nube pública de Azure. Por ejemplo, el uso de recursos de nube de Azure como copia de seguridad para proteger los recursos locales, o el uso de análisis de Azure para obtener información sobre las cargas de trabajo locales. 

Como parte de la estrategia de nube híbrida, Azure proporciona soluciones innovadoras para migrar aplicaciones locales y cargas de trabajo a la nube. Con pasos sencillos, puede evaluar exhaustivamente sus recursos locales para averiguar cómo se ejecutarán en la nube de Azure. Después, con una valoración profunda en la mano, puedes migrar recursos con seguridad a Azure. Cuando los recursos están funcionando en Azure, puede optimizarlos para retener y mejorar el acceso, la flexibilidad, la seguridad y la confiabilidad.

## <a name="migration-strategies"></a>Estrategias de migración

Las estrategias para la migración a la nube abarcan cuatro categorías amplias: rehospedar, refactorizar, rediseñar o recompilar. La estrategia que adopte depende de los impulsores de negocio y los objetivos de la migración. Puede adoptar varias estrategias. Por ejemplo, podría elegir rehospedar aplicaciones simples (migrar mediante lift-and-shift ) o aplicaciones que no son fundamentales para su negocio, pero rediseñar las que son más complejas y críticas para la empresa. Echemos un vistazo a las estrategias.


**Estrategia** | **Definición** | **Cuándo se deben usar** 
--- | --- | --- 
**Rehospedaje** | A menudo se denomina una migración mediante "lift-and-shift". Esta opción no requiere cambios en el código y permite migrar las aplicaciones existentes a Azure rápidamente. Cada aplicación se migra tal cual, para aprovechar las ventajas que ofrece la nube, sin correr el riesgo ni incurrir en los costos asociados con los cambios de código. | Cuando necesite mover aplicaciones rápidamente a la nube.<br/><br/> Cuando quiera mover una aplicación sin modificación alguna.<br/><br/> Cuando la arquitectura de las aplicaciones se diseñan de modo que puedan aprovechar la escalabilidad de [IaaS de Azure](https://azure.microsoft.com/overview/what-is-iaas/) después de la migración.<br/><br/> Cuando las aplicaciones son importantes para su negocio, pero no es necesario realizar cambios inmediatamente en las funciones de la aplicación.
**Refactorización** | Este concepto con frecuencia se conoce como "reempaquetar" y requiere una mínima cantidad de cambios en las aplicaciones para que puedan conectarse a [PaaS de Azure](https://azure.microsoft.com/overview/what-is-paas/) y usar las ofertas de la nube.<br/><br/> Por ejemplo, puede migrar sus aplicaciones existentes a Azure App Service o Azure Kubernetes Service (AKS).<br/><br/> O bien, podría refactorizar las bases de datos relacionales y no relacionales en opciones como, por ejemplo, Instancia administrada de Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL y Azure Cosmos DB. | Si la aplicación se puede reempaquetar fácilmente para que funcione en Azure.<br/><br/> Si quiere aplicar prácticas DevOps innovadoras proporcionadas por Azure, o piensa en DevOps con una estrategia de contenedor para las cargas de trabajo.<br/><br/> Para la refactorización, debe pensar en la portabilidad de la base de código existente y las capacidades de desarrollo disponibles.
**Rediseño** | El rediseño para la migración se centra en modificar y extender la funcionalidad de las aplicaciones y el código base con el fin de optimizar la arquitectura de aplicación para la escalabilidad en la nube.<br/><br/> Por ejemplo, podría dividir una aplicación monolítica en un grupo de microservicios que funcionan en conjunto y se escalan fácilmente.<br/><br/> O bien, puede rediseñar las bases de datos relacionales y no relacionales para adaptarlas a soluciones DBaaS totalmente administradas, como Instancia administrada de Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL y Azure Cosmos DB. | Cuando las aplicaciones necesitan revisiones importantes para incorporar funcionalidades nuevas o para funcionar de forma eficaz en una plataforma de nube.<br/><br/> Si desea usar las inversiones existentes en las aplicaciones, cumplir los requisitos de escalabilidad, aplicar prácticas innovadoras de Azure DevOps y minimizar el uso de máquinas virtuales.
**Recompilación** | La recompilación va más lejos al recompilar una aplicación desde cero mediante tecnologías de nube de Azure.<br/><br/> Por ejemplo, puede compilar aplicaciones tipo green field con tecnologías nativas de nube, como soluciones sin servidor, Azure AI, Instancia administrada de SQL Database y Azure Cosmos DB. | Cuando quiera un desarrollo rápido, y las aplicaciones existentes tengan funcionalidad y ciclo de vida limitados.<br/><br/> Cuando esté listo para acelerar la innovación comercial (como las prácticas de DevOps que proporciona Azure), compilar nuevas aplicaciones mediante tecnologías nativas a la nube y aprovechar los avances en inteligencia artificial, cadena de bloques e IoT.

## <a name="migration-articles"></a>Artículos sobre la migración

En la tabla siguiente se proporciona un resumen de los artículos que componen la serie sobre la migración de Contoso.  

- Los escenarios son cada vez más complejos e iremos agregando información a medida que pase el tiempo.
- A cada escenario de migración lo impulsan objetivos empresariales ligeramente distintos que determinan la estrategia de migración.
- Para cada escenario de implementación, se proporciona información sobre los impulsores y objetivos empresariales, una arquitectura propuesta, pasos para realizar la migración y una recomendación para la limpieza y los pasos siguientes una vez completada la migración.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
Artículo 1: Introducción (este artículo) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que se usan. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-overview.md) (Implementación de una infraestructura de Azure) | Se describe cómo Contoso prepara su infraestructura local y de Azure para la migración. Se usa la misma infraestructura para todos los escenarios de migración de Contoso. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales) | Se muestra cómo Contoso realiza una valoración de su aplicación de dos niveles local SmartHotel que se ejecuta en VMware. Se evalúan las VM de la aplicación mediante el servicio [Azure Migrate](migrate-overview.md) y la base de datos de SQL Server de aplicaciones con [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehospedar en máquinas virtuales de Azure y una instancia administrada de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Se muestra cómo Contoso migra la aplicación SmartHotel a Azure. Migrar la VM de aplicación web con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) y la base de datos de aplicaciones a una instancia administrada de SQL mediante el servicio [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible
[Artículo 5: Volver a hospedar máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Se muestra cómo Contoso migra sus máquinas virtuales de la aplicación SmartHotel mediante el servicio Site Recovery.
[Artículo 6: Rehospedar en VM de Azure y grupos de disponibilidad de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Se muestra cómo Contoso migra la aplicación SmartHotel. Se usa Site Recovery para migrar laVM de la aplicación y Database Migration Service para migrar la base de datos de aplicaciones a un grupo de disponibilidad AlwaysOn de SQL Server. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Se muestra cómo Contoso migra las VM de aplicación a Azure mediante Site Recovery. | Planeado
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Se muestra cómo Contoso migra la VM de la aplicación mediante Site Recovery y usa MySQL Workbench para migrar a una instancia de Azure MySQL Server. | Planeado



### <a name="demo-apps"></a>Aplicaciones de demostración

En los artículos se usan dos aplicaciones de demostración: SmartHotel y osTicket.

- **SmartHotel360**: esta aplicación la desarrolló Microsoft como una aplicación de prueba que se puede usar cuando se trabaja con Azure. Se proporciona como código abierto y se puede descargar desde [GitHub](https://github.com/Microsoft/SmartHotel360). Se trata de una aplicación ASP.NET conectada a una base de datos de SQL Server. Actualmente, la aplicación se encuentra en dos VM de VMware que ejecutan Windows Server 2008 R2 y SQL Server 2008 R2. Las VM de la aplicación se hospedan localmente y las administra vCenter Server.
- **osTicket** es una aplicación de incidencias de consola de servicio de código abierto que se ejecuta en Linux. Puede descargarlo en [GitHub](https://github.com/osTicket/osTicket). La aplicación actual se encuentra en dos VM de VMware que ejecutan Ubuntu 16.04LTS, con Apache 2, PHP 7.0 y MySQL 5.7.
    

## <a name="next-steps"></a>Pasos siguientes

[Vea cómo](contoso-migration-infrastructure.md) Contoso configura su infraestructura local y de Azure mientras se prepara para la migración.



