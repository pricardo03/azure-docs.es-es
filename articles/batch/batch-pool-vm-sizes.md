---
title: 'Selección de tamaños de VM para grupos: Azure Batch | Microsoft Docs'
description: Cómo elegir uno de los tamaños de máquina virtual disponibles para los nodos de proceso en grupos de Azure Batch
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/12/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: be19de19dab92bc40ca5529ad578e033a98929cd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023572"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Selección de un tamaño de máquina virtual para nodos de proceso en un grupo de Azure Batch

Cuando seleccione un tamaño de nodo para un grupo de Azure Batch, puede elegir entre casi todas las familias y tamaños de máquinas virtuales disponibles en Azure. Azure ofrece una variedad de tamaños para máquinas virtuales Windows y Linux para diferentes cargas de trabajo.

Hay algunas excepciones y limitaciones a la hora de elegir un tamaño de máquina virtual:

* Algunas series o tamaños de máquina virtual no se admiten en Batch.
* Algunos tamaños de máquina virtual están restringidos y tienen que habilitarse específicamente antes de poder asignarlos.

## <a name="supported-vm-series-and-sizes"></a>Series y tamaños de maquina virtual compatibles

### <a name="pools-in-virtual-machine-configuration"></a>Grupos en la configuración de máquina virtual

Los grupos de Batch en la configuración de máquina virtual son compatibles con casi todos los tamaños de máquina virtual ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Consulte la tabla siguiente para más información sobre los tamaños admitidos y las restricciones.

No se garantiza la compatibilidad de los tamaños de máquina virtual promocionales o en versión preliminar que no figuren en la lista.

| Series de máquinas virtuales  | Tamaños admitidos | Modo de asignación de grupo de cuenta de Batch<sup>1</sup> |
|------------|---------|-----------------|
| Serie A básica | Todos los tamaños *excepto* Basic_A0 (A0) | Any |
| Serie A | Todos los tamaños *excepto* Standard_A0 | Any |
| Serie Av2 | Todos los tamaños | Any |
| Serie B | None | No disponible |
| Serie DC | None | No disponible |
| Series Dv2 y Dsv2 | Todos los tamaños | Any |
| Series Dv3 y Dsv3 | Todos los tamaños | Any |
| Series Ev3 y Esv3 | Todos los tamaños | Any |
| Serie Fsv2 | Todos los tamaños | Any |
| Serie H | Todos los tamaños | Any |
| Serie HB<sup>2</sup> | Todos los tamaños | Any |
| Serie HC<sup>2</sup> | Todos los tamaños | Any |
| Serie Ls | Todos los tamaños | Any |
| Serie Lsv2 | None | No disponible |
| Serie M | Standard_M64ms (solo baja prioridad), Standard_M128s (solo baja prioridad) | Any |
| Serie Mv2 | None | No disponible |
| Serie NC | Todos los tamaños | Any |
| Serie NCv2<sup>2</sup> | Todos los tamaños | Any |
| Serie NCv3<sup>2</sup> | Todos los tamaños | Any |
| Serie ND<sup>2</sup> | Todos los tamaños | Any |
| Serie NDv2 | Todos los tamaños | Modo de suscripción de usuario |
| Serie NV | Todos los tamaños | Any |
| Serie NVv3 | None | No disponible |
| SAP HANA | None | No disponible |

<sup>1</sup> Inicialmente, se admiten algunas series de máquinas virtuales más recientes de forma parcial. Puede asignar estas series de máquinas virtuales mediante cuentas de Batch con el **Modo de asignación de grupo** establecido en **Suscripción de usuario**. Consulte [Administración de cuentas de Batch](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) para más información sobre la configuración de la cuenta de Batch. Consulte [Límites y cuotas](batch-quota-limit.md) para información sobre cómo solicitar la cuota para estas series de máquinas virtuales parcialmente admitidas para cuentas de Batch con el modo **Suscripción de usuario**.  

<sup>2</sup> Estos tamaños de máquina virtual se pueden asignar en grupos de Batch en la configuración de la máquina virtual, pero tiene que solicitar un [aumento de cuota](batch-quota-limit.md#increase-a-quota) específico.

### <a name="pools-in-cloud-service-configuration"></a>Grupos en la configuración de Cloud Service

Los grupos de Batch en la configuración de Cloud Services son compatibles con todos los [tamaños de máquina virtual para Cloud Services](../cloud-services/cloud-services-sizes-specs.md)**excepto** en los siguientes casos:

| Series de máquinas virtuales  | Tamaños no compatibles |
|------------|-------------------|
| Serie A   | Extra pequeño       |
| Serie Av2 | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Consideraciones de tamaño

* **Requisitos de aplicación**: tenga en cuenta las características y los requisitos de la aplicación que se va a ejecutar en los nodos. Aspectos tales como si la aplicación es multiproceso y cuánta memoria consume pueden ayudar a determinar el tamaño de nodo más adecuado y rentable. Para varias instancias de [cargas de trabajo MPI](batch-mpi.md) o aplicaciones CUDA, considere la posibilidad de tamaños de máquina virtual especializados [HPC](../virtual-machines/linux/sizes-hpc.md) o [habilitado GPU](../virtual-machines/linux/sizes-gpu.md), respectivamente. (Consulte [Uso de instancias compatibles con RDMA o habilitadas para GPU en grupos de Batch](batch-pool-compute-intensive-sizes.md)).

* **Tareas por nodo**: normalmente, se selecciona un tamaño de nodo bajo el supuesto de que no se ejecutará más que una sola tarea a la vez en un nodo. No obstante, puede tener ventajas [ejecutar en paralelo](batch-parallel-node-tasks.md) varias tareas y, por tanto, varias instancias de la aplicación, en varios nodos de proceso durante la ejecución del trabajo. En este caso, es habitual elegir un tamaño de nodo de varios núcleos para acomodar el aumento de la demanda por la ejecución de tareas en paralelo.

* **Niveles de carga para diferentes tareas**: todos los nodos en un grupo tienen el mismo tamaño. Si va a ejecutar aplicaciones con requisitos del sistema o niveles de carga diferentes, es recomendable usar grupos separados.

* **Disponibilidad por regiones**: una serie o tamaño de máquina virtual, podría no estar disponible en las regiones en las que cree las cuentas de Batch. Para comprobar que un tamaño está disponible, vea [Productos disponibles por región](https://azure.microsoft.com/regions/services/).

* **Cuotas**: la [cuota de núcleos](batch-quota-limit.md#resource-quotas) en su cuenta de Batch puede limitar el número de nodos de un tamaño específico que se puede agregar a un grupo de Batch. Para solicitar un aumento de la cuota, consulte [este artículo](batch-quota-limit.md#increase-a-quota). 

* **Configuración de grupo**: por lo general, tiene más opciones de tamaño de máquina virtual cuando crea un grupo en la configuración de máquina virtual, en comparación con la configuración de Cloud Service.

## <a name="next-steps"></a>Pasos siguientes

* Para información general más detallada acerca de Batch, consulte [Desarrollo de soluciones de procesos paralelos a gran escala con Batch](batch-api-basics.md).
* Para más información acerca del uso de tamaños de máquinas virtuales de proceso intensivo, consulte [Uso de instancias compatibles con RDMA o habilitadas para GPU en grupos de Batch](batch-pool-compute-intensive-sizes.md).
