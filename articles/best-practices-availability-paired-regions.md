---
title: 'Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure | Microsoft Docs'
description: Conozca más sobre el emparejamiento de regiones de Azure para asegurarse de que las aplicaciones sean resistentes durante los errores del centro de datos.
author: rayne-wiselman
manager: carmon
ms.service: multiple
ms.topic: article
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 81ba993e6cbe55b45d34325545754bec561ce479
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514464"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure

## <a name="what-are-paired-regions"></a>¿Qué son las regiones emparejadas?

Azure funciona en varias ubicaciones geográficas del mundo. Una ubicación geográfica de Azure es un área definida del mundo que contiene al menos una región de Azure. Una región de Azure es un área dentro de una ubicación geográfica que contiene uno o varios centros de datos.

Cada región de Azure se empareja con otra región de la misma zona geográfica, que juntas forman un emparejamiento regional. La excepción es el Sur de Brasil, ya que se trata de una región emparejada con otra que se encuentra fuera de su ubicación geográfica. Por los pares de regiones, Azure serializa las actualizaciones de la plataforma (mantenimiento planeado) de forma que solo se actualiza una región emparejada a la vez. Si se produce una interrupción que afecte a varias regiones, se dará prioridad a la recuperación de al menos una región de cada par.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Figura 1: Parejas regionales de Azure

| Geography | Regiones emparejadas |  |
|:--- |:--- |:--- |
| Asia |Asia oriental |Sudeste asiático |
| Australia |Este de Australia |Sudeste de Australia |
| Australia |Centro de Australia |Centro de Australia 2 |
| Brasil |Sur de Brasil |Centro-Sur de EE. UU |
| Canadá |Centro de Canadá |Este de Canadá |
| China |Norte de China |Este de China|
| China |Norte de China 2 |Este de China 2|
| Europa |Norte de Europa (Irlanda) |Oeste de Europa (Países Bajos) |
| Francia |Centro de Francia|Sur de Francia|
| Alemania |Centro de Alemania |Noreste de Alemania |
| India |India Central |Sur de la India |
| India |Oeste de la India |Sur de la India |
| Japón |Este de Japón |Oeste de Japón |
| Corea |Corea Central |Corea del Sur |
| Norteamérica |Este de EE. UU |Oeste de EE. UU. |
| Norteamérica |Este de EE. UU. 2 |Centro de EE. UU. |
| Norteamérica |Centro-Norte de EE. UU |Centro-Sur de EE. UU |
| Norteamérica |Oeste de EE. UU. 2 |Centro occidental de EE.UU. 
| Sudáfrica | Norte de Sudáfrica | Oeste de Sudáfrica
| Reino Unido |Oeste de Reino Unido |Sur de Reino Unido 2 |
| Emiratos Árabes Unidos | Norte de Emiratos Árabes Unidos | Centro de Emiratos Árabes Unidos
| Departamento de Defensa de Estados Unidos |Departamento de Defensa de EE. UU. Este |Departamento de Defensa de EE. UU. Centro |
| Gobierno de Estados Unidos |Gobierno de EE. UU.: Arizona |Gobierno de EE. UU.: Texas |
| Gobierno de Estados Unidos |US Gov Iowa |Gobierno de EE. UU. - Virginia |
| Gobierno de Estados Unidos |Gobierno de EE. UU. - Virginia |Gobierno de EE. UU.: Texas |

Tabla 1: Asignación de las parejas regionales de Azure

- Oeste de la India se empareja en una sola dirección. La región secundaria del Oeste de la India es Sur de la India, pero la región secundaria esta última es India Central.
- Sur de Brasil es un caso único porque se empareja con una región fuera de su propia ubicación geográfica. La región secundaria de Sur de Brasil es Centro-sur de EE. UU. La región secundaria de Centro-sur de EE. UU. no es Sur de Brasil.
- La región secundaria de US Gov Iowa es US Gov Virginia.
- La región secundaria de US Gov Virginia es US Gov Texas.
- La región secundaria de US Gov Texas es US Gov Arizona.


Se recomienda que configure la continuidad empresarial y recuperación ante desastres entre las parejas regionales para beneficiarse de las directivas de aislamiento y disponibilidad de Azure. Para aplicaciones que admiten varias regiones activas, recomendamos utilizar, siempre que sea posible, ambas regiones en un par de regiones. Esto asegurará una disponibilidad óptima para las aplicaciones y minimizará el tiempo de recuperación en caso de desastre. 

## <a name="an-example-of-paired-regions"></a>Un ejemplo de regiones emparejadas
La figura 2 muestra una aplicación hipotética que utiliza el par regional para la  recuperación ante desastres. Los números verdes resaltan las actividades entre regiones de tres servicios de Azure (Compute, Storage y Database) y cómo están configurados para replicar entre regiones. Con los números de color naranja se resaltan los beneficios exclusivos de la implementación en regiones emparejadas.

![Información general sobre las ventajas de las regiones emparejadas](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Ilustración 2: Pareja regional de Azure hipotética

## <a name="cross-region-activities"></a>Actividades entre regiones
Como se indica en la ilustración 2.

![IaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure Compute (Iaas)** : debe aprovisionar recursos de procesos adicionales de antemano para asegurarse de que haya recursos disponibles en otra región durante un desastre. Para obtener más información, consulte [Guía técnica sobre resistencia en Azure](resiliency/resiliency-technical-guidance.md).

![Storage](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage**: si utiliza discos administrados, obtenga información sobre las [copias de seguridad entre regiones](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region#virtual-machines) con Azure Backup y la [replicación de máquinas virtuales](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication) de una región a otra con Azure Site Recovery. Si está usando cuentas de almacenamiento, el almacenamiento con redundancia geográfica (GRS) se configura de manera predeterminada cuando se crea una cuenta de Azure Storage. Con GRS, los datos se replican automáticamente tres veces dentro de la región primaria y tres veces en la región emparejada. Para obtener más información, consulte [Opciones de redundancia de Azure Storage](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL Database**: con la replicación geográfica de SQL Database, puede configurar la replicación asincrónica de las transacciones en cualquier región del mundo; sin embargo, se recomienda implementar estos recursos en una región emparejada para la mayoría de los escenarios de recuperación ante desastres. Para más información, consulte [Replicación geográfica en Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png)**Azure Resource Manager**: Resource Manager proporciona de forma inherente aislamiento lógico de los componentes entre regiones. Esto significa que es menos probable que los errores lógicos de una región afecten a otra.

## <a name="benefits-of-paired-regions"></a>Ventajas de las regiones emparejadas
Como se indica en la ilustración 2.  

![Aislamiento](./media/best-practices-availability-paired-regions/5Orange.png)
**Aislamiento físico**: cuando sea posible, Azure prefiere por lo menos 300 millas de separación entre los centros de datos de un emparejamiento regional, aunque esto no será práctico o posible en todo el mundo. La separación del centro de datos físico reduce la probabilidad de que los desastres naturales, los disturbios civiles, los cortes del suministro eléctrico o las interrupciones de la red física afecten simultáneamente a ambas regiones. El aislamiento está sujeto a las restricciones geográficas (tamaño de la ubicación geográfica, disponibilidad de la infraestructura de red/energía, normativas, etc.).  

![Replicación](./media/best-practices-availability-paired-regions/6Orange.png)
**Replicación proporcionada por la plataforma**: algunos servicios, como el almacenamiento con redundancia geográfica ofrecen replicación automática a la región emparejada.

![Recuperación](./media/best-practices-availability-paired-regions/7Orange.png)
**Orden de recuperación de región**: en el caso de una interrupción amplia, tiene prioridad la recuperación de una región por cada pareja. Se garantiza que, si las aplicaciones se implementan en regiones emparejadas, se dará prioridad a la recuperación de una de las regiones. Si una aplicación se implementa en regiones que no están emparejadas, podría retrasarse la recuperación; en el peor de los casos, es posible que las regiones elegidas sean las dos últimas en recuperarse.

![Actualizaciones](./media/best-practices-availability-paired-regions/8Orange.png)
**Actualizaciones secuenciales**: las actualizaciones planeadas del sistema de Azure se implementan en regiones emparejadas de forma secuencial (no al mismo tiempo) para minimizar el tiempo de inactividad, el efecto de los errores y la aparición de errores lógicos en el infrecuente caso de una actualización incorrecta.

![Datos](./media/best-practices-availability-paired-regions/9Orange.png)
**Residencia de datos**: una región reside dentro de la misma ubicación geográfica que su pareja (a excepción del Sur de Brasil) con el objeto de cumplir los requisitos de residencia de datos para fines de jurisdicción de impuestos y aplicación de la ley.
