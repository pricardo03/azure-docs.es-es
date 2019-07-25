---
title: Tamaños de las máquinas virtuales Windows en Azure | Microsoft Docs
description: Enumera los tamaños diferentes disponibles para las máquinas virtuales de informática de alto rendimiento Windows en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: amverma
ms.reviewer: jonbeck
ms.openlocfilehash: 6fd08ca912c14a50064f4b6da18334d8bf9df0ca
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67871584"
---
# <a name="high-performance-compute-vm-sizes"></a>Tamaños de máquina virtual de informática de alto rendimiento

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Sistema operativo**: Windows Server 2016 en todas las máquinas virtuales de la serie HPC anterior. Windows Server 2012 R2 y Windows Server 2012 también se admiten en las máquinas virtuales no habilitadas para SR-IOV (de ahí que se excluyan HB y HC).

* **MPI**: los tamaños de máquina virtual habilitados para SR-IOV en Azure (HB, HC) permiten que se use prácticamente cualquier tipo de MPI con Mellanox OFED.
En las máquinas virtuales en que no está habilitado SR-IOV, las implementaciones de MPI compatibles usan la interfaz Microsoft Network Direct (ND) para comunicarse entre las instancias. Por lo tanto, solo se admiten las versiones de Microsoft MPI (MS-MPI) 2012 R2 o posterior e Intel MPI 5.x. Las versiones posteriores (2017 y 2018) de la biblioteca en tiempo de ejecución de MPI pueden ser o no compatibles con los controladores de Azure RDMA.

* **Extensión de VM InfiniBandDriverWindows**: en las máquinas virtuales compatibles con RDMA, agregue la extensión InfiniBandDriverWindows para habilitar InfiniBand. Esta extensión de máquina virtual instala controladores Windows Network Direct (en máquinas virtuales sin SR-IOV) o controladores Mellanox OFED (en máquinas virtuales con SR-IOV) para la conectividad RDMA.
En algunas implementaciones de las instancias A8 y A9, la extensión HpcVmDrivers se agrega automáticamente. Tenga en cuenta que la extensión de máquina virtual HpcVmDrivers está en desuso, por lo que no se actualizará. Para agregar la extensión de máquina virtual a una máquina virtual, puede usar los cmdlets de [Azure PowerShell](/powershell/azure/overview). 

  El comando siguiente instala la versión más reciente de la extensión InfiniBandDriverWindows, la versión 1.0, en una máquina virtual compatible con RDMA existente denominada *myVM* implementada en el grupo de recursos denominado *myResourceGroup* de la región *Oeste de EE. UU.* :

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

  El comando siguiente instala la versión más reciente de la extensión InfiniBandDriverWindows, la versión 1.0, en todas las máquinas virtuales compatibles con RDMA en un conjunto de escalado de máquina virtual denominado *myVMSS* implementado en el grupo de recursos denominado *myResourceGroup*:

  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```

  Para obtener más información, consulte el artículo de [características y extensiones de las máquinas virtuales](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). También puede trabajar con las extensiones para las máquinas virtuales implementadas en el [modelo de implementación clásica](classic/manage-extensions.md).

* **Espacio de direcciones de red RDMA** : la red RDMA en Azure reserva el espacio de direcciones 172.16.0.0/16. Para ejecutar aplicaciones MPI en instancias implementadas en una red virtual Azure, asegúrese de que el espacio de direcciones de la red virtual no se superpone a la red RDMA.


### <a name="cluster-configuration-options"></a>Opciones de configuración del clúster

Azure ofrece varias opciones para crear clústeres de máquinas virtuales de HPC de Windows que se pueden comunicar con la red RDMA, incluidos: 

* **Máquinas virtuales**: implemente las máquinas virtuales de HPC compatibles con RDMA en el mismo conjunto de disponibilidad (cuando use el modelo de implementación de Azure Resource Manager). Si usa el modelo de implementación clásica, implemente las máquinas virtuales en el mismo servicio en la nube. 

* **Conjuntos de escalado de máquinas virtuales**: en un conjunto de escalado de máquinas virtuales, asegúrese de limitar la implementación a un único grupo de selección de ubicación. Por ejemplo, en una plantilla de Resource Manager, establezca la propiedad `singlePlacementGroup` en `true`. 

* **MPI entre las máquinas virtuales**: si es necesaria la comunicación de MPI entre las máquinas virtuales, asegúrese de que las máquinas virtuales estén en el mismo conjunto de disponibilidad o en el mismo conjunto de escalado de la máquina virtual.

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
