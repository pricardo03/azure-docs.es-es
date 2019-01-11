---
title: Parametrización de servicios vinculados en Azure Data Factory | Microsoft Docs
description: Aprenda a parametrizar los servicios vinculados en Azure Data Factory y a pasar valores dinámicos en tiempo de ejecución.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: douglasl
ms.openlocfilehash: af06be520b10184c1e96bdb27d0b8b648cde84a2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018460"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Parametrización de servicios vinculados en Azure Data Factory

Ahora puede parametrizar un servicio vinculado y pasar valores dinámicos en tiempo de ejecución. Por ejemplo, si desea conectarse a bases de datos diferentes en el mismo servidor de Azure SQL Database, ahora puede parametrizar el nombre de la base de datos en la definición del servicio vinculado. Esto evita tener que crear un servicio vinculado para cada base de datos en el servidor de Azure SQL Database. Por ejemplo, puede parametrizar otras propiedades en la definición de servicio vinculado también; por ejemplo, *Nombre de usuario.*

Puede usar la interfaz de usuario de Data Factory en Azure Portal o una interfaz de programación para parametrizar los servicios vinculados.

> [!TIP]
> Se recomienda no parametrizar las contraseñas ni los secretos. Almacene en cambio todas las cadenas de conexión en Azure Key Vault y parametrice el *nombre del secreto*.

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Almacenes de datos compatibles

En este momento, la parametrización de los servicios vinculados se admite en la interfaz de usuario de Data Factory en Azure Portal para los siguientes almacenes de datos. Para todos los demás almacenes de datos, puede parametrizar el servicio vinculado si selecciona el icono **Código** en la pestaña **Conexiones** y usa el editor de JSON.
- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server
- Oracle
- Cosmos DB
- Amazon Redshift
- MySQL
- Azure Database for MySQL

## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory

![Incorporación de contenido dinámico a la definición de servicio vinculado](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Crear un nuevo parámetro](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "value": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
                "type": "SecureString"
            }
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
