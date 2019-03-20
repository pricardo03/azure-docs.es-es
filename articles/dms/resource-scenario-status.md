---
title: Estado del escenario de migración de bases de datos | Microsoft Docs
description: Obtenga información sobre el estado de los escenarios de migración que admite Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: b07f36055f9c0690450e8d4ab5abacb8c84d9bf2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182547"
---
# <a name="status-of-migration-scenarios-supported-by-the-azure-database-migration-service"></a>Estado de los escenarios de migración que admite Azure Database Migration Service
Azure Database Migration Service está diseñado para admitir una serie de escenarios de migración (pares de origen/destino) para las migraciones sin conexión (de un solo uso) y en línea (sincronización continua). Azure Database Migration Service proporciona una cobertura de escenarios que amplían constantemente. Nos encargamos de agregar nuevos escenarios de forma regular. En este artículo se identifican los escenarios de migración que actualmente son compatibles con Azure Database Migration Service y el estado (versión preliminar privada [o limitada], versión preliminar pública o disponibilidad general) de cada escenario.

## <a name="offline-versus-online-migrations"></a>Migraciones sin conexión o en línea
Cuando migra bases de datos a Azure mediante Azure Database Migration Service, puede realizar una migración sin conexión o en línea. Si usa las migraciones *sin conexión*, el tiempo de inactividad de la aplicación comienza al mismo tiempo que la migración. En cuanto a las *migraciones en línea*, el tiempo de inactividad se limita al tiempo requerido para pasar al nuevo entorno cuando finaliza la migración. Es recomendable probar una migración sin conexión para determinar si el tiempo de inactividad es aceptable; si no fuera así, realice una migración en línea.

## <a name="migration-scenario-status"></a>Estado del escenario de migración
El estado de cada escenario de migración que admite Azure Database Migration Service varía con el tiempo. En general, los escenarios se publican por primera vez en la **versión preliminar privada** y, para poder aprovechar sus funcionalidades, es necesario que un cliente envíe una solicitud a través del [sitio de versión preliminar de DMS](https://aka.ms/dms-preview). Cuando se completa la versión preliminar privada, el estado del escenario cambia a **versión preliminar pública**. Todos los usuarios de Azure Database Migration Service pueden aprovechar las ventajas de los escenarios de migración que están disponibles en versión preliminar pública. Sin embargo, el escenario de migración puede que no esté disponible en todas las regiones y la funcionalidad puede sufrir cambios adicionales antes del lanzamiento de la versión final. Cuando un escenario de migración tiene el estado **Disponible en general**, que es el estado final publicado, quiere decir que la funcionalidad está completada y es accesible para todos los usuarios de Azure Database Migration Service. 

## <a name="migration-scenario-support"></a>Compatibilidad del escenario de migración

En las siguientes tablas se muestran qué escenarios de migración se admiten cuando se usa Azure Database Migration Service.

> [!NOTE]
> Si un escenario que se indica como admitido no aparece en la interfaz de usuario, póngase en contacto con el [Equipo de migración de datos](mailto:datamigrationteam@microsoft.com) para obtener información adicional.

### <a name="offline-one-time-migration-support"></a>Compatibilidad de la migración sin conexión (de un solo uso)
En la siguiente tabla se muestra la compatibilidad de Azure Database Migration Service con las migraciones sin conexión.

| Destino  | Origen | Soporte técnico |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | SQL de RDS  |  ✔ |
|   | Oracle  |   |
| **Azure SQL DB MI**  | SQL Server  | ✔ |
|   | SQL de RDS  | ✔ |
|   | Oracle  | ✔  |
| **Máquina virtual de Azure SQL**  | SQL Server | ✔ |
|   | Oracle  |   |
| **Azure Cosmos DB**  | MongoDB | ✔ |
| **Azure DB para MySQL**  | MySQL |  |
|   | MySQL de RDS  |  |
| **Azure DB para PostgreSQL**  | PostgreSQL |  |
|  | PostgreSQL de RDS  |  |

### <a name="online-continuous-sync-migration-support"></a>Compatibilidad con la migración en línea (sincronización continua)
En la siguiente tabla se muestra la compatibilidad de Azure Database Migration Service con las migraciones en línea.

| Destino  | Origen | Soporte técnico |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | SQL de RDS  |   |
|   | Oracle  |  ✔ |
| **Azure SQL DB MI**  | SQL Server  | ✔ |
|   | SQL de RDS  |  |
|   | Oracle  | ✔  |
| **Máquina virtual de Azure SQL**  | SQL Server  |   |
|   | Oracle  | ✔  |
| **Azure Cosmos DB**  | MongoDB  | ✔ |
| **Azure DB para MySQL**  | MySQL | ✔ |
|   | MySQL de RDS  | ✔ |
| **Azure DB para PostgreSQL**  | PostgreSQL | ✔ |
|  | PostgreSQL de RDS  | ✔ |

## <a name="next-steps"></a>Pasos siguientes
Para información general sobre Azure Database Migration Service y la disponibilidad regional, consulte el artículo [¿Qué es la versión preliminar de Azure Database Migration Service?](dms-overview.md) 
