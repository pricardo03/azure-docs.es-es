---
title: Notas de la base de datos SQL de Azure | Microsoft Docs
description: Obtenga información sobre las nuevas características y mejoras en el servicio de Azure SQL Database y en la documentación de Azure SQL Database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: carlrab
ms.openlocfilehash: 6600a578ba9c73c8a2c71466fd0b008f19058b80
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57861303"
---
# <a name="sql-database-release-notes"></a>Notas de la base de datos SQL

En este artículo se enumera las nuevas características y mejoras en el servicio de base de datos SQL y en la documentación de la base de datos SQL. Para mejorar el servicio SQL Database, consulte [actualizaciones del servicio de base de datos SQL](https://azure.microsoft.com/updates/?product=sql-database). Mejoras a otros servicios de Azure, consulte [las actualizaciones del servicio](https://azure.microsoft.com/updates).

## <a name="march-2019"></a>Marzo de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| Próximamente ||
| &nbsp; |

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Mejoras en la documentación | Detalles |
| --- | --- |
| Límites del registro se ha agregado para bases de datos únicas|Para obtener más información, consulte [límites de recursos de memoria con núcleo virtual de base de datos únicas](sql-database-vcore-resource-limits-single-databases.md).|
| Límites del registro se ha agregado para grupos elásticos y bases de datos agrupadas|Para obtener más información, consulte [los límites de recursos de memoria con núcleo virtual de los grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).|
| Se ha agregado gobierno de tasa de registro transacciones| Agrega contenido nuevo para [gobierno de tasa de registro de transacciones](sql-database-resource-limits-database-server.md#transaction-log-rate-governance).|
| Ejemplos de PowerShell actualizados para bases de datos únicas y grupos elásticos para usar el módulo az.sql | Para obtener más información, consulte [ejemplos de PowerShell para grupos elásticos y bases de datos únicas](sql-database-powershell-samples.md#single-database-and-elastic-pools).|
| &nbsp; |

## <a name="february-2019"></a>Febrero de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
|Crear un índice en línea reanudable ahora está disponible con carácter general| Para obtener más información, consulte [Create Index](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql).|
|Compatibilidad con instancia administrada ha mejorado de las tablas de rutas| Para obtener más información, consulte [requisitos de red](sql-database-managed-instance-connectivity-architecture.md#network-requirements).|
|Cambio de nombre de base de datos compatible con instancia administrada | Para obtener más información, consulte el [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) y [sp_rename](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rename-transact-sql) sintaxis.|
|Base de datos de SQL como origen de datos de referencia para Stream Analytics. | Para obtener más información, consulte [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).|
|Asistencia para la migración de datos agrega compatibilidad para la instancia administrada. |Para obtener más información, consulte [cuáles son las novedades en DMA](https://docs.microsoft.com/sql/dma/dma-whatsnew).|
|SQL Server Migration Assistant agrega compatibilidad para la evaluación de disponibilidad de destino para la instancia administrada. | Para obtener más información, consulte [SQL Server Migration Assistant](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant).
|Servicio de migración de datos admite la migración desde Amazon RDS a instancia administrada | Para más información, consulte [Tutorial: Migración de SQL Server de RDS a Azure SQL Database o en línea una instancia administrada de Azure SQL Database mediante DMS](../dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Mejoras en la documentación | Detalles |
| --- | --- |
|Agregar administrados aclaraciones de opción de implementación de instancia|Actualizar muchos artículos para clarificar la aplicabilidad a la base de datos única, grupo elástico y las opciones de implementación instancia administrada. |
|Tamaños de tempdb actualizada para el modelo de compra basado en DTU | Para obtener más información, consulte [base de datos Tempdb en SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
|Actualizado importación y exportación con el archivo bacpac para la compatibilidad con instancia administrada| Para obtener más información, consulte [importar desde BACPAC](sql-database-import.md) y [exportar al BACPAC](sql-database-export.md). |
| &nbsp; |


## <a name="january-2019"></a>Enero de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| Opciones de granularidad adicional para los recursos de proceso | El propósito general y crítico para la empresa de servicio para los niveles de [bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md) y [grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md) ahora tiene más opciones de proceso específica.|
| Ver los registros de auditoría para la instancia administrada en Azure portal | Ver [registros para las instancias administradas de auditoría](sql-database-managed-instance-auditing.md) en Azure portal ahora se admite.|
| Característica de detección de amenazas avanzada ha cambiado a seguridad avanzada de datos | Cambiar el nombre de característica de detección de amenazas avanzada para [seguridad avanzada de datos](sql-advanced-threat-protection.md) para instancias administradas, grupos elásticos y bases de datos únicas. |
| &nbsp; |

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Mejoras en la documentación | Detalles |
| --- | --- |
| Las instancias administradas y la replicación transaccional | Se ha agregado artículo sobre el uso de [la replicación transaccional con las instancias administradas](replication-with-sql-database-managed-instance.md) |
| Se ha agregado a Azure AD con el tutorial de la instancia administrada | Esto [Azure AD con instancia administrada](sql-database-managed-instance-aad-security-tutorial.md) tendrá que configurar y probar el tutorial se muestra administra la seguridad de instancia mediante inicios de sesión de Azure AD. |
| Contenido actualizado de automatización de trabajos mediante scripts de Transact-SQL | Actualiza y se ha aclarado el contenido para el uso de [automatización del trabajo con scripts de Transact-SQL](sql-database-job-automation-overview.md) para instancias administradas, grupos elásticos y bases de datos únicas |
| Contenido de seguridad para las instancias administradas actualizado | Actualiza y se ha aclarado el contenido de la [modelo de seguridad para las instancias administradas](sql-database-security-overview.md)y diferencias con el modelo de seguridad de bases de datos únicas y grupos elásticos |
| Actualiza todos los inicios rápidos y tutoriales | Todos los inicios rápidos y tutoriales en la [documentación](https://docs.microsoft.com/azure/sql-database) se han actualizado y se actualiza para que coincida con los cambios en el portal de Azure |
| Guías de inicio rápido se ha agregado información general | Agrega una guía de información general de la Guía de inicio rápido para [bases de datos únicas](sql-database-quickstart-guide.md) y para [las instancias administradas](sql-database-managed-instance-quickstart-guide.md) |
| Glosario de base de datos SQL se ha agregado de términos | Esto [Glosario de términos](sql-database-glossary-terms.md) artículo proporciona una lista definitiva de los términos de base de datos SQL y vínculos a la página principal de conceptual que explica el término en el contexto. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>Contribuir a la mejora de contenido

El conjunto de documentación de SQL Azure es código abierto. Trabajar en el cuadro Abrir proporciona varias ventajas:

- Repositorios de código abierto se planean para obtener comentarios sobre qué documentos son más necesarios.
- Repositorios de código abierto se revisan para publicar el contenido más útil en la primera versión.
- Repositorios de código abierto se actualizan para que sea más fácil mejorar continuamente el contenido.

Para contribuir al contenido de la documentación de Azure SQL Database, consulte el [Introducción a la Guía de colaboradores de Docs Microsoft](https://docs.microsoft.com/contribute/). La experiencia del usuario en [docs.microsoft.com](https://docs.microsoft.com/) integra [GitHub](https://github.com/) flujos de trabajo directamente para que sea más fácil. Empiece por [editando el documento que está viendo](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). O bien, para ayudar al [revisar nuevos temas](https://docs.microsoft.com/contribute/#review-open-prs), o [crear problemas de calidad](https://docs.microsoft.com/contribute/#create-quality-issues).
