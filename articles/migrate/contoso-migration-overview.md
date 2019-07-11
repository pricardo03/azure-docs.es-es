---
title: Serie de migración de Contoso | Microsoft Docs
description: Se proporciona una introducción a la estrategia y los escenarios de migración que Contoso usa para migrar su centro de datos local a Azure.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: raynew
ms.openlocfilehash: 82585a6f1df369e299bd9df4d57f6106aad4ead7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696217"
---
# <a name="contoso-migration-series"></a>Serie de migración de Contoso


Hay una serie de artículos en los que mostramos cómo la organización ficticia Contoso migra una infraestructura local a la nube de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/). 

La serie incluye información y escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. La complejidad de los escenarios va en aumento. En los artículos se muestra cómo la empresa Contoso completa su misión de migración, y también se proporcionan consejos de lectura general e instrucciones específicas a lo largo de la serie.

## <a name="migration-articles"></a>Artículos sobre la migración

En la tabla siguiente se resumen los artículos de la serie.  

- A cada escenario de migración lo impulsan objetivos empresariales ligeramente distintos que determinan la estrategia de migración.
- Para cada escenario de implementación, se proporciona información sobre los impulsores y objetivos empresariales, una arquitectura propuesta, pasos para realizar la migración y una recomendación para la limpieza y los pasos siguientes una vez completada la migración.

**Artículo** | **Detalles** 
--- | --- 
[Artículo 1: Introducción](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Información general de la serie de artículos, la estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. 
[Artículo 2: implementar una infraestructura de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso prepara la infraestructura local y la infraestructura de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie sobre migración. 
[Artículo 3: valorar las cargas de trabajo locales para la migración a Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-assessment)  | Contoso ejecuta una valoración de su aplicación local SmartHotel360 que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant.
[Artículo 4: rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso ejecuta una migración mediante lift-and-shift a Azure para su aplicación SmartHotel360 local. Contoso migra la máquina virtual de front-end de la aplicación mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview).
[Artículo 5: Rehospedar una aplicación local en máquinas virtuales de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Site Recovery. 
[Artículo 6: rehospedaje de una aplicación local en las VM de Azure y en el grupo de disponibilidad AlwayOn de SQL Server](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) |Contoso migra la aplicación SmartHotel360. Contoso usa Site Recovery para migrar las máquinas virtuales de la aplicación. Usa Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server que está protegido por un grupo de disponibilidad Always On. 
[Artículo 7: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso realiza una migración mediante "lift-and-shift" de su aplicación osTicket de Linux a máquinas virtuales de Azure mediante el servicio Site Recovery.
[Artículo 8: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure y Azure Database for MySQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso migra su aplicación osTicket de Linux a máquinas virtuales de Azure mediante Site Recovery. Migra la base de datos de la aplicación a Azure Database for MySQL con MySQL Workbench. 
[Artículo 9: refactorizar una aplicación local a una aplicación web de Azure y una base de datos de Azure SQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso migra su aplicación SmartHotel360 a una aplicación web de Azure, y migra la base de datos de la aplicación a una instancia de Azure SQL Server con Database Migration Assistant.     
[Artículo 10: Migración de Contoso: refactorización de una aplicación de consola de servicio de Linux en Contoso en varias regiones con Azure App Service, Traffic Manager y Azure MySQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure con Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. 
[Artículo 11: Refactorización de Team Foundation Server en Azure DevOps Services](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso migra su implementación local de Team Foundation Server a Azure DevOps Services en Azure.
[Artículo 12: rediseñar la arquitectura de una aplicación local en un contenedor de Azure y Azure SQL Database](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rearchitect-container-sql) | Contoso migra su aplicación SmartHotel a Azure. A continuación, rediseña el nivel de aplicación web como un contenedor de Windows que se ejecuta en Azure Service Fabric, y la base de datos con Azure SQL Database. 
[Artículo 13: Recompilación de una aplicación en Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB.  
[Artículo 14: Escalado de una migración en Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Después de probar combinaciones de migración, Contoso se prepara para escalar una migración completa a Azure. 


    

## <a name="next-steps"></a>Pasos siguientes

[Más información](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/) sobre la migración a la nube. 

