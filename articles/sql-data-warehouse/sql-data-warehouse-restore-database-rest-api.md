---
title: 'Restauración de Azure SQL Data Warehouse: API REST | Microsoft Docs'
description: Restauración de una instancia de Azure SQL Data Warehouse mediante la API REST.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: daf013472e5fa533912920e4c14a552905b5d333
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241525"
---
# <a name="restore-an-azure-sql-data-warehouse-with-rest-apis"></a>Restauración de una instancia de Azure SQL Data Warehouse con la API REST
Restauración de una instancia de Azure SQL Data Warehouse mediante la API REST.

## <a name="before-you-begin"></a>Antes de empezar
**Compruebe la capacidad DTU**. Cada instancia de SQL Data Warehouse está hospedada en un servidor SQL Server lógico (p. ej., myserver.database.windows.net) que tiene una [cuota de DTU](../sql-database/sql-database-what-is-a-dtu.md) predeterminada.  Antes de que pueda restaurar una instancia de SQL Data Warehouse, compruebe que su servidor SQL Server tiene suficientes cuotas de DTU restantes para la base de datos en proceso de restauración. Para solicitar más DTU, también puede [crear una incidencia de soporte técnico](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-an-active-or-paused-data-warehouse"></a>Restauración de un almacén de datos activo o en pausa
Para restaurar un almacén de datos:

1. Obtenga la lista de puntos de restauración de base de datos mediante la operación para obtener puntos de restauración de base de datos.
2. Inicie la restauración con la operación [Crear solicitud de restauración de base de datos](https://msdn.microsoft.com/library/azure/dn509571.aspx) .
3. Realice un seguimiento del estado de la restauración con la operación [Estado de operación de base de datos](https://msdn.microsoft.com/library/azure/dn720371.aspx) .

> [!NOTE]
> Una vez finalizada la restauración, puede configurar el almacén de datos recuperado siguiendo la guía [Configuración de la base de datos después de realizar la recuperación](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="restore-a-deleted-data-warehouse"></a>Restauración de un almacén de datos eliminado
Para restaurar un almacén de datos eliminado:

1. Enumere todos los almacenes de datos eliminados que se pueden restaurar con la operación [List restorable dropped databases](https://msdn.microsoft.com/library/azure/dn509562.aspx) (Enumeración de bases de datos eliminadas que se pueden restaurar).
2. Para obtener los detalles del almacén de datos eliminado que desea restaurar, utilice la operación [Get restorable dropped database][Get restorable dropped database](Obtención de la base de datos eliminada que se puede restaurar).
3. Inicie la restauración con la operación [Crear solicitud de restauración de base de datos](https://msdn.microsoft.com/library/azure/dn509571.aspx) .
4. Realice un seguimiento del estado de la restauración con la operación [Estado de operación de base de datos](https://msdn.microsoft.com/library/azure/dn720371.aspx) .

> [!NOTE]
> Para configurar la base de datos una vez finalizada la restauración, consulte [Configuración de la base de datos después de realizar la recuperación](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las características de continuidad empresarial de las ediciones de Azure SQL Database, consulte [Información general: continuidad del negocio en la nube y recuperación ante desastres con la SQL Database](../sql-database/sql-database-business-continuity.md).
