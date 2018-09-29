---
title: Almacenamiento de copias de seguridad de Azure SQL Database durante diez años como máximo | Microsoft Docs
description: Obtenga información acerca de cómo Azure SQL Database admite almacenar copias de seguridad de bases de datos completas durante un máximo de 10 años.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: b2d4166c25014416efbb7392acda3f3f028b4fa9
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162056"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Almacenamiento de copias de seguridad de Azure SQL Database durante diez años como máximo

Muchas aplicaciones tienen finalidades normativas, de conformidad u otras de carácter empresarial que requieren que se conserven copias de seguridad de las bases de datos más allá de entre los 7 y 35 días proporcionados por las [copias de seguridad automáticas](sql-database-automated-backups.md) de Azure SQL Database. Con la característica Retención a largo plazo, puede almacenar copias de seguridad completas de SQL Database en Blob Storage de [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) durante un máximo de 10 años. Así, podrá restaurar cualquier copia de seguridad como si fuera una base de datos nueva.

## <a name="how-sql-database-long-term-retention-works"></a>Funcionamiento de la retención a largo plazo de SQL Database

La retención a largo plazo aprovecha las [copias de seguridad automáticas de SQL Database](sql-database-automated-backups.md) creadas para la restauración a un momento dado (PITR). Puede configurar una directiva de retención a largo plazo para cada base de datos SQL y especificar con qué frecuencia debe copiar las copias de seguridad en el almacenamiento a largo plazo. Para habilitar la flexibilidad, puede definir la directiva mediante una combinación de cuatro parámetros: retención de copia de seguridad semanal (W), retención de copia de seguridad mensual (M), retención de copia de seguridad anual (Y) y la semana del año (WeekOfYear). Si especifica W, se copiará una copia de seguridad cada semana en el almacenamiento a largo plazo. Si especifica M, se copiará una copia de seguridad durante la primera semana de cada mes en el almacenamiento a largo plazo. Si especifica Y, se copiará una copia de seguridad durante la semana especificada en WeekOfYear en el almacenamiento a largo plazo. Cada copia de seguridad se mantendrá en el almacenamiento a largo plazo durante el período especificado por estos parámetros. 

Ejemplos:

-  W=0, M=0, Y=5, WeekOfYear=3

   La tercera copia de seguridad completa de cada año se conservará durante 5 años.
- W=0, M=3, Y=0

   La primera copia de seguridad completa de cada mes se conservará durante 3 meses.

- W=12, M=0, Y=0

   Cada copia de seguridad completa semanal se conservará durante 12 semanas.

- W=6, M=12, Y=10, WeekOfYear=16

   Cada copia de seguridad completa semanal se conservará durante 6 semanas. Excepto la primera copia de seguridad completa de cada mes, que se conservará durante 12 meses. Excepto la copia de seguridad completa realizada en la 16ª semana del año, que se conservará durante 10 años. 

En la tabla siguiente se muestra la cadencia y caducidad de las copias de seguridad a largo plazo para la siguiente directiva:

W=12 semanas (84 días), M=12 meses (365 días), Y=10 años (3650 días), WeekOfYear=15 (semana posterior al 15 de abril)

   ![Ejemplo de LTR](./media/sql-database-long-term-retention/ltr-example.png)


 
Si tuviera que modificar la directiva anterior y establece W=0 (sin copias de seguridad semanales), la cadencia de las copias de seguridad cambiaría tal y como se muestra en la tabla anterior en función de las fechas destacadas. La cantidad de almacenamiento necesaria para mantener estas copias de seguridad se reduciría según corresponda. 

> [!NOTE]
1. Las copias de LTR se crean mediante el servicio de almacenamiento de Azure, por lo que el proceso de copia no tiene ningún impacto en el rendimiento en la base de datos existente.
2. La directiva se aplica a las copias de seguridad futuras. Por ejemplo, Si el valor de WeekOfYear especificado pertenece al pasado al configurar la directiva, la primera copia de seguridad de LTR se creará el próximo año. 
3. Para restaurar una base de datos desde el almacenamiento de LTR, puede seleccionar una copia de seguridad específica en función de su marca de tiempo.   La base de datos se puede restaurar en cualquier servidor existente en la misma suscripción que la base de datos original. 
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Retención de copia de seguridad a largo plazo y replicación geográfica

Si usa grupos de conmutación por error o de replicación geográfica activa como su solución de continuidad del negocio, debe prepararse para posibles conmutaciones por error y configurar la misma directiva LTR en la base de datos geográfica secundaria. Esto no aumentará el costo de almacenamiento LTR ya que las copias de seguridad no se generan desde las bases de datos secundarias. Solo cuando la base de datos secundaria se convierte en principal se crearán las copias de seguridad. De este modo, garantizará la generación de las copias de seguridad LTR ininterrumpida cuando se desencadene la conmutación por error y la base de datos principal se mueva a la región secundaria. 

> [!NOTE]
Cuando la base de datos principal original se recupere de la interrupción que provoque su conmutación por error, se convertirá en una nueva base de datos secundaria. Por lo tanto, no se reanudará la creación de copia de seguridad y la directiva LTR existente no surtirá efecto hasta que vuelva a ser la base de datos principal de nuevo. 
> 

## <a name="configure-long-term-backup-retention"></a>Configuración de la retención de copia de seguridad a largo plazo

Para obtener información sobre cómo configurar la retención a largo plazo mediante Azure Portal o mediante PowerShell, consulte [Configure long-term backup retention](sql-database-long-term-backup-retention-configure.md) (Configuración de la retención de copia de seguridad a largo plazo).

## <a name="next-steps"></a>Pasos siguientes

Dado que las copias de seguridad de bases de datos protegen los datos de eliminaciones o daños accidentales, son una parte esencial de cualquier estrategia de recuperación ante desastres y de continuidad empresarial. Para descubrir otras soluciones de continuidad empresarial de SQL Database, consulte el artículo de [información general sobre la continuidad empresarial](sql-database-business-continuity.md).
