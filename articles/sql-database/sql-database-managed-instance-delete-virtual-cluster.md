---
title: Eliminación de la red virtual después de eliminar una instancia administrada de Azure SQL Database | Microsoft Docs
description: Obtenga información sobre cómo eliminar la red virtual después de eliminar una instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: management
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: 61f6c25031c4906e65c2f75a7679600741e8311a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65791373"
---
# <a name="delete-subnet-after-deleting-azure-sql-database-managed-instance"></a>Eliminación de la subred después de eliminar una instancia administrada de Azure SQL Database

En este artículo se proporcionan instrucciones sobre cómo eliminar manualmente la subred después de eliminar la última instancia administrada de Azure SQL Database que reside en ella.

El [clúster virtual](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) que contenía la instancia administrada eliminada se conservará durante 12 horas tras la eliminación de la instancia. El clúster virtual se mantiene activo por cuestiones de diseño para habilitar la rápida creación de instancias administradas en la misma subred. Mantener un clúster virtual vacío es gratuito. Durante este período, no se puede eliminar la subred asociada con el clúster virtual.

La liberación inmediata de la subred usada por un clúster virtual es posible mediante la eliminación manual del clúster virtual. El clúster virtual se puede eliminar mediante Azure Portal o la API de clústeres virtuales.

> [!NOTE]
> Para que la eliminación se realice correctamente, el clúster virtual no debe contener ninguna instancia administrada.

## <a name="delete-virtual-cluster-from-azure-portal"></a>Eliminación de un clúster virtual desde Azure Portal

Para eliminar un clúster virtual mediante Azure Portal, busque los recursos de clúster virtual con la búsqueda integrada.

![Busque el clúster virtual.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Una vez que localice el clúster virtual que desea eliminar, seleccione este recurso y seleccione la opción Eliminar. Se le pedirá que confirme la eliminación del clúster virtual.

![Elimine el clúster virtual.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

En las notificaciones de Azure Portal encontrará la confirmación de que el clúster virtual se ha eliminado. La eliminación correcta del clúster virtual libera inmediatamente la subred para volver a usarla.

## <a name="delete-virtual-cluster-using-api"></a>Eliminación de un clúster virtual mediante la API

Para eliminar un clúster virtual mediante la API, utilice los parámetros de URI que se especifican en el [método de eliminación de clústeres virtuales](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada?](sql-database-managed-instance.md)
- Obtenga información sobre la [arquitectura de conectividad en Instancia administrada](sql-database-managed-instance-connectivity-architecture.md).
- Aprenda a [modificar una red virtual existente para Instancia administrada](sql-database-managed-instance-configure-vnet-subnet.md).
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una instancia administrada y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started.md).
- Para ver información relativa a los distintos problemas de DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md).
