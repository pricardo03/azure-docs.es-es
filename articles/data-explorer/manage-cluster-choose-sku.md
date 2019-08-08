---
title: Selección de la SKU de máquina virtual correcta para el clúster de Azure Data Explorer
description: En este artículo se describe cómo debe seleccionar el tamaño de SKU óptimo para el clúster de Azure Data Explorer.
author: avnera
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: b0cf6eab86b0b932e44b6824305c23df01f35808
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383823"
---
# <a name="select-the-correct-vm-sku-for-your-azure-data-explorer-cluster"></a>Selección de la SKU de máquina virtual correcta para el clúster de Azure Data Explorer 

Azure Data Explorer tiene varias SKU de máquina virtual entre las que elegir al crear un nuevo clúster u optimizar el clúster para una carga de trabajo cambiante. Las máquinas virtuales se eligieron detenidamente para permitir el costo más óptimo posible para cualquier carga de trabajo. 

El servicio Azure Data Explorer administra completamente el tamaño y la SKU de la máquina virtual del clúster de administración de datos. Estos vienen determinados por factores como el tamaño de la máquina virtual del motor y la carga de trabajo de ingesta. 

La SKU de la máquina virtual del clúster del motor se puede cambiar en cualquier momento. Para ello, es necesario [escalar verticalmente el clúster](manage-cluster-vertical-scaling.md). Por lo tanto, es mejor empezar con el tamaño mínimo de la SKU que se adapte al escenario inicial. Tenga en cuenta que el escalado vertical del clúster provoca un tiempo de inactividad de hasta 30 minutos mientras se vuelve a crear el clúster con la nueva SKU de máquina virtual.

> [!TIP]
> Las [RI (instancias reservadas)](https://docs.microsoft.com/azure/virtual-machines/windows/prepay-reserved-vm-instances) de proceso se pueden aplicar al clúster de Azure Data Explorer.  

En este artículo se describen las diferentes opciones de SKU de máquina virtual y se proporcionan los detalles técnicos que pueden ayudarle a tomar la decisión óptima.

## <a name="select-the-cluster-type"></a>Selección del tipo de clúster

Azure Data Explorer ofrece dos tipos de clústeres:

* **Producción**: los clústeres de producción contienen dos nodos para los clústeres de administración de datos y del motor y operan en virtud del [Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/data-explorer/v1_0/) de Azure Data Explorer.

* **Desarrollo/pruebas (sin Acuerdo de Nivel de Servicio)** : los clústeres de desarrollo/pruebas tienen un único nodo D11_v2 para el clúster de motor y un solo nodo D1 para el clúster de administración de datos. Este tipo de clúster es la configuración de menor costo debido al bajo recuento de instancias y a que no se aplica ningún recargo por marcado de motor. Para esta configuración de clúster, no hay ningún Acuerdo de Nivel de Servicio, ya que carece de redundancia.

## <a name="sku-types"></a>Tipos de SKU

Cuando cree un clúster de Azure Data Explorer, seleccione la SKU de máquina virtual *óptima* para la carga de trabajo planeada. Azure Data Explorer tiene dos familias de SKU entre las que puede elegir:

* **D_V2**: la SKU D, que se ha optimizado para el proceso y se proporciona en dos tipos.
    * La propia máquina virtual
    * La máquina virtual agrupada con discos de Premium Storage

* **LS**: la SKU L, que está optimizada para almacenamiento. Tiene un tamaño de SSD mucho mayor que la SKU **D** que tiene un precio similar.

En la tabla siguiente, se proporcionan las diferencias principales entre los tipos de SKU disponibles:
 
|**Atributo** | **SKU D** | **SKU L**
|---|---|---
|**SKU pequeñas**|El tamaño mínimo es "D11" con dos núcleos|El tamaño mínimo es "L4" con cuatro núcleos
|**Disponibilidad**|Disponible en todas las regiones (la versión DS + PS tiene una disponibilidad más limitada)|Disponible en algunas regiones
|**Costo por caché de GB por núcleo**|Alto con la SKU D, bajo con la versión DS + PS|Más económico con la opción de *pago por uso*
|**Precio de RI (instancias reservadas)**|Descuento alto (más del 55 % para un compromiso de tres años)|Descuento menor (20 % para un compromiso de tres años)  

## <a name="select-your-cluster-vm"></a>Selección de la máquina virtual del clúster 

Para seleccionar la máquina virtual del clúster, [configure el escalado vertical](manage-cluster-vertical-scaling.md#configure-vertical-scaling). 

Las distintas opciones de SKU de máquina virtual le permiten optimizar los costos de los requisitos de rendimiento y de caché de acceso frecuente necesarios para el escenario deseado. Si el escenario requiere un rendimiento óptimo para un volumen de consulta elevado, la SKU ideal debe optimizarse para el proceso . Por otro lado, si el escenario requiere consultas de grandes volúmenes de datos con una carga de consultas relativamente baja, la SKU optimizada para el almacenamiento reducirá los costos al tiempo que proporcionará un rendimiento excelente.

El número de instancias por clúster para las SKU pequeñas es limitado, por lo que es preferible usar máquinas virtuales más grandes que tengan más RAM. El tamaño de la RAM es necesario para algunos tipos de consulta que ponen más demanda en el recurso de RAM, como las consultas que utilizan `joins`. Por lo tanto, si tenemos en cuenta las opciones de escalado, es recomendable escalar verticalmente a una SKU más grande en lugar de escalar horizontalmente mediante la adición de más instancias.

## <a name="vm-options"></a>Opciones de máquina virtual

En la tabla siguiente se proporcionan las especificaciones técnicas para las máquinas virtuales del clúster de Azure Data Explorer:

|**Nombre**| **Categoría** | **Tamaño de SSD** | **Núcleos** | **RAM** | **Discos de Premium Storage (1 GB)**| **Número mínimo de instancias por clúster** | **Número máximo de instancias por clúster**
|---|---|---|---|---|---|---|---
|D11_v2| Optimizada para proceso | 75 GB    | 2 | 14 GB | 0 | 1 | 8 (excepto para la SKU de desarrollo/pruebas en que es 1)
|D12_v2| Optimizada para proceso | 150 GB   | 4 | 28 GB | 0 | 2 | 16
|D13_v2| Optimizada para proceso | 307 GB   | 8 | 56 GB | 0 | 2 | 1000
|D14_v2| Optimizada para proceso | 614 GB   | 16| 112 GB | 0 | 2 | 1000
|DS13_v2+1TB PS| Optimizada para almacenamiento | 1 TB | 8 | 56 GB | 1 | 2 | 1000
|DS13_v2+2TB PS| Optimizada para almacenamiento | 2 TB | 8 | 56 GB | 2 | 2 | 1000
|DS14_v2+3TB PS| Optimizada para almacenamiento | 3 TB | 16 | 112 GB | 2 | 2 | 1000
|DS14_v2+4TB PS| Optimizada para almacenamiento | 4 TB | 16 | 112 GB | 4 | 2 | 1000
|L4s_v1| Optimizada para almacenamiento | 650 GB | 4 | 32 GB | 0 | 2 | 16
|L8s_v1| Optimizada para almacenamiento | 1,3 TB | 8 | 64 GB | 0 | 2 | 1000
|L16s_1| Optimizada para almacenamiento | 2,6 TB | 16| 128 GB | 0 | 2 | 1000

* Consulte la lista de SKU de máquina virtual actualizada por región mediante la [API ListSkus](/dotnet/api/microsoft.azure.management.kusto.clustersoperationsextensions.listskus?view=azure-dotnet) de Azure Data Explorer. 
* Obtenga más información acerca de las [diferentes SKU de proceso](/azure/virtual-machines/windows/sizes-compute). 

## <a name="next-steps"></a>Pasos siguientes

* El clúster del motor se [puede escalar o reducir](manage-cluster-vertical-scaling.md) verticalmente en cualquier momento. Para ello, hay que cambiar la SKU de la máquina virtual según las distintas necesidades. 

* El tamaño del clúster del motor se [puede escalar y reducir](manage-cluster-horizontal-scaling.md) horizontalmente para alterar la capacidad con demandas cambiantes.

