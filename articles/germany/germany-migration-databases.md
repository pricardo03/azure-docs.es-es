---
title: Migración de recursos de base de datos de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar los recursos de base de datos de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: bfa076f33ce3b2e7315a07717bba5768b89511c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033738"
---
# <a name="migrate-database-resources-to-global-azure"></a>Migración de recursos de base de datos a Azure global

En este artículo se proporciona información que puede ayudarle a migrar los recursos de base de datos de Azure desde Azure Alemania a Azure global.

## <a name="sql-database"></a>SQL Database

Para migrar las cargas de trabajo más pequeñas de Azure SQL Database, utilice la función de exportación para crear un archivo BACPAC. Un archivo BACPAC es un archivo comprimido (.zip) que contiene los metadatos y los datos de la base de datos de SQL Server. Después de crear el archivo BACPAC, puede copiar el archivo en el entorno de destino (por ejemplo, mediante el uso de AzCopy) y utilizar la función de importación para recompilar la base de datos. Tenga presente las siguientes consideraciones:

- Para que una exportación sea transaccionalmente coherente, asegúrese de que se cumple una de las siguientes condiciones:
  - No se produce ninguna actividad de escritura durante la exportación.
  - Realiza la exportación desde una copia transaccionalmente coherente de la base de datos SQL.
- Para exportar a Azure Blob Storage, el tamaño del archivo BACPAC se limita a 200 GB. Para un archivo BACPAC de mayor tamaño, exporte al almacenamiento local.
- Si la operación de exportación de SQL Database tarda más de 20 horas, es posible que se cancele la operación. Consulte los artículos siguientes para obtener sugerencias sobre cómo aumentar el rendimiento.

> [!NOTE]
> La cadena de conexión cambia después de la operación de exportación porque el nombre DNS del servidor cambia durante la exportación.

Para obtener más información:

- Aprenda a [exportar una base de datos a un archivo BACPAC](../sql-database/sql-database-export.md).
- Aprenda a [importar un archivo BACPAC en una base de datos](../sql-database/sql-database-import.md).
- Revise la [documentación de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sql-data-warehouse"></a>SQL Data Warehouse

Para migrar los recursos de Azure SQL Data Warehouse de Azure Alemania a Azure global, siga los pasos que se describen en Azure SQL Database.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Puede usar la herramienta de migración de datos de Azure Cosmos DB para migrar los datos a Azure Cosmos DB. La herramienta de migración de datos Azure Cosmos DB es una solución de código abierto que importa datos a Azure Cosmos DB desde una variedad de orígenes.

La herramienta de migración de datos en Azure Cosmos DB está disponible como una herramienta de interfaz gráfica o como herramienta de línea de comandos. El código fuente está disponible en el repositorio de GitHub de la [herramienta de migración de datos de Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool). Una [versión compilada de la herramienta](https://www.microsoft.com/download/details.aspx?id=46436) está disponible en el Centro de descarga de Microsoft.

Para migrar recursos de Azure Cosmos DB, se recomienda que complete los pasos siguientes:

1. Revise los requisitos de tiempo de actividad de la aplicación y las configuraciones de cuentas para determinar el mejor plan de acción.
1. Clone las configuraciones de la cuenta de Azure Alemania a la nueva región mediante la ejecución de la herramienta de migración de datos.
1. Si es posible usar una ventana de mantenimiento, copie los datos desde el origen al destino mediante la ejecución de la herramienta de migración de datos.
1. Si no se puede usar una ventana de mantenimiento, copie los datos desde el origen al destino mediante la ejecución de la herramienta y, después, complete estos pasos:
   1. Utilice un enfoque basado en la configuración para realizar cambios de lectura o escritura en una aplicación.
   1. Complete la sincronización por primera vez.
   1. Configure una sincronización incremental y póngase al día con la fuente de cambios.
   1. Lea en la nueva cuenta y valide la aplicación.
   1. Detenga el proceso de escritura en la cuenta antigua, valide que la fuente de cambios esté al día y, después, escriba en la nueva cuenta.
   1. Detenga la herramienta y elimine la cuenta antigua.
1. Ejecute la herramienta para validar que los datos son coherentes entre las cuentas antiguas y nuevas.

Para obtener más información:

- Lea una [introducción a Azure Cosmos DB](../cosmos-db/introduction.md).
- Aprenda cómo [importar datos en masa a Azure Cosmos DB](../cosmos-db/import-data.md).

## <a name="azure-cache-for-redis"></a>Azure Cache for Redis

Tiene algunas opciones si quiere migrar una instancia de Azure Cache for Redis de Azure Alemania a Azure global. La opción que elija depende de los requisitos.

### <a name="option-1-accept-data-loss-create-a-new-instance"></a>Opción 1: Aceptar la pérdida de datos, crear una nueva instancia

Este enfoque resulta idóneo cuando se cumplen las condiciones siguientes:

- Está usando Azure Cache for Redis como una caché de datos transitorios.
- La aplicación vuelve a llenar los datos de la memoria caché automáticamente en la nueva región.

Para migrar con pérdida de datos y crear una nueva instancia:

1. Cree una nueva instancia de Azure Cache for Redis en la nueva región de destino.
1. Actualice la aplicación para que utilice la nueva instancia de la nueva región.
1. Elimine la instancia anterior de Azure Cache for Redis en la región de origen.

### <a name="option-2-copy-data-from-the-source-instance-to-the-target-instance"></a>Opción 2: Copiar datos de la instancia de origen a la instancia de destino

Un miembro del equipo de Azure Cache for Redis ha escrito una herramienta de código abierto que copia datos de una instancia de Azure Cache for Redis a otra sin necesidad de importar o exportar la funcionalidad. Vea el paso 4 de los pasos siguientes para más información sobre la herramienta.

Para copiar los datos de la instancia de origen a la de destino:

1. Cree una máquina virtual en la región de origen. Si su conjunto de datos en Azure Cache for Redis es grande, asegúrese de seleccionar un tamaño de máquina virtual relativamente eficaz para minimizar el tiempo de copia.
1. Cree una nueva instancia de Azure Cache for Redis en la región de destino.
1. Vacíe los datos de la instancia de **destino**. (Asegúrese de que *no* se vacía desde la instancia de **origen**. El vaciado es necesario porque la herramienta de copia *no sobrescribe* las claves existentes en la ubicación de destino).
1. Utilice la siguiente herramienta para copiar automáticamente los datos de la instancia Azure Cache for Redis de origen a la instancia de Azure Cache for Redis de destino: [Origen de la herramienta](https://github.com/deepakverma/redis-copy) y [descarga de la herramienta](https://github.com/deepakverma/redis-copy/releases/download/alpha/Release.zip).

> [!NOTE]
> Este proceso puede tardar mucho tiempo según el tamaño del conjunto de datos.

### <a name="option-3-export-from-the-source-instance-import-to-the-destination-instance"></a>Opción 3: Exportar desde la instancia de origen, importar a la instancia de destino

Este enfoque aprovecha las características que solo están disponibles en el nivel Premium.

Para exportar de la instancia de origen e importar a la instancia de destino:

1. Cree un nuevo nivel Premium de la instancia de Azure Cache for Redis en la región de destino. Use el mismo tamaño que la instancia de Azure Cache for Redis de origen.
1. [Exporte los datos desde la caché de origen](../redis-cache/cache-how-to-import-export-data.md) o utilice el [cmdlet de PowerShell Export-AzRedisCache](/powershell/module/az.rediscache/export-azrediscache).

   > [!NOTE]
   > La cuenta de Azure Storage de exportación debe estar en la misma región que la instancia de la caché.

1. Copie los blobs exportados en una cuenta de almacenamiento de la región de destino (por ejemplo, mediante AzCopy).
1. [Importe los datos a la caché de destino](../redis-cache/cache-how-to-import-export-data.md) o utilice el [cmdlet de PowerShell Import-AzRedisCAche](/powershell/module/az.rediscache/import-azrediscache).
1. Vuelva a configurar la aplicación para que utilice la instancia de Azure Cache for Redis de destino.

### <a name="option-4-write-data-to-two-azure-cache-for-redis-instances-read-from-one-instance"></a>Opción 4: Escribir datos en dos instancias de Azure Cache for Redis, leer desde una instancia

Para este enfoque, debe modificar la aplicación. La aplicación tiene que escribir datos en más de una instancia de caché durante la lectura de una de las instancias de caché. Este enfoque tiene sentido si los datos almacenados en Azure Cache for Redis cumplen los criterios siguientes:
- Los datos se actualizan de forma periódica. 
- Todos los datos se escriben en la instancia de Azure Cache for Redis de destino.
- Se dispone de tiempo suficiente para que todos los datos se actualicen.

Para obtener más información:

- Revise la [introducción a Azure Cache for Redis](../redis-cache/cache-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Identidad](./germany-migration-identity.md)
- [Seguridad](./germany-migration-security.md)
- [Herramientas de administración](./germany-migration-management-tools.md)
- [Elementos multimedia](./germany-migration-media.md)
