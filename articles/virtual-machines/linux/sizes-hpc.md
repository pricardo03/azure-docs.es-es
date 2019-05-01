---
title: 'Tamaños de las máquinas virtuales Linux en Azure: HPC | Microsoft Docs'
description: Enumera los tamaños diferentes disponibles para las máquinas virtuales de informática de alto rendimiento Linux en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: jonbeck
ms.openlocfilehash: 44b965bd60d976d4d28dc5e31d78a1c838d4ee02
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704674"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Tamaños de máquina virtual de proceso de alto rendimiento

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Se admiten solo las versiones Intel MPI 5.x.

> [!NOTE]
> Las versiones posteriores (2017 y 2018) del tiempo de ejecución de Intel MPI biblioteca puede o no sea compatible con los controladores Azure Linux RDMA.

### <a name="distributions"></a>Distribuciones
 
Implemente una máquina virtual de proceso intensivo desde una de las imágenes de Azure Marketplace que sea compatible con la conectividad RDMA:
  
* **Ubuntu**: Ubuntu Server 16.04 LTS. Configure controladores RDMA en la máquina virtual y regístrese en Intel para descargar Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server**: SLES 12 SP3 para HPC, SLES 12 SP3 para HPC (Premium), SLES 12 SP1 para HPC, SLES 12 SP1 para HPC (Premium). Los controladores RDMA se instalan y se distribuyen los paquetes Intel MPI en la máquina virtual. Instale MPI mediante la ejecución del comando siguiente:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **HPC basada en CentOS**: HPC basada en CentOS 6.5 o una versión posterior (para H-series, se recomienda la versión 7.1 o posterior). Los controladores RDMA e Intel MPI 5.1 están instalados en la máquina virtual.  
 
  > [!NOTE]
  > En las imágenes de HPC basadas en CentOS, las actualizaciones del núcleo están deshabilitadas en el archivo de configuración **yum** . Esto se debe a que los controladores Linux RDMA se distribuyen en forma de paquete RPM y sus actualizaciones de estos podrían no funcionar si se actualiza el kernel.
  > 
 
### <a name="cluster-configuration-options"></a>Opciones de configuración del clúster

Azure ofrece varias opciones para crear clústeres de máquinas virtuales de HPC de Linux que se pueden comunicar con la red RDMA, incluidos: 

* **Máquinas virtuales**: implemente las máquinas virtuales de HPC compatibles con RDMA en el mismo conjunto de disponibilidad (cuando use el modelo de implementación de Azure Resource Manager). Si usa el modelo de implementación clásica, implemente las máquinas virtuales en el mismo servicio en la nube. 

* **Conjuntos de escalado de máquinas virtuales**: en un conjunto de escalado de máquinas virtuales, asegúrese de limitar la implementación a un único grupo de selección de ubicación de red. Por ejemplo, en una plantilla de Resource Manager, establezca la propiedad `singlePlacementGroup` en `true`. 

* **Azure CycleCloud**: cree un clúster de HPC en [Azure CycleCloud](/azure/cyclecloud/) para ejecutar trabajos MPI en nodos de Linux.

* **Azure Batch**: cree un grupo de [Azure Batch](/azure/batch/) para ejecutar cargas de trabajo MPI en nodos de proceso de Linux. Para obtener más información, consulte [Uso de instancias compatibles con RDMA o habilitadas para GPU en grupos de Batch](../../batch/batch-pool-compute-intensive-sizes.md). Consulte también el proyecto [Batch Shipyard](https://github.com/Azure/batch-shipyard) para ejecutar cargas de trabajo basadas en contenedores en Batch.

* **Microsoft HPC Pack**: [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) admite varias distribuciones de Linux en nodos de proceso implementados en máquinas virtuales de Azure compatibles con RDMA, administradas por un nodo principal de Windows Server. -  Para ver una implementación de ejemplo, consulte [Create HPC Pack Linux RDMA Cluster in Azure](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam) (Creación de un clúster RDMA de HPC Pack en Linux desde Azure).

Según la herramienta de administración de clúster que haya elegido, puede ser necesaria una configuración adicional del sistema para ejecutar trabajos MPI. Por ejemplo, en un clúster de máquinas virtuales, es posible que deba establecer confianza entre los nodo de clúster mediante la generación de claves SSH o mediante el establecimiento de confianza SSH sin contraseña.

### <a name="network-topology-considerations"></a>Consideraciones sobre la topología de red
* En las máquinas virtuales con Linux compatibles con RDMA de Azure, Eth1 se reserva para el tráfico de red RDMA. No cambie ninguna configuración Eth1 ni ninguna información del archivo de configuración que haga referencia a esta red. Eth0 se reserva para el tráfico de red regular de Azure.

* La red RDMA en Azure reserva el espacio de direcciones 172.16.0.0/16. 




## <a name="other-sizes"></a>Otros tamaños
- [Uso general](sizes-general.md)
- [Proceso optimizado](sizes-compute.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.




