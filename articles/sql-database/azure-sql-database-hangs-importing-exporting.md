---
title: El servicio Import/Export de Azure SQL Database tarda mucho tiempo en importar o exportar una base de datos | Microsoft Docs
description: El servicio Import/Export de Azure SQL Database tarda mucho tiempo en importar o exportar una base de datos
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: ed5f3d1cd505270eb91c9cfbd6fb5c38b908f33d
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975371"
---
# <a name="azure-sql-database-importexport-service-takes-a-long-time-to-import-or-export-a-database"></a>El servicio Import/Export de Azure SQL Database tarda mucho tiempo en importar o exportar una base de datos

Al usar el servicio Import/Export de Azure SQL Database, es posible que observe que a veces el proceso tarda mucho tiempo en completarse. En este artículo se proporciona información adicional sobre las posibles causas de los retrasos y los métodos alternativos que puede usar para solucionar estos problemas.

## <a name="azure-sql-database-importexport-service"></a>Servicio Import/Export de Azure SQL Database

El servicio Import/Export de Azure SQL Database es un servicio web basado en REST que se ejecuta en cada centro de datos de Microsoft Azure. Este es el servicio al que se llama cuando se usan las opciones [Importar base de datos](https://docs.microsoft.com/azure/sql-database/sql-database-import#import-from-a-bacpac-file-in-the-azure-portal) o [Exportar](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-azure-portal) para trasladar la base de datos SQL en Microsoft Azure Portal. El servicio proporciona unos servicios gratuitos de puesta en cola de solicitudes y de proceso para realizar importaciones y exportaciones desde una base de datos de Microsoft Azure SQL a un almacenamiento de objetos binarios grandes (BLOB) de Microsoft Azure.

Las operaciones de importación y exportación no son una copia de seguridad de base de datos física tradicional, sino una copia de seguridad lógica de la base de datos que usa un formato BACPAC especial. Este formato de BACPAC lógico le permite evitar tener que usar un formato físico que puede variar entre las versiones de SQL Server y SQL Database. Por lo tanto, puede utilizarlo para restaurar de forma segura la base de datos en una base de datos SQL y en una base de datos SQL Server.

## <a name="what-causes-the-process-to-take-a-long-time"></a>Qué hace que el proceso tarde mucho tiempo

El servicio Import/Export de Azure SQL Database proporciona un número limitado de máquinas virtuales de proceso por región para procesar las operaciones de importación y exportación. La máquina virtual de proceso se hospeda por región para asegurarse de que la importación o exportación evita los cargos y los retrasos de ancho de banda entre regiones. Debido a esto, si se realizan demasiadas solicitudes al mismo tiempo en la misma región, se producen retrasos significativos en el procesamiento de las operaciones. El tiempo necesario para completar las solicitudes puede variar de unos segundos a varias horas.

> [!NOTE]
> Si una solicitud no se procesa en un plazo de cuatro días, el servicio la cancela automáticamente.

## <a name="recommended-solutions"></a>Soluciones recomendadas

Si las exportaciones de bases de datos se usan solo para la recuperación cuando hay una eliminación accidental de datos, todas las ediciones de base de datos de Azure SQL Server proporcionan una funcionalidad de restauración de autoservicio a partir de las copias de seguridad generadas por el sistema. Pero si necesita estas exportaciones por otras razones, y si requiere un rendimiento de importación o exportación más rápido o más predecible, debe tener en cuenta las siguientes opciones:

* [Exportar a un archivo BACPAC mediante la utilidad SQLPackage](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-sqlpackage-utility)
* [Exportar a un archivo BACPAC mediante SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-sql-server-management-studio-ssms)
* Ejecute la importación o exportación de BACPAC directamente en el código mediante la API de Microsoft® SQL Server® Data-Tier Application Framework (DacFx). Para información adicional revise los siguientes artículos:
  * [Exportar una aplicación de la capa de datos](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
  * [Microsoft.SqlServer.Dac Namespace](https://docs.microsoft.com/dotnet/api/microsoft.sqlserver.dac)
  * [Descarga de DacFx](https://www.microsoft.com/download/details.aspx?id=55713)

## <a name="considerations-when-exporting-or-importing-an-azure-sql-database"></a>Consideraciones al exportar o importar una base de datos de Azure SQL

* Todos los métodos descritos en este artículo usan la cuota de DTU, lo que da como resultado la limitación del servicio Azure SQL Database. Puede [consultar las estadísticas de DTU de la base de datos en Azure Portal](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview#monitor-database-performance). Si la base de datos alcanza los límites de recursos, [actualice el nivel de servicio](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources) para agregar más recursos.
* Lo ideal es que las aplicaciones cliente (por ejemplo, la utilidad sqlpackage o la aplicación DAC personalizada) se ejecuten desde una máquina virtual en la misma región que la base de datos SQL, de lo contrario podría tener problemas de rendimiento debido a la latencia de la red.
* La exportación de tablas grandes sin índices agrupados puede ser un proceso muy lento o incluso producir un error. Esto se debe a que la tabla no se puede dividir y exportar en paralelo y tiene que exportarse en una sola transacción, lo que provoca lentitud y posibles errores durante la exportación, especialmente en el caso de las tablas grandes. 


## <a name="related-documents"></a>Documentos relacionados

[Consideraciones al exportar una base de datos de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-export#considerations-when-exporting-an-azure-sql-database)
