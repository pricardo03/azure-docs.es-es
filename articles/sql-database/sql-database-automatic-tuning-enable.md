---
title: Habilitación del ajuste automático para Azure SQL Database | Microsoft Docs
description: Puede habilitar fácilmente el ajuste automático en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 10/05/2018
ms.openlocfilehash: e5f1f2fe3745ae2817b73b2500ee90a59fc283e1
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53607088"
---
# <a name="enable-automatic-tuning-to-monitor-queries-and-improve-workload-performance"></a>Habilitación del ajuste automático para supervisar las consultas y mejorar el rendimiento de la carga de trabajo

Azure SQL Database es un servicio de datos completamente administrado que supervisa constantemente las consultas e identifica la acción que puede realizar para mejorar el rendimiento de la carga de trabajo. Puede revisar las recomendaciones y aplicarlas manualmente o dejar que Azure SQL Database aplique automáticamente acciones correctoras, lo que se conoce como **modo de ajuste automático**.

El ajuste automático puede habilitarse en el servidor o el nivel de base de datos a través de [Azure Portal](sql-database-automatic-tuning-enable.md#azure-portal), llamadas a la [API REST](sql-database-automatic-tuning-enable.md#rest-api) y comandos [T-SQL](sql-database-automatic-tuning-enable.md#t-sql).

## <a name="enable-automatic-tuning-on-server"></a>Habilitación del ajuste automático en servidor
En el nivel de servidor, puede optar por heredar la configuración de ajuste automático de "Valores predeterminados de Azure" o no heredarla. Los valores predeterminados de Azure son FORCE_LAST_GOOD_PLAN (habilitado), CREATE_INDEX (habilitado) y DROP_INDEX (deshabilitado).

### <a name="azure-portal"></a>Azure Portal
Para habilitar el ajuste automático en el **servidor** lógico de Azure SQL Database, vaya al servidor en Azure Portal y seleccione **Ajuste automático** en el menú.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Tenga en cuenta que, en este momento, la opción **DROP_INDEX** no es compatible con las aplicaciones que usan sugerencias de índice y la conmutación de particiones, y no debe estar habilitada en estos casos.
>

Seleccione las opciones de ajuste automático que quiere habilitar y haga clic en **Aplicar**.

Las opciones de ajuste automático de un servidor se aplican a todas las bases de datos que contiene. De forma predeterminada, todas las bases de datos heredan la configuración de su servidor primario; sin embargo, puede invalidar esta opción y configurar cada base de datos individualmente.

### <a name="rest-api"></a>API DE REST

Para más información sobre el uso de API REST para habilitar el ajuste automático en un servidor, consulte el artículo sobre el [ajuste automático en SQL Server de los métodos HTTP UPDATE y GET](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).


## <a name="enable-automatic-tuning-on-an-individual-database"></a>Habilitar el ajuste automático en una base de datos individual

Azure SQL Database le permite especificar individualmente la configuración de ajuste automático en cada base de datos. En el nivel de base de datos, puede decidir si desea heredar la configuración de ajuste automático del servidor primario, si desea heredarla de los valores predeterminados de Azure o si prefiere no heredarla. Los valores predeterminados de Azure son FORCE_LAST_GOOD_PLAN (habilitado), CREATE_INDEX (habilitado) y DROP_INDEX (deshabilitado).

> [!NOTE]
> La recomendación general es administrar la configuración de ajuste automático en el **nivel de servidor**, de forma que se pueda aplicar la misma configuración en todas las bases de datos automáticamente. Solo debe configurar el ajuste automático en una base de datos específica si necesita que esa base de datos tenga una configuración distinta a la configuración heredada del mismo servidor.
>

### <a name="azure-portal"></a>Azure Portal

Para habilitar el ajuste automático en una **única base de datos**, vaya a la base de datos en Azure Portal y seleccione **Ajuste automático**.

El ajuste automático se puede configurar por separado en cada base de datos. Puede configurar manualmente la opción de ajuste automático o especificar una opción que herede la configuración del servidor.

![Base de datos](./media/sql-database-automatic-tuning-enable/database.png)

Tenga en cuenta que, en este momento, la opción DROP_INDEX no es compatible con las aplicaciones que usan sugerencias de índice y la conmutación de particiones, y no debe estar habilitada en estos casos.

Cuando haya seleccionado la configuración que desee, haga clic en **Aplicar**.

### <a name="rest-api"></a>API de REST

Para más información sobre el uso de API REST para habilitar el ajuste automático en una base de datos única, consulte el artículo sobre el [ajuste automático en las bases de datos de SQL de los métodos HTTP UPDATE y GET](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning).

### <a name="t-sql"></a>T-SQL

Para habilitar el ajuste automático en una sola base de datos mediante T-SQL, conéctese a la base de datos y ejecute la consulta siguiente:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Si establece el ajuste automático en AUTO, se aplicarán los valores predeterminados de Azure. Si se establece en INHERIT, se heredará la configuración de ajuste automático del servidor primario. Si elige CUSTOM, deberá configurar manualmente el ajuste automático.

Para configurar las opciones individuales de ajuste automático mediante T-SQL, conéctese a la base de datos y ejecute una consulta como esta:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
Si establece la opción de ajuste individual en ON, invalidará todas las configuraciones que la base de datos haya heredado y habilitará la opción de ajuste. Si la establece en OFF, también invalidará todas las configuraciones que la base de datos haya heredado y deshabilitará la opción de ajuste. Las opciones de ajuste automático para las que se especifique DEFAULT heredarán la configuración de ajuste automático del nivel de base de datos.  

Para más información sobre las opciones de T-SQL para configurar el ajuste automático, consulte el artículo sobre las [opciones de ALTER DATABASE SET (Transact-SQL) para el servidor lógico de SQL Database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current).

## <a name="disabled-by-the-system"></a>Deshabilitado por el sistema
El ajuste automático supervisa todas las acciones que realiza en la base de datos y, en algunos casos, determina que no funciona correctamente en la base de datos. En esta situación, el sistema deshabilitará la opción de ajuste. En la mayoría de los casos, esto sucede porque el Almacén de consultas no está habilitado o se encuentra en estado de solo lectura en una base de datos específica.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Configuración de notificaciones por correo electrónico para el ajuste automático

Consulte la guía [Notificaciones por correo electrónico para el ajuste automático](sql-database-automatic-tuning-email-notifications.md).

## <a name="next-steps"></a>Pasos siguientes
* Para aprender más sobre el ajuste automático y cómo puede ayudarle a mejorar su rendimiento, lea el [artículo sobre este tema](sql-database-automatic-tuning.md).
* Consulte [Recomendaciones de rendimiento](sql-database-advisor.md) para ver información general sobre las recomendaciones de rendimiento de Azure SQL Database.
* Consulte [Query Performance Insight](sql-database-query-performance.md) para más información sobre el impacto en el rendimiento de las principales consultas.
