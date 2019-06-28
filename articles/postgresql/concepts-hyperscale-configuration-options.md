---
title: Opciones de rendimiento de Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar)
description: Opciones para un grupo de servidores Hiperescala (Citus), incluidas las regiones, el almacenamiento y el proceso de nodos.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e8c1c2e51ca14ae9b17f0d7efb20552cdd55139b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077296"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-preview-performance-options"></a>Opciones de rendimiento de Azure Database for PostgreSQL Hiperescala (Citus) (versión preliminar)

## <a name="compute-and-storage"></a>Compute y Storage
 
Puede seleccionar la configuración de proceso y almacenamiento por separado para los nodos de trabajo y el nodo de coordinación en un grupo de servidores Hiperescala (Citus).  Los recursos de proceso se proporcionan como núcleos virtuales, que representan la CPU lógica del hardware subyacente. El tamaño de almacenamiento para el aprovisionamiento se refiere a la capacidad disponible para los nodos de trabajo y de coordinación en el grupo de servidores Hiperescala (Citus). El almacenamiento incluye archivos de base de datos, archivos temporales, registros de transacciones y registros de servidor PostgreSQL. La cantidad total de almacenamiento que se aprovisiona también define la capacidad de E/S disponible para cada nodo de coordinación y de trabajo.
 
|                       | Nodo de trabajo           | Nodo de coordinación      |
|-----------------------|-----------------------|-----------------------|
| Proceso, núcleos virtuales       | 4, 8, 16, 32          | 4, 8, 16, 32          |
| Memoria por núcleo virtual, GiB | 8                     | 4                     |
| Tamaño de almacenamiento, TiB     | 0,5, 1, 2             | 0,5, 1, 2             |
| Tipo de almacenamiento          | Uso general (SSD) | Uso general (SSD) |
| E/S                  | Hasta 3 IOPS/GiB      | Hasta 3 IOPS/GiB      |


## <a name="regions"></a>Regiones
Los grupos de servidores Hiperescala (Citus) están disponibles en las siguientes regiones de Azure:
* Este de EE. UU. 2
* Sudeste de Asia
* Europa occidental
* Oeste de EE. UU. 2

## <a name="pricing"></a>Precios
Para conocer la información más actualizada sobre precios, consulte la [página de precios](https://azure.microsoft.com/pricing/details/postgresql/) del servicio.
Para ver el costo de la configuración deseada, en [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) se muestra el costo mensual en la pestaña **Configurar** según las opciones que seleccione. Si no tiene una suscripción de Azure, puede usar la calculadora de precios de Azure para obtener un precio estimado. En el sitio web [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/), seleccione **Agregar elementos**, expanda la categoría **Bases de datos** y elija **Azure Database for PostgreSQL: Hiperescala (Citus)** para personalizar las opciones.
 
## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [crear un grupo de servidores Hiperescala (Citus) en el portal](quickstart-create-hyperscale-portal.md).
