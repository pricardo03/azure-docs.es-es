---
title: Cómo configurar Azure SQL Database | Microsoft Docs
description: Aprenda cómo configurar y administrar Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: b4dd21324591075d7625a82fbbb661c4a8e84b1d
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53439542"
---
# <a name="how-to-use-azure-sql-database"></a>Cómo usar Azure SQL Database

En esta sección puede encontrar varias guías, scripts y explicaciones que pueden ayudarle a administrar y configurar Azure SQL Database. También puede encontrar guías de procedimientos específicas para [Base de datos única](sql-database-howto-single-database.md) e [Instancia administrada](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Carga de datos

- [Copia de una base de datos única en Azure](https://docs.microsoft.com/azure/sql-database/sql-database-copy)
- [Importación de una base de datos desde un archivo BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-import)
- [Exportación de una base de datos a un archivo BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-export)
- [Carga de datos con BCP](https://docs.microsoft.com/azure/sql-database/sql-database-load-from-csv-with-bcp)
- [Carga de datos con ADF](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Sincronización de datos

- [SQL Data Sync](https://docs.microsoft.com/azure/sql-database/sql-database-sync-data)
- [Agente de sincronización de datos](https://docs.microsoft.com/azure/sql-database/sql-database-data-sync-agent)
- [Replicación de cambios de esquema](https://docs.microsoft.com/azure/sql-database/sql-database-update-sync-schema)
- [Supervisión con OMS](https://docs.microsoft.com/azure/sql-database/sql-database-sync-monitor-oms)
- [Procedimientos recomendados para Data Sync](https://docs.microsoft.com/azure/sql-database/sql-database-best-practices-data-sync)
- [Solución de problemas de Data Sync](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-data-sync)

## <a name="monitoring-and-tuning"></a>Supervisión y ajuste

-  [Ajuste manual](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
- [Uso de DMV para supervisar el rendimiento](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
- [Uso del almacén de consultas para supervisar el rendimiento](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)
- [Solución de problemas de rendimiento con Intelligent Insights](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance)
- [Uso del registro de diagnóstico de Intelligent Insights](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-use-diagnostics-log)
- [Supervisión del espacio OLTP en memoria](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring)

### <a name="extended-events"></a>Eventos extendidos

- [Eventos extendidos](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr)
- [Almacenar eventos extendidos en el archivo de evento](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-event-file)
- [Almacenar eventos extendidos en el búfer en anillo](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-ring-buffer)

## <a name="configure-features"></a>Configuración de características

- [Configuración de la autenticación de Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)
- [Configuración del acceso condicional](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- [Multi-factor Authentication para AAD](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Configuración de Multi-factor Authentication](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication-configure)
- [Configuración temporal de la directiva de retención](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables-retention-policy)
- [Configuración de TDE con BYOK](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure)
- [Rotación de claves BYOK de TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-key-rotation)
- [Eliminación del protector de TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-remove-tde-protector)
- [Configuración de OLTP en memoria](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-migration)
- [Configuración de Azure Automation](https://docs.microsoft.com/azure/sql-database/sql-database-manage-automation)

## <a name="develop-applications"></a>Desarrollo de aplicaciones

- [Conectividad](https://docs.microsoft.com/azure/sql-database/sql-database-libraries)
- [Uso de Spark Connector](https://docs.microsoft.com/azure/sql-database/sql-database-spark-connector)
- [Autenticación de aplicaciones](https://docs.microsoft.com/azure/sql-database/sql-database-client-id-keys)
- [Mensajes de error](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages)
- [Uso del procesamiento por lotes para mejorar el rendimiento](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance)
- [Guía de conectividad](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)
- [Alias DNS](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)
- [Configuración de PowerShell con alias DNS](https://docs.microsoft.com/azure/sql-database/dns-alias-powershell)
- [Puertos: ADO.NET](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12)
- [C y C ++](https://docs.microsoft.com/azure/sql-database/sql-database-develop-cplusplus-simple)
- [Excel](https://docs.microsoft.com/azure/sql-database/sql-database-connect-excel)

## <a name="design-applications"></a>Diseño de aplicaciones

- [Diseño de la recuperación ante desastres](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
- [Diseño para grupos elásticos](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)
- [Diseño para actualización de aplicaciones](https://docs.microsoft.com/azure/sql-database/sql-database-manage-application-rolling-upgrade)

### <a name="design-multi-tenant-saas-applications"></a>Diseño de aplicaciones SaaS multiinquilino

- [Patrones de diseño SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-app-design-patterns)
- [Video Indexer SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-video-index-wingtip-brk3120-20171011)
- [Seguridad de aplicaciones SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-elastic-tools-multi-tenant-row-level-security)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [guías de procedimientos en Instancia administrada](sql-database-howto-managed-instance.md).
- Obtenga más información sobre las [guías de procedimientos en Base de datos única](sql-database-howto-single-database.md).
