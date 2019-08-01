---
title: Eliminación de una subred después de eliminar una instancia administrada de Azure SQL Database | Microsoft Docs
description: Obtenga información sobre cómo eliminar una red virtual de Azure después de eliminar una instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
ms.date: 06/26/2019
ms.openlocfilehash: f6e0b55ad2fbd9b4c45dbd1facaebd4750314c63
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567538"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Eliminación de una subred después de eliminar una instancia administrada de Azure SQL Database

En este artículo se proporcionan instrucciones sobre cómo eliminar manualmente una subred después de eliminar la última instancia administrada de Azure SQL Database que reside en ella.

SQL Database emplea un [clúster virtual](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) para contener la instancia administrada eliminada. El clúster virtual persiste durante 12 horas después de la eliminación de la instancia, para que pueda crear rápidamente instancias administradas en la misma subred. No hay ningún cargo por mantener un clúster virtual vacío. Durante este período, no se puede eliminar la subred asociada con el clúster virtual.

Si no desea esperar 12 horas y prefiere eliminar el clúster virtual y su subred inmediatamente, puede hacerlo manualmente. Puede eliminar manualmente el clúster virtual mediante Azure Portal o la API de clústeres virtuales.

> [!NOTE]
> Para que la eliminación se realice correctamente, el clúster virtual no debe contener ninguna instancia administrada.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Eliminación de un clúster virtual desde Azure Portal

Para eliminar un clúster virtual mediante Azure Portal, busque los recursos del clúster virtual.

![Captura de pantalla de Azure Portal con el cuadro de búsqueda resaltado](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Una vez que localice el clúster virtual que desea eliminar, seleccione este recurso y seleccione **Eliminar**. Se le pedirá que confirme la eliminación del clúster virtual.

![Captura de pantalla del panel de clústeres virtuales de Azure Portal con la opción Eliminar resaltada](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

El área de notificación de Azure Portal muestra la confirmación de que el clúster virtual se ha eliminado. La eliminación correcta del clúster virtual libera inmediatamente la subred para volver a usarla.

> [!TIP]
> Si no se muestran instancias administradas en el clúster virtual y no puede eliminar el clúster virtual, asegúrese de que no tiene una implementación de instancia en curso. Esto incluye las implementaciones iniciadas y canceladas que siguen en curso. Revise la pestaña Implementaciones del grupo de recursos en el que se implementó la instancia para ver si hay implementaciones en curso. En este caso, espere a que la implementación finalice, elimine la instancia administrada y, a continuación, el clúster virtual.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Eliminación de un clúster virtual mediante la API

Para eliminar un clúster virtual mediante la API, utilice los parámetros de URI que se especifican en el [método de eliminación de clústeres virtuales](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada?](sql-database-managed-instance.md)
- Obtenga información sobre la [arquitectura de conectividad en Instancia administrada](sql-database-managed-instance-connectivity-architecture.md).
- Aprenda a [modificar una red virtual existente para Instancia administrada](sql-database-managed-instance-configure-vnet-subnet.md).
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una instancia administrada y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started.md).
- Para ver información relativa a los distintos problemas de DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md).
