---
title: 'Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar) las opciones de rendimiento'
description: Opciones para un grupo de servidores (Citus) a gran escala, incluidas las regiones, almacenamiento y proceso de nodo.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e8c1c2e51ca14ae9b17f0d7efb20552cdd55139b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077296"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-preview-performance-options"></a>Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar) las opciones de rendimiento

## <a name="compute-and-storage"></a>Compute y Storage
 
Puede seleccionar la configuración de proceso y almacenamiento por separado para los nodos de trabajo y el nodo coordinador en un grupo de servidores (Citus) a gran escala.  Los recursos de proceso se proporcionan como núcleos virtuales, que representan la CPU lógica del hardware subyacente. El tamaño de almacenamiento para el aprovisionamiento se refiere a la capacidad disponible para los nodos de coordinador y de trabajo en el grupo de servidores (Citus) a gran escala. El almacenamiento incluye archivos de base de datos, los archivos temporales, los registros de transacciones y los registros del servidor Postgres. La cantidad total de almacenamiento que se aprovisiona también define la capacidad de E/S disponible para cada nodo coordinador y de trabajo.
 
|                       | Nodo de trabajo           | Nodo del coordinador      |
|-----------------------|-----------------------|-----------------------|
| Procesos, núcleos virtuales       | 4, 8, 16, 32          | 4, 8, 16, 32          |
| Memoria por núcleo virtual, GiB | 8                     | 4                     |
| Tamaño de almacenamiento, TiB     | 0.5, 1, 2             | 0.5, 1, 2             |
| Tipo de almacenamiento          | Uso general (SSD) | Uso general (SSD) |
| E/S                  | Hasta 3 IOPS/GiB      | Hasta 3 IOPS/GiB      |


## <a name="regions"></a>Regiones
Grupos de servidores de gran escala (Citus) están disponibles en las siguientes regiones de Azure:
* Este de EE. UU. 2
* Sudeste de Asia
* Europa occidental
* Oeste de EE. UU. 2

## <a name="pricing"></a>Precios
Para conocer la información más actualizada sobre precios, consulte la [página de precios](https://azure.microsoft.com/pricing/details/postgresql/) del servicio.
Para ver el costo de la configuración que desee, el [portal Azure](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) se muestra el costo mensual en la **configurar** ficha según las opciones que seleccione. Si no tiene una suscripción de Azure, puede usar la calculadora de precios de Azure para obtener un precio estimado. En el [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) sitio Web, seleccione **agregar elementos**, expanda el **bases de datos** categoría y elija **-Azure Database for PostgreSQL: A gran escala (Citus)** para personalizar las opciones.
 
## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [crear un grupo de servidores (Citus) a gran escala en el portal de](quickstart-create-hyperscale-portal.md).
