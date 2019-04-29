---
title: Estado del escenario de migración de bases de datos | Microsoft Docs
description: Obtenga información sobre el estado de los escenarios de migración admitidos por Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/04/2019
ms.openlocfilehash: 4159b2e7af83030f46d5aca150ef99a1380e711f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473015"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Estado de los escenarios de migración admitidos por Azure Database Migration Service
Azure Database Migration Service está diseñado para admitir escenarios de migración diferentes (pares de origen o destino) para ambos sin conexión (un solo uso) y las migraciones en línea (sincronización continua). Se ha ampliado la cobertura de escenario proporcionada por Azure Database Migration Service con el tiempo. Nos encargamos de agregar nuevos escenarios de forma regular. Este artículo identifican los escenarios de migración compatibles actualmente con Azure Database Migration Service y el estado (versión preliminar privada, versión preliminar pública o la disponibilidad general) para cada escenario.

## <a name="offline-versus-online-migrations"></a>Migraciones sin conexión o en línea
Con Azure Database Migration Service, puede realizar una migración en línea o sin conexión. Si usa las migraciones *sin conexión*, el tiempo de inactividad de la aplicación comienza al mismo tiempo que la migración. Para limitar el tiempo de inactividad en el tiempo necesario para pasar al nuevo entorno cuando se complete la migración, use un *online* migración. Se recomienda para probar una migración sin conexión para determinar si el tiempo de inactividad es aceptable; Si no es así, realizar una migración en línea.

## <a name="migration-scenario-status"></a>Estado del escenario de migración
El estado de los escenarios de migración admitidos por Azure Database Migration Service varía con el tiempo. Por lo general, los escenarios se publican primero en **versión preliminar privada**. Participar en versión preliminar privada requiere que los clientes para que envíe una solicitud a través de la [sitio de vista previa de DMS](https://aka.ms/dms-preview). Después de la versión preliminar privada, el estado de la situación cambia a **versión preliminar pública**. Los usuarios de Azure Database Migration Service pueden probar escenarios de migración en versión preliminar pública directamente desde la interfaz de usuario. Registro no es necesario.  Sin embargo, los escenarios de migración en versión preliminar pública pueden no estar disponibles en todas las regiones y pueden sufrir cambios adicionales antes de la versión final. Después de la versión preliminar pública, el estado de la situación cambia a **generalmente disponibilidad**. Disponibilidad general (GA) es el estado de la versión final y la funcionalidad está completa y puede tener acceso a todos los usuarios.

## <a name="migration-scenario-support"></a>Compatibilidad del escenario de migración
Las siguientes tablas muestran los escenarios de migración son compatibles con Azure Database Migration Service.

> [!NOTE]
> Si un escenario que se indica como admitido no aparece en la interfaz de usuario, póngase en contacto con el [Equipo de migración de datos](mailto:datamigrationteam@microsoft.com) para obtener información adicional.

> [!IMPORTANT]
> Para ver todos los escenarios compatibles actualmente con Azure Database Migration Service en versión preliminar privada, consulte el [sitio de vista previa de DMS](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Compatibilidad de la migración sin conexión (de un solo uso)
En la siguiente tabla se muestra la compatibilidad de Azure Database Migration Service con las migraciones sin conexión.

| Destino  | Origen | Soporte técnico | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | SQL de RDS |  |  |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | SQL de RDS |  |  |
|   | Oracle |  |   |
| **Máquina virtual de Azure SQL** | SQL Server | ✔ | GA |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | Versión preliminar pública |
| **Azure DB para MySQL** | MySQL |   |   |
|   | MySQL de RDS |   |   |
| **Azure DB para PostgreSQL** | PostgreSQL |  |
|  | PostgreSQL de RDS |   |   |

### <a name="online-continuous-sync-migration-support"></a>Compatibilidad con la migración en línea (sincronización continua)
En la siguiente tabla se muestra la compatibilidad de Azure Database Migration Service con las migraciones en línea.

| Destino  | Origen | Soporte técnico | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | SQL de RDS | ✔ | GA |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | SQL de RDS | ✔ | GA |
|   | Oracle | ✔ | Versión preliminar privada |
| **Máquina virtual de Azure SQL** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | Versión preliminar pública |
| **Azure DB para MySQL** | MySQL | ✔ | GA |
|   | MySQL de RDS | ✔ | GA |
| **Azure DB para PostgreSQL** | PostgreSQL | ✔ | GA |
|   | PostgreSQL de RDS | ✔ | GA |
|   | Oracle | ✔ | Versión preliminar privada |

## <a name="next-steps"></a>Pasos siguientes
Para obtener información general de Azure Database Migration Service y la disponibilidad regional, consulte el artículo [¿qué es Azure Database Migration Service](dms-overview.md).
