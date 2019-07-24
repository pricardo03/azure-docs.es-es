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
ms.openlocfilehash: 003a14174ff65bab253f27a458d4f3e2c0a1a6db
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069996"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Tamaños de máquina virtual de proceso de alto rendimiento

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Los tamaños de las máquinas virtuales habilitadas para SR-IOV en Azure permiten que se utilice casi cualquier tipo de MPI.
Las máquinas virtuales no habilitadas para SR-IOV admiten únicamente versiones Intel MPI 5.x. Las versiones posteriores (2017 y 2018) de la biblioteca en tiempo de ejecución de Intel MPI pueden ser o no compatibles con los controladores Azure Linux RDMA.


### <a name="supported-os-images"></a>Imágenes de sistema operativo compatibles
 
Azure Marketplace tiene muchas distribuciones de Linux que admiten la conectividad de RDMA:
  
* **HPC basada en CentOS**: en las máquinas virtuales no habilitadas para SR-IOV, se admite una HPC basada en CentOS versión 6.5 o posterior, hasta la versión 7.5. En las máquinas virtuales de la serie H, se recomienda usar las versiones 7.1 a 7.5. Los controladores RDMA e Intel MPI 5.1 están instalados en la máquina virtual.
  En las máquinas virtuales habilitadas para SR-IOV, HPC 7.6 basada en CentOS viene optimizada y precargada con los controladores de RDMA y con diversos paquetes de MPI instalados.
  En otras imágenes de máquina virtual de RHEL/CentOS, agregue la extensión InfiniBandLinux para habilitar InfiniBand. Esta extensión de máquina virtual de Linux instala controladores Mellanox OFED (en máquinas virtuales de SR-IOV) para la conectividad RDMA. El siguiente cmdlet de PowerShell instala la versión más reciente (versión 1.0) de la extensión InfiniBandDriverLinux en una máquina virtual compatible con RDMA existente. La máquina virtual compatible con RDMA se denomina *myVM* y se implementa en el grupo de recursos denominado *myResourceGroup* en la región *Oeste de EE. UU.* del siguiente modo:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  Como alternativa, se pueden incluir extensiones de máquina virtual en las plantillas de Azure Resource Manager para facilitar la implementación, con el siguiente elemento JSON:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
 
  > [!NOTE]
  > En las imágenes de HPC basadas en CentOS, las actualizaciones del núcleo están deshabilitadas en el archivo de configuración **yum** . Esto se debe a que los controladores Linux RDMA se distribuyen en forma de paquete RPM y sus actualizaciones de estos podrían no funcionar si se actualiza el kernel.
  >
  

* **SUSE Linux Enterprise Server**: SLES 12 SP3 para HPC, SLES 12 SP3 para HPC (Premium), SLES 12 SP1 para HPC, SLES 12 SP1 para HPC (Premium), SLES 12 SP4 y SLES 15. Los controladores RDMA se instalan y se distribuyen los paquetes Intel MPI en la máquina virtual. Instale MPI mediante la ejecución del comando siguiente:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu**: Ubuntu Server 16.04 LTS, 18.04 LTS. Configure controladores RDMA en la máquina virtual y regístrese en Intel para descargar Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  Para obtener más detalles sobre cómo habilitar InfiniBand y cómo configurar MPI, vea [Habilitar InfiniBand](../workloads/hpc/enable-infiniband.md).


### <a name="cluster-configuration-options"></a>Opciones de configuración del clúster

Azure ofrece varias opciones para crear clústeres de máquinas virtuales de HPC de Linux que se pueden comunicar con la red RDMA, incluidos: 

* **Máquinas virtuales**: implemente las máquinas virtuales de HPC compatibles con RDMA en el mismo conjunto de disponibilidad (cuando use el modelo de implementación de Azure Resource Manager). Si usa el modelo de implementación clásica, implemente las máquinas virtuales en el mismo servicio en la nube. 

* **Conjuntos de escalado de máquinas virtuales**: en un conjunto de escalado de máquinas virtuales, asegúrese de limitar la implementación a un único grupo de selección de ubicación. Por ejemplo, en una plantilla de Resource Manager, establezca la propiedad `singlePlacementGroup` en `true`. 

* **Azure CycleCloud**: cree un clúster de HPC en [Azure CycleCloud](/azure/cyclecloud/) para ejecutar trabajos MPI en nodos de Linux.

* **Azure Batch**: cree un grupo de [Azure Batch](/azure/batch/) para ejecutar cargas de trabajo MPI en nodos de proceso de Linux. Para obtener más información, consulte [Uso de instancias compatibles con RDMA o habilitadas para GPU en grupos de Batch](../../batch/batch-pool-compute-intensive-sizes.md). Consulte también el proyecto [Batch Shipyard](https://github.com/Azure/batch-shipyard) para ejecutar cargas de trabajo basadas en contenedores en Batch.

* **Microsoft HPC Pack**: [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) admite varias distribuciones de Linux en nodos de proceso implementados en máquinas virtuales de Azure compatibles con RDMA, administradas por un nodo principal de Windows Server. -  Para ver una implementación de ejemplo, consulte [Create HPC Pack Linux RDMA Cluster in Azure](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam) (Creación de un clúster RDMA de HPC Pack en Linux desde Azure).


### <a name="network-considerations"></a>Consideraciones sobre la red
* En las máquinas virtuales Linux compatibles con RDMA y no habilitadas para SR-IOV de Azure, eth1 se reserva para el tráfico de red RDMA. No cambie ninguna configuración eth1 ni ninguna información del archivo de configuración que haga referencia a esta red.
* En las máquinas virtuales habilitadas para SR-IOV (series HB y HC), ib0 está reservado para el tráfico de red de RDMA.
* La red RDMA en Azure reserva el espacio de direcciones 172.16.0.0/16. Para ejecutar aplicaciones MPI en instancias implementadas en una red virtual Azure, asegúrese de que el espacio de direcciones de la red virtual no se superpone a la red RDMA.
* Según la herramienta de administración de clúster que haya elegido, puede ser necesaria una configuración adicional del sistema para ejecutar trabajos MPI. Por ejemplo, en un clúster de máquinas virtuales, es posible que deba establecer confianza entre los nodo de clúster mediante la generación de claves SSH o mediante el establecimiento de inicios de sesión de SSH sin contraseña.


## <a name="other-sizes"></a>Otros tamaños
- [Uso general](sizes-general.md)
- [Proceso optimizado](sizes-compute.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre cómo configurar, optimizar y escalar [cargas de trabajo de HPC](../workloads/hpc/configure.md) en Azure.
- Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
