---
title: 'Azure SQL Database: Uso general y Crítico para la empresa | Microsoft Docs'
description: El artículo explica el uso general y los niveles de servicio críticos de negocio en el modelo de compra basado en núcleos virtuales.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 02/23/2019
ms.openlocfilehash: e5426bb7c8eba9d58dbf0472360c6ce0b19c9bc4
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431329"
---
# <a name="azure-sql-database-service-tiers"></a>Niveles de servicio de Azure SQL Database

La base de datos de SQL Azure se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta para que el entorno de nube garantizar la disponibilidad del 99,99%, incluso si hay un error de infraestructura. Tres niveles de servicio se usan en Azure SQL Database, cada uno con un modelo arquitectónico diferente. Estos niveles de servicio son:

- [Uso general](sql-database-service-tier-general-purpose.md), que está diseñado para cargas de trabajo más genéricos.
- [Crítico para la empresa](sql-database-service-tier-business-critical.md), que está diseñado para cargas de trabajo de baja latencia con una réplica legible.
- [Hiperescala](sql-database-service-tier-hyperscale.md), que está diseñado para bases de datos muy grandes (hasta 100 TB) con varias réplicas legibles.

Este artículo describen las consideraciones de copia de seguridad y almacenamiento para el uso general y los niveles de servicio críticos de negocio en el modelo de compra basado en núcleos virtuales.

> [!NOTE]
> Para obtener información sobre el nivel de servicio a gran escala en el modelo de compra basado en núcleos virtuales, consulte [nivel de servicio de hiperescala](sql-database-service-tier-hyperscale.md). Para ver una comparación entre el modelo de compra basado en núcleo virtual y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Almacenamiento de datos y de registro

Los siguientes factores afectan a la cantidad de almacenamiento utilizado para los archivos de datos y de registro:

- Se usa el almacenamiento asignado por los archivos de datos (MDF) y los archivos de registro (LDF).
- Cada base de datos única de proceso tamaño admite un tamaño máximo de la base de datos, con un tamaño máximo predeterminado de 32 GB.
- Al configurar el tamaño necesario de base de datos única (el tamaño del archivo MDF), se agrega automáticamente el 30 por ciento más de almacenamiento adicional para admitir archivos LDF.
- El tamaño de almacenamiento para una instancia administrada de SQL Database debe especificarse en múltiplos de 32 GB.
- Puede seleccionar cualquier tamaño de base de datos única entre 10 GB y el máximo admitido.
  - Para almacenamiento en los niveles de servicio de uso general o estándar, aumente o disminuya el tamaño en incrementos de 10 GB.
  - Para el almacenamiento en premium o los niveles de servicio críticos de negocio, aumentar o disminuir el tamaño en incrementos de 250 GB.
- En el nivel de servicio de uso general `tempdb` usa una SSD asociada y este costo de almacenamiento se incluye en el precio del núcleo virtual.
- En el nivel de servicio críticos empresariales `tempdb` comparte la SSD asociada con los archivos MDF y LDF y el `tempdb` costo de almacenamiento se incluye en el precio del núcleo virtual.

> [!IMPORTANT]
> Se le cobrará el almacenamiento total asignado para los archivos MDF y LDF.

Para supervisar el tamaño total actual de los archivos MDF y LDF, use [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para supervisar el tamaño actual de los archivos MDF y LDF individuales, use [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para obtener más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Copias de seguridad y almacenamiento

Se asigna almacenamiento para las copias de seguridad de base de datos para admitir la restauración en un momento (PITR) y [retención a largo plazo (LTR)](sql-database-long-term-retention.md) capacidades de base de datos de SQL. Este almacenamiento se asigna por separado para cada base de datos y se factura como dos cargos independientes por base de datos.

- **PITR**: Las copias de seguridad de base de datos individuales se copian en [almacenamiento de acceso de lectura con redundancia geográfica (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md) automáticamente. El tamaño de almacenamiento aumenta dinámicamente cuando se crean nuevas copias de seguridad. Se usa el almacenamiento, copias de seguridad completas semanales, copias de seguridad diferenciales diarias y copias del registro de transacciones, que se copian cada 5 minutos. El consumo de almacenamiento depende de la tasa de cambio de la base de datos y el período de retención de copias de seguridad. Puede configurar un período de retención diferente para cada base de datos de entre 7 y 35 días. Se proporciona una cantidad de almacenamiento mínimo igual al 100 por ciento (1 x) de tamaño de la base de datos sin costo adicional. En la mayoría de las bases de datos, esta cantidad es suficiente para almacenar copias de seguridad durante 7 días.
- **LTR**: SQL Database ofrece la opción de configurar la retención a largo plazo de copias de seguridad completas hasta 10 años. Si configura una directiva LTR, estas copias de seguridad se almacenan en almacenamiento RA-GRS automáticamente, pero puede controlar con qué frecuencia se copian las copias de seguridad. Para cumplir los requisitos de cumplimiento diferentes, puede seleccionar diferentes períodos de retención para copias de seguridad semanales, mensuales o anuales. La configuración que elija determina la cantidad de almacenamiento se usará para las copias de seguridad LTR. Para calcular el costo del almacenamiento LTR, puede usar la calculadora de precios de LTR. Para obtener más información, consulte [retención a largo plazo de SQL Database](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Pasos siguientes

- Para detalles sobre específico del proceso de tamaños y tamaños de almacenamiento disponibles para una sola base de datos en el uso general y los niveles de servicio críticos de negocio, consulte [límites de recursos basado en núcleos virtuales de SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md).
- Para detalles sobre específico del proceso de tamaños y los tamaños de almacenamiento disponibles para los grupos elásticos en el uso general y los niveles de servicio críticos de negocio, consulte [límites de recursos basado en núcleos virtuales de base de datos SQL para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).
