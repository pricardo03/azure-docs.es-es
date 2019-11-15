---
title: Pausa, reanudación y escalado con API REST
description: Administre la potencia de proceso en Azure SQL Data Warehouse mediante las API REST.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/29/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: f72b3fd1024a68a6f48d2e9e676fc7ca23bf2a4f
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686043"
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>API REST para Azure SQL Data Warehouse
API REST para administrar los procesos en Azure SQL Data Warehouse.

## <a name="scale-compute"></a>Escalado de proceso
Para cambiar las unidades de almacenamiento de datos, use la API REST para [crear o actualizar la base de datos](/rest/api/sql/databases/createorupdate). En el ejemplo siguiente se establecen las unidades de almacenamiento de datos en DW1000 para la base de datos MySQLDW, que está hospedada en el servidor MyServer. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute"></a>Pausa del proceso

Para pausar una base de datos, utilice la API de REST [Pausar la base de datos](/rest/api/sql/databases/pause) . El siguiente ejemplo pausa una base de datos denominada Database02 que está hospedada en un servidor llamado Server01. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Reanudación del proceso

Para iniciar una base de datos, utilice la API de REST [Reanudar la base de datos](/rest/api/sql/databases/resume) . El siguiente ejemplo inicia una base de datos denominada Database02 que está hospedada en un servidor llamado Server01. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Comprobar el estado de la base de datos

> [!NOTE]
> Actualmente, la comprobación del estado de la base de datos podría devolver ONLINE mientras se está completando el flujo de trabajo en línea, lo que produce errores de conexión. Es posible que deba agregar un retraso de 2 a 3 minutos en el código de aplicación si utiliza esta llamada de API para desencadenar los intentos de conexión.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>Obtención de la programación de mantenimiento
Se comprueba la programación de mantenimiento que se ha establecido para un almacén de datos. 

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Establecimiento de la programación de mantenimiento
Se establece y actualiza una programación de mantenimiento en un almacén de datos existente.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": Saturday,
                                "startTime": 00:00,
                                "duration": 08:00,
                },
                {
                                "dayOfWeek": Wednesday
                                "startTime": 00:00,
                                "duration": 08:00,
                }
                ]
    }
}

```


## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte [Administración de proceso en Azure SQL Data Warehouse](sql-data-warehouse-manage-compute-overview.md).

