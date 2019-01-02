---
title: Introducción a la migración de Contoso en Azure | Microsoft Docs
description: Se proporciona una introducción a la estrategia y los escenarios de migración que Contoso usa para migrar su centro de datos local a Azure.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: f18af40c67195ee2e1e74c04c0834eea06fb14d1
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53191961"
---
# <a name="contoso-migration-overview"></a>Migración de Contoso: Información general


En este artículo se muestra cómo la organización ficticia Contoso migra una infraestructura local a la nube de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/). 

Este documento es el primero de una serie de artículos que muestran cómo la compañía ficticia Contoso migra a Azure. La serie incluye información y escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. La complejidad de los escenarios aumenta e iremos agregando otros artículos con el tiempo. En los artículos se muestra cómo la empresa Contoso completa su misión de migración, y también se proporcionan consejos de lectura general e instrucciones específicas a lo largo de la serie.

## <a name="introduction"></a>Introducción

Azure proporciona acceso a un conjunto completo de servicios en la nube. Los desarrolladores y profesionales de TI pueden usar estos servicios para compilar, implementar y administrar aplicaciones en una variedad de herramientas y marcos, a través de una red mundial de centros de datos. A medida que su negocio se enfrenta a los desafíos asociados con el cambio digital, la nube de Azure le ayuda a encontrar la forma de optimizar los recursos y las operaciones, a comprometerse con sus clientes y empleados, y a transformar sus productos.

Sin embargo, Azure reconoce que incluso con todas las ventajas que la nube proporciona en términos de velocidad y flexibilidad, costos minimizados, rendimiento y confiabilidad, muchas organizaciones necesitarán ejecutar centros de datos locales durante algún tiempo. En respuesta a las barreras de adopción de la nube, Azure proporciona una estrategia híbrida de nube que construye puentes entre sus centros de datos locales y la nube pública de Azure. Por ejemplo, el uso de recursos de nube de Azure como Azure Backup para proteger los recursos locales, o el uso de Azure Analytics para obtener información sobre las cargas de trabajo locales. 

Como parte de la estrategia de nube híbrida, Azure proporciona soluciones innovadoras para migrar aplicaciones locales y cargas de trabajo a la nube. Con pasos sencillos, puede evaluar exhaustivamente sus recursos locales para averiguar cómo se ejecutarán en la nube de Azure. Después, con una valoración profunda en la mano, puedes migrar recursos con seguridad a Azure. Cuando los recursos están funcionando en Azure, puede optimizarlos para retener y mejorar el acceso, la flexibilidad, la seguridad y la confiabilidad.

## <a name="migration-strategies"></a>Estrategias de migración

Las estrategias para la migración a la nube abarcan cuatro categorías amplias: rehospedar, refactorizar, rediseñar o recompilar. La estrategia que adopte depende de los impulsores de negocio y los objetivos de la migración. Puede adoptar varias estrategias. Por ejemplo, podría elegir rehospedar aplicaciones simples (migrar mediante lift-and-shift ) o aplicaciones que no son fundamentales para su negocio, pero rediseñar las que son más complejas y críticas para la empresa. Echemos un vistazo a las estrategias.


**Estrategia** | **Definición** | **Cuándo se deben usar** 
--- | --- | --- 
**Rehospedaje** | A menudo se denomina una migración mediante "lift-and-shift". Esta opción no requiere cambios en el código y permite migrar las aplicaciones existentes a Azure rápidamente. Cada aplicación se migra tal cual, para aprovechar las ventajas que ofrece la nube, sin correr el riesgo ni incurrir en los costos asociados con los cambios de código. | Cuando necesite mover aplicaciones rápidamente a la nube.<br/><br/> Cuando quiera mover una aplicación sin modificación alguna.<br/><br/> Cuando la arquitectura de las aplicaciones se diseñan de modo que puedan aprovechar la escalabilidad de [IaaS de Azure](https://azure.microsoft.com/overview/what-is-iaas/) después de la migración.<br/><br/> Cuando las aplicaciones son importantes para su negocio, pero no es necesario realizar cambios inmediatamente en las funciones de la aplicación.
**Refactorización** | Este concepto con frecuencia se conoce como "reempaquetar" y requiere una mínima cantidad de cambios en las aplicaciones para que puedan conectarse a [PaaS de Azure](https://azure.microsoft.com/overview/what-is-paas/) y usar las ofertas de la nube.<br/><br/> Por ejemplo, puede migrar sus aplicaciones existentes a Azure App Service o Azure Kubernetes Service (AKS).<br/><br/> O bien, podría refactorizar las bases de datos relacionales y no relacionales en opciones como, por ejemplo, Instancia administrada de Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL y Azure Cosmos DB. | Si la aplicación se puede reempaquetar fácilmente para que funcione en Azure.<br/><br/> Si quiere aplicar prácticas DevOps innovadoras proporcionadas por Azure, o piensa en DevOps con una estrategia de contenedor para las cargas de trabajo.<br/><br/> Para la refactorización, debe pensar en la portabilidad de la base de código existente y las capacidades de desarrollo disponibles.
**Rediseño** | El rediseño para la migración se centra en modificar y ampliar la funcionalidad de las aplicaciones y el código base con el fin de optimizar la arquitectura de aplicación para la escalabilidad en la nube.<br/><br/> Por ejemplo, podría dividir una aplicación monolítica en un grupo de microservicios que funcionan en conjunto y se escalan fácilmente.<br/><br/> O bien, puede rediseñar las bases de datos relacionales y no relacionales para adaptarlas a soluciones DBaaS totalmente administradas, como Instancia administrada de Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL y Azure Cosmos DB. | Cuando las aplicaciones necesitan revisiones importantes para incorporar funcionalidades nuevas o para funcionar de forma eficaz en una plataforma de nube.<br/><br/> Si desea usar las inversiones existentes en las aplicaciones, cumplir los requisitos de escalabilidad, aplicar prácticas innovadoras de Azure DevOps y minimizar el uso de máquinas virtuales.
**Recompilación** | La recompilación va más lejos al recompilar una aplicación desde cero mediante tecnologías de nube de Azure.<br/><br/> Por ejemplo, puede compilar aplicaciones tipo green field con tecnologías nativas de nube, Azure Functions, Azure AI, Instancia administrada de Azure SQL Database y Azure Cosmos DB. | Cuando quiera un desarrollo rápido, y las aplicaciones existentes tengan funcionalidad y ciclo de vida limitados.<br/><br/> Cuando esté listo para acelerar la innovación empresarial (como los procedimientos de DevOps que proporciona Azure), cree aplicaciones mediante tecnologías nativas-nube y aproveche los avances en inteligencia artificial, cadena de bloques e IoT.

## <a name="migration-articles"></a>Artículos sobre la migración

En la tabla siguiente se resumen los artículos de la serie.  

- A cada escenario de migración lo impulsan objetivos empresariales ligeramente distintos que determinan la estrategia de migración.
- Para cada escenario de implementación, se proporciona información sobre los impulsores y objetivos empresariales, una arquitectura propuesta, pasos para realizar la migración y una recomendación para la limpieza y los pasos siguientes una vez completada la migración.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
Artículo 1: Información general | Información general de la serie de artículos, la estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. | Este artículo
[Artículo 2: Contoso: Implementar una infraestructura de migración](contoso-migration-infrastructure.md) | Contoso prepara la infraestructura local y la infraestructura de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie sobre migración. | Disponible
[Artículo 3: Migración de Contoso: valorar las cargas de trabajo locales para la migración a Azure](contoso-migration-assessment.md)  | Contoso ejecuta una valoración de su aplicación local SmartHotel360 que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant. | Disponible
[Artículo 4: Rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso ejecuta una migración mediante lift-and-shift a Azure para su aplicación SmartHotel360 local. Contoso migra la máquina virtual de front-end de la aplicación mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible   
[Artículo 5: Rehospedar una aplicación local en máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Site Recovery. | Disponible [Artículo 5: Rehospedar una aplicación local en máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Site Recovery. | Disponible
[Artículo 6: Migración de Contoso: rehospedaje de una aplicación local en las VM de Azure y en el grupo de disponibilidad Always On de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Contoso migra la aplicación SmartHotel360. Contoso usa Site Recovery para migrar las máquinas virtuales de la aplicación. Usa Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server que está protegido por un grupo de disponibilidad Always On. | Disponible [Artículo 7: Migración de Contoso: rehospedaje de una aplicación de Linux local en VM de Azure](contoso-migration-rehost-linux-vm.md) | Contoso realiza una migración mediante "lift-and-shift" de su aplicación osTicket de Linux a máquinas virtuales de Azure mediante el servicio Site Recovery. | Disponible
[Artículo 8: Migración de Contoso: Rehospedaje de la aplicación de Linux local en VM de Azure y Azure Database for MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migra su aplicación osTicket de Linux a máquinas virtuales de Azure mediante Site Recovery. Migra la base de datos de la aplicación a Azure Database for MySQL con MySQL Workbench. | Disponible
[Artículo 9: Migración de Contoso: refactorizar una aplicación local a una instancia de Azure Web App y a una instancia de Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Contoso migra su aplicación SmartHotel360 a una aplicación web de Azure, y migra la base de datos de la aplicación a una instancia de Azure SQL Server con Database Migration Assistant. | Disponible    
[Artículo 10: Migración de Contoso: refactorización de una aplicación de consola de servicio de Linux en Contoso en varias regiones con Azure App Service, Traffic Manager y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure con Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. | Disponible
[Artículo 11: Migración de Contoso: refactorización de una implementación de Team Foundation Server a Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migra su implementación local de Team Foundation Server a Azure DevOps Services en Azure. | Disponible
[Artículo 12: Migración de Contoso: rediseñar la arquitectura de una aplicación local en un contenedor de Azure y Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso migra su aplicación SmartHotel a Azure. A continuación, rediseña el nivel de aplicación web como un contenedor de Windows que se ejecuta en Azure Service Fabric, y la base de datos con Azure SQL Database. | Disponible    
[Artículo 13: Migración de Contoso: Recompilar una aplicación local en Azure](contoso-migration-rebuild.md) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB. | Disponible 
[Artículo 14: Contoso: Escalado de una migración en Azure](contoso-migration-scale.md) | Después de probar combinaciones de migración, Contoso se prepara para escalar una migración completa a Azure. | Disponible

En este artículo, Contoso configura todos los elementos de infraestructura necesarios para completar todos los escenarios de migración. 







### <a name="demo-apps"></a>Aplicaciones de demostración

En los artículos se usan dos aplicaciones de demostración: SmartHotel360 y osTicket.

- **SmartHotel360**: Microsoft desarrolló esta aplicación como una aplicación de prueba que se puede usar cuando se trabaja con Azure. Se proporciona como código abierto y se puede descargar desde [GitHub](https://github.com/Microsoft/SmartHotel360). Se trata de una aplicación ASP.NET conectada a una base de datos de SQL Server. Actualmente, la aplicación se encuentra en dos VM de VMware que ejecutan Windows Server 2008 R2 y SQL Server 2008 R2. Las VM de la aplicación se hospedan localmente y las administra vCenter Server.
- **osTicket**: se trata de una aplicación de incidencias de soporte técnico de código abierto que se ejecuta en Linux. Puede descargarlo en [GitHub](https://github.com/osTicket/osTicket). En este momento, la aplicación se encuentra en dos máquinas virtuales VMware que ejecutan Ubuntu 16.04 LTS, con Apache 2, PHP 7.0 y MySQL 5.7.
    

## <a name="next-steps"></a>Pasos siguientes

[Vea cómo](contoso-migration-infrastructure.md) Contoso configura una infraestructura local y de Azure para prepararse para la migración.



