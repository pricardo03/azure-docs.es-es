---
title: Tamaños de las máquinas virtuales Windows en Azure | Microsoft Docs
description: Enumera los tamaños diferentes disponibles para las máquinas virtuales de informática de alto rendimiento Windows en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: jonbeck;amverma
ms.openlocfilehash: ad490084b34a8bf6e89c7feb14d5cd2e70a8138f
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755323"
---
# <a name="high-performance-compute-vm-sizes"></a>Tamaños de máquina virtual de informática de alto rendimiento

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Sistema operativo**: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

* **MPI** : Microsoft MPI (MS-MPI) 2012 R2 o versiones posteriores, MPI Intel Library 5.x.

  En máquinas virtuales habilitadas-SR-IOV, implementaciones de MPI compatibles usan la interfaz Microsoft Network Direct (ND) para comunicarse entre instancias. Lo tamaños de máquina virtual habilitados (HB y HC-series) en Azure permiten casi cualquier versión de MPI para usarse con Mellanox OFED SR-IOV. 

* **Extensión de VM InfiniBandDriverWindows** : en las máquinas virtuales compatibles con RDMA, agregue la extensión InfiniBandDriverWindows para habilitar InfiniBand. Esta extensión de máquina virtual de Windows instala Windows Network Direct controladores (en máquinas virtuales que no sean SR-IOV) o los controladores Mellanox OFED (en máquinas virtuales de SR-IOV) para la conectividad RDMA.
En algunas implementaciones de instancias A8 y A9, la extensión HpcVmDrivers se agrega automáticamente. Tenga en cuenta que la extensión HpcVmDrivers VM está en desuso; no se actualizará. Para agregar la extensión de máquina virtual a una máquina virtual, puede usar los cmdlets de [Azure PowerShell](/powershell/azure/overview). 

  El comando siguiente instala la extensión de InfiniBandDriverWindows versión 1.0 más reciente en una máquina virtual compatibles con RDMA existente denominada *myVM* implementados en el grupo de recursos denominado *myResourceGroup* en el  *Oeste de Estados Unidos* región:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  ```
  Como alternativa, se pueden incluir extensiones de máquina virtual en las plantillas de Azure Resource Manager para facilitar la implementación, con el siguiente elemento JSON:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverWindows",
  "typeHandlerVersion": "1.0",
  } 
  ```
  
  Para obtener más información, consulte el artículo de [características y extensiones de las máquinas virtuales](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). También puede trabajar con las extensiones para las máquinas virtuales implementadas en el [modelo de implementación clásica](classic/manage-extensions.md).

* **Espacio de direcciones de red RDMA** : la red RDMA en Azure reserva el espacio de direcciones 172.16.0.0/16. Para ejecutar aplicaciones MPI en instancias implementadas en una red virtual Azure, asegúrese de que el espacio de direcciones de la red virtual no se superpone a la red RDMA.


### <a name="cluster-configuration-options"></a>Opciones de configuración del clúster

Azure ofrece varias opciones para crear clústeres de máquinas virtuales de HPC de Windows que se pueden comunicar con la red RDMA, incluidos: 

* **Máquinas virtuales**: implemente las máquinas virtuales de HPC compatibles con RDMA en el mismo conjunto de disponibilidad (cuando use el modelo de implementación de Azure Resource Manager). Si usa el modelo de implementación clásica, implemente las máquinas virtuales en el mismo servicio en la nube. 

* **Conjuntos de escalado de máquinas virtuales** - escalado de máquinas virtuales en el conjunto, asegúrese de limitar la implementación en un único grupo. Por ejemplo, en una plantilla de Resource Manager, establezca la propiedad `singlePlacementGroup` en `true`. 

* **Azure CycleCloud**: cree un clúster de HPC en [Azure CycleCloud](/azure/cyclecloud/) para ejecutar trabajos MPI en nodos de Windows.

* **Azure Batch**: cree un grupo de [Azure Batch](/azure/batch/) para ejecutar cargas de trabajo MPI en nodos de proceso de Windows Server. Para más información, consulte [Uso de instancias compatibles con RDMA o habilitadas para GPU en grupos de Batch](../../batch/batch-pool-compute-intensive-sizes.md). Consulte también el proyecto [Batch Shipyard](https://github.com/Azure/batch-shipyard) para ejecutar cargas de trabajo basadas en contenedores en Batch.

* **Microsoft HPC Pack**: [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) incluye un entorno de tiempo de ejecución para MS-MPI que usa la red RDMA de Azure cuando se implementa en máquinas virtuales de Windows compatibles con RDMA. -  Para obtener ejemplos de implementación, consulte la [Configuración de un clúster de Windows RDMA con HPC Pack para ejecutar aplicaciones MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Otros tamaños
- [Uso general](sizes-general.md)
- [Proceso optimizado](sizes-compute.md)
- [Memoria optimizada](../virtual-machines-windows-sizes-memory.md)
- [Almacenamiento optimizado](../virtual-machines-windows-sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

- Para ver listas de comprobación para usar instancias de proceso intensivo con HPC Pack en Windows Server, consulte [Configuración de un clúster de Windows RDMA con HPC Pack para ejecutar aplicaciones MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Para usar instancias de proceso intensivo para ejecutar aplicaciones MPI con Azure Batch, consulte [Uso de tareas de instancias múltiples para ejecutar aplicaciones de la Interfaz de paso de mensajes (MPI) en Azure Batch](../../batch/batch-mpi.md).

- Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
