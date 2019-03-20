---
title: 'Escalar recursos de base de datos única: Azure SQL Database | Microsoft Docs'
description: En este artículo se describe cómo escalar los recursos de proceso y almacenamiento disponibles para una base de datos única en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 03/14/2019
ms.openlocfilehash: 02dcdfa6f356d48b8fa22603323a7f3035e0fe51
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57858777"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Escalar recursos de base de datos única en Azure SQL Database

En este artículo se describe cómo escalar los recursos de proceso y almacenamiento disponibles para una base de datos única en Azure SQL Database.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

## <a name="change-compute-resources-vcores-or-dtus"></a>Cambiar los recursos de proceso (núcleos virtuales o Dtu)

Después de elegir inicialmente el número de Dtu o de núcleos virtuales, puede escalar una base de datos única hacia arriba o abajo dinámicamente en función de la experiencia real mediante la [portal Azure](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [ PowerShell](/powershell/module/az.sql/set-azsqldatabase), [CLI de Azure](/cli/azure/sql/db#az-sql-db-update), o el [API de REST](https://docs.microsoft.com/rest/api/sql/databases/update).

El vídeo siguiente muestra cómo cambiar de manera dinámica el nivel de servicio y el tamaño de proceso para aumentar las DTU disponibles para una base de datos única.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Impacto de cambiar el tamaño de proceso de nivel o cambiar la escala de servicio

Cambiar el servicio de nivel o calcular el tamaño de una sola base de datos implica principalmente el servicio siguiendo estos pasos:

1. Crear nueva instancia de proceso para la base de datos  

    Se crea una nueva instancia de proceso para la base de datos con el tamaño de proceso y el nivel de servicio solicitado. Para algunas combinaciones de nivel de servicio y los cambios de tamaño de proceso, una réplica de la base de datos debe crearse en la nueva instancia de proceso que implica copiar los datos y puede influir notoriamente en la latencia total. En cualquier caso, la base de datos permanece en línea durante este paso y continuarán conexiones para que se dirijan a la base de datos en la instancia de proceso original.

2. Cambiar el enrutamiento de conexiones a la nueva instancia de proceso

    Se quitan las conexiones existentes con la base de datos en la instancia de proceso original. Las nuevas conexiones se establecen en la base de datos en la nueva instancia de proceso. Para algunas combinaciones de nivel de servicio y los cambios de tamaño de proceso, los archivos de base de datos se separa y volver a adjuntar durante la conmutación.  No obstante, el conmutador puede provocar una breve interrupción del servicio cuando la base de datos no está disponible con carácter general para menos de 30 segundos y a menudo solo unos segundos. Si no hay de larga ejecución transacciones que se ejecutan cuando se quitan conexiones, la duración de este paso puede tardar más tiempo con el fin de recuperar las transacciones anuladas. [Acelerado de recuperación de base de datos](sql-database-accelerated-database-recovery.md) puede reducir el impacto de anulación de transacciones de larga ejecución.

> [!IMPORTANT]
> No se pierden datos durante los pasos del flujo de trabajo.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Latencia del cambio de tamaño de proceso de nivel o cambiar la escala de servicio

La latencia para cambiar el nivel de servicio o cambiar el tamaño de proceso de una sola base de datos o el grupo elástico se parametriza como sigue:

|Nivel de servicio|Básica única base de datos</br>Estándar (S0-S1)|Grupo elástico básico,</br>Estándar (S2-S12) </br>A gran escala, </br>Finalidad única base de datos general o del grupo elástico|Base de datos única Premium o crítico para la empresa o el grupo elástico|
|:---|:---|:---|:---|
|**Base de datos único básica,</br> estándar (S0-S1)**|&bull; &nbsp;Latencia de tiempo constante independientemente del espacio utilizado</br>&bull; &nbsp;Normalmente, menos de 5 minutos|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|
|**Grupo elástico básico, </br>estándar (S2-S12) </br>a gran escala, </br>única base de datos de uso General o el grupo elástico**|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia de tiempo constante independientemente del espacio utilizado</br>&bull; &nbsp;Normalmente, menos de 5 minutos|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|
|**Base de datos única Premium o crítico para la empresa o el grupo elástico**|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|&bull; &nbsp;Latencia proporcional al espacio de base de datos puede utilizar debido a copiar los datos</br>&bull; &nbsp;Normalmente, menos de 1 minuto por GB de espacio usado|

> [!TIP]
> Para supervisar las operaciones en curso, consulte: [Administración de operaciones mediante la API REST de SQL](https://docs.microsoft.com/rest/api/sql/operations/list), [Administración de operaciones mediante la CLI](/cli/azure/sql/db/op), [Supervisión de operaciones mediante T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) y estos dos comandos de PowerShell: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) y [Stop AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Consideraciones adicionales cuando se cambia de servicio de nivel o cambiar la escala de tamaño de proceso

- Si va a actualizar a un nivel de servicio o tamaño de proceso más elevado, el tamaño máximo de la base de datos no aumenta a no ser que especifique un tamaño mayor (maxsize).
- Para cambiar una base de datos a una versión anterior, su espacio usado no debe alcanzar el tamaño máximo permitido del nivel de servicio de destino y del tamaño de proceso.
- Al pasar del nivel **Premium** al nivel **Estándar**, se aplica un costo de almacenamiento adicional si (1) el tamaño máximo de la base de datos es compatible con el tamaño de proceso de destino y (2) el tamaño máximo supera la cantidad de almacenamiento incluido del tamaño de proceso de destino. Por ejemplo, si una base de datos P1 con un tamaño máximo de 500 GB se reduce a S3, se aplica un costo de almacenamiento adicional porque S3 admite un tamaño máximo de 500 GB y su cantidad de almacenamiento incluido es solo de 250 GB. Por lo tanto, la cantidad de almacenamiento adicional es 500 GB – 250 GB = 250 GB. Para conocer el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Si la cantidad de espacio real utilizada es menor que la cantidad de almacenamiento incluido, este costo adicional puede evitarse si se reduce el tamaño máximo de la base de datos a la cantidad incluida.
- Al actualizar una base de datos con la [replicación geográfica](sql-database-geo-replication-portal.md) habilitada, actualice sus bases de datos secundarias al nivel de servicio y al tamaño de proceso deseados antes de actualizar la base de datos principal (instrucciones generales para mejorar el rendimiento). Al actualizar a una diferente, antes hay que actualizar la base de datos secundaria.
- Al cambiar a una versión anterior la base de datos con la [replicación geográfica](sql-database-geo-replication-portal.md) habilitada, se cambian también sus bases de datos principales al nivel de servicio y tamaño de proceso deseados antes de cambiar a una versión anterior la base de datos secundaria (regla general para un mejor rendimiento). Al cambiar a una versión anterior a una edición diferente, antes hay que cambiar la principal a una versión anterior.
- Las ofertas del servicio de restauración son diferentes para los distintos niveles de servicio. Si va a cambiar al nivel **Básico**, hay un período de retención de copia de seguridad más bajo. Consulte el artículo sobre [Copias de seguridad de Azure SQL Database](sql-database-automated-backups.md).
- Las nuevas propiedades de la base de datos no se aplican hasta que se completan los cambios.

### <a name="billing-during-rescaling"></a>Facturación durante el cambio de escala

Se le cobrará por cada hora que una base de datos exista con el mayor nivel de servicio + tamaño de proceso aplicable durante esa hora, independientemente del uso o de si la base de datos estuvo activa durante menos tiempo. Por ejemplo, si crea una base de datos única y la elimina a los cinco minutos, se le efectuará un cargo de una hora por usar la base de datos.

## <a name="change-storage-size"></a>Modificar el tamaño de almacenamiento

### <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleos virtuales

- Se puede aprovisionar el almacenamiento hasta el límite máximo de tamaño con incrementos de 1 GB. El almacenamiento de datos mínimo configurable es 5 GB
- Se puede aprovisionar almacenamiento para una base de datos única mediante el aumento o disminución de su tamaño máximo con [Azure Portal](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), la [CLI de Azure](/cli/azure/sql/db#az-sql-db-update) o la [API REST](https://docs.microsoft.com/rest/api/sql/databases/update).
- SQL Database asigna automáticamente 30 % de almacenamiento adicional para los archivos de registro y 32 GB por núcleo virtual para TempDB, sin exceder los 384 GB. TempDB se encuentra en una SSD asociada en todos los niveles de servicio.
- El precio de almacenamiento para una única base de datos es la suma de los importes de almacenamiento de datos y almacenamiento de registros multiplicado por el precio de la unidad de almacenamiento del nivel de servicio. El costo de TempDB se incluye en el precio del núcleo virtual. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

- El precio de la DTU para una base de datos única incluye una cierta cantidad de almacenamiento sin costo adicional. El almacenamiento adicional que supere la cantidad incluida se puede aprovisionar por un costo extra hasta el límite de tamaño máximo en incrementos de 250 GB hasta 1 TB, y luego en incrementos de 256 GB superando 1 TB. Para conocer los límites de tamaño máximo y cantidades de almacenamiento, consulte [Base de datos única: tamaños de almacenamiento y tamaños de proceso](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Se puede aprovisionar almacenamiento adicional para una base de datos única aumentando su tamaño máximo con Azure Portal, [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), la [CLI de Azure](/cli/azure/sql/db#az-sql-db-update) o la [API de REST](https://docs.microsoft.com/rest/api/sql/databases/update).
- El precio del almacenamiento adicional para una base de datos única es la cantidad de almacenamiento adicional multiplicado por el precio de la unidad de almacenamiento adicional del nivel de servicio. Para más información sobre el precio del almacenamiento adicional, consulte los [precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Modelo de compra basado en DTU: limitaciones de P11 y P15 cuando el tamaño máximo es superior a 1 TB

Existe más de 1 TB de almacenamiento en el nivel Premium actualmente disponible en todas las regiones excepto: Este de China, Norte de China, Centro de Alemania, Nordeste de Alemania, Centro-oeste de EE. UU., US regiones de US DoD y Centro de Gobierno de EE. UU. En estas regiones, el almacenamiento máximo en el nivel Prémium está limitado a 1 TB. Para más información, consulte las [limitaciones actuales de P11 y P15](sql-database-single-database-scale.md#dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb). Las siguientes consideraciones y limitaciones se aplican a las bases de datos P11 y P15 con un tamaño máximo mayor de 1 TB:

- Si elige un tamaño máximo mayor de 1 TB al crear una base de datos (con un valor de 4 TB o 4096 GB), el comando de creación produce un error si la base de datos se aprovisiona en una región no admitida.
- Para las bases de datos P11 y P15 existentes ubicadas en alguna de las regiones admitidas, puede aumentar el almacenamiento máximo hasta superar 1 TB en incrementos de 256 MB hasta 4 TB. Para ver si se admite un tamaño mayor en su región, utilice la función [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) o inspeccione el tamaño de la base de datos en Azure Portal. La actualización de una base de datos P11 o P15 existente solo puede realizarse mediante un inicio de sesión de entidad de seguridad de nivel de servidor o por los miembros del rol de base de datos dbmanager.
- Si se ha ejecutado una operación de actualización en una región admitida, la configuración se actualiza inmediatamente. La base de datos permanecerá en línea durante el proceso de actualización. Sin embargo, no puede utilizar la cantidad total de almacenamiento a partir de 1 TB de almacenamiento hasta que se hayan actualizado los archivos de base de datos reales con el nuevo tamaño máximo. El período de tiempo necesario depende del tamaño de la base de datos que se va a actualizar.
- Al crear o actualizar una base de datos P11 o P15, solo puede optar entre un tamaño máximo de 1 TB y 4 TB en incrementos de 256 GB. Cuando crea una base de datos P11 o P15, se selecciona previamente la opción de almacenamiento predeterminada de 1 TB. En el caso de bases de datos ubicadas en alguna de las regiones admitidas, puede aumentar el almacenamiento máximo hasta un máximo de 4 TB en bases de datos únicas nuevas o existentes. Para todas las demás regiones, el tamaño máximo no se puede aumentar por encima de 1 TB. El precio no cambia cuando se selecciona la opción de 4 TB de almacenamiento incluido.
- Si el tamaño máximo de una base de datos se establece en más de 1 TB, no podrá modificarse a 1 TB incluso si el almacenamiento real utilizado es inferior a 1 TB. Por lo tanto, no puede cambiar a una versión anterior una P11 o P15 con un tamaño máximo mayor que 1 TB a un P11 de 1 TB o a un P15 de 1 TB o un tamaño de proceso menor, por ejemplo, P1-P6). Esta restricción también se aplica a los escenarios de copia y restauración, lo que incluye la restauración a un momento dado, la restauración geográfica, la retención de copias de seguridad a largo plazo y la copia de base de datos. Cuando se configura una base de datos con un tamaño máximo de 1 TB, todas las operaciones de restauración de esta base de datos deben ejecutarse en una P11/P15 con un tamaño máximo mayor de 1 TB.
- En escenarios de replicación geográfica activa:
  - Configuración de una relación de replicación geográfica: si la base de datos principal es P11 o P15, la secundaria (una o varias) también debe ser P11 o P15; aquellas con un tamaño de proceso inferior se rechazarán como secundarias, puesto que no tienen capacidad para admitir más de 1 TB.
  - Actualización de la base de datos principal en una relación de replicación geográfica: al cambiar el tamaño máximo a 1 TB en una base de datos principal, se desencadenará el mismo cambio en la base de datos secundaria. Ambas actualizaciones deben realizarse correctamente para que el cambio en la principal surta efecto. Se aplican limitaciones por región para la opción de más de 1 TB. Si la base de datos secundaria está en una región que no admite más de 1 TB, no se actualizará la principal.
- No se admite el uso del servicio Import/Export para cargar bases de datos P11/P15 con más de 1 TB. Use SqlPackage.exe para [importar](sql-database-import.md) y [exportar](sql-database-export.md) datos.

## <a name="next-steps"></a>Pasos siguientes

Para conocer los límites de recursos globales, consulte [Límites de recursos basados en núcleos virtuales de Azure SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md) y [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-single-databases.md) (Límites de recursos basados en DTU de SQL Database para grupos elásticos).
