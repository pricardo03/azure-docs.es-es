---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/17/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: de998265188b1482be22225bba3c6ab3d58e562b
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66755570"
---
Los tamaños de VM optimizadas para almacenamiento proporcionan un alto rendimiento de disco y de E/S y son ideales para macrodatos, bases de datos SQL y NoSQL, almacenamiento de datos y bases de datos transaccionales grandes.  Por ejemplo, Cassandra, MongoDB, Cloudera y Redis. En este artículo, se proporciona información acerca del número de vCPU, discos de datos y tarjetas de interfaz de red, así como del rendimiento del almacenamiento local y del ancho de banda de red para cada tamaño optimizado.

La serie Lsv2 proporciona un alto rendimiento, baja latencia, almacenamiento NVMe local asignado directamente que se ejecuta en el procesador [AMD EPYC &trade; 7551](https://www.amd.com/en/products/epyc-7000-series) con una potencia en todos los núcleos de 2,55 GHz y una potencia máxima de 3,0 GHz. Las máquinas virtuales de la serie Lsv2 están disponibles en tamaños de 8 a 80 vCPU en una configuración de varios subprocesos simultáneos.  Hay 8 GiB de memoria por vCPU y un dispositivo SSD NVMe M.2 de 1,92 TB por cada 8 vCPU, con hasta 19,2 TB (10 x 1,92 TB) disponibles en la versión L80s v2.

> [!NOTE]
> Las máquinas virtuales de serie Lsv2 están optimizadas para usar el disco local en el nodo conectado directamente a la máquina virtual, en lugar de utilizar discos de datos duradero. Esto permite mayores IOPS por rendimiento para las cargas de trabajo. El Lsv2 y la serie Ls no admiten la creación de una caché local para aumentar el número de IOPs factible por los discos de datos duradero.
>
> El alto rendimiento e IOPs del disco local hace que la Lsv2 y máquinas virtuales de la serie Ls ideal para los almacenes NoSQL, como Apache Cassandra y MongoDB que replican datos entre varias máquinas virtuales para lograr persistencia en el caso de una sola máquina virtual.
>
> Para obtener más información, consulte [optimizar el rendimiento en las máquinas virtuales de serie Lsv2](../articles/virtual-machines/linux/storage-performance.md).  


## <a name="lsv2-series"></a>Serie Lsv2

ACU: 150-175

Premium Storage: Compatible

Caching de Premium Storage: No compatible

| Tamaño          | vCPU | Memoria (GiB) | Disco temporal <sup>1</sup> (GiB) | Discos NVMe<sup>2</sup> | Rendimiento de disco NVMe<sup>3</sup> (IOPS de lectura/MBps) | Rendimiento de disco de datos (IOPs/MBps) no en caché máx.<sup>4</sup> | Nº máx. de discos de datos | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|---------------|-----------|-------------|--------------------------|----------------|---------------------------------------------------|-------------------------------------------|------------------------------|------------------------------| 
| Standard_L8s_v2   |  8 |  64 |  80 |  1 de 1,92 TB  | 400000 / 2000 | 8000/160 | 16 | 2 / 3200  |
| Standard_L16s_v2  | 16 | 128 | 160 |  2 de 1,92 TB  | 800000 / 4000 | 16000/320 | 32 | 4 / 6400  |
| Standard_L32s_v2  | 32 | 256 | 320 |  4 de 1,92 TB  | 1.5 M / 8000    | 32000/640 | 32 | 8 / 12800 |
| Standard_L64s_v2  | 64 | 512 | 640 |  8 de 1,92 TB  | 2.9 M / 16000   | 64000/1280 | 32 | 8 / 16000+ |
| Standard_L80s_v2<sup>5</sup> | 80 | 640 | 800 | 10 de 1,92 TB   | 3.8 M / 20000   | 80000/1400 | 32 | 8 / 16000+ |

<sup>1</sup> Las máquinas virtuales de la serie Lsv2 tienen un disco de recursos temporal basado en el estándar SCSI para paginación o el archivo de intercambio del sistema operativo (D: en Windows, /dev/sdb en Linux). Dicho disco proporciona 80 GiB de almacenamiento, 4000 IOPS y una velocidad de transferencia de 80 MBps por cada 8 vCPU (p. ej., el tamaño Standard_L80s_v2 proporciona 800 GiB a 40 000 IOPS y 800 MBps). Esto garantiza que las unidades de NVMe se puedan dedicar completamente al uso de aplicaciones. Este disco es efímero y se perderán todos los datos al detenerlo o desasignarlo.

<sup>2</sup> Los discos NVMe locales son efímeros y los datos se perderán en estos discos si se detiene o desasigna la VM.

<sup>3</sup> La tecnología Hyper-V NVMe Direct proporciona acceso sin límite a las unidades de NVMe locales asignadas de forma segura al espacio de VM de invitado.  Para lograr el máximo rendimiento es necesario usar la última compilación WS2019 o Ubuntu 18.04 o 16.04 de Azure Marketplace.  El rendimiento de escritura varía en función del tamaño de E/S, la carga de unidad y la utilización de capacidad.

<sup>4</sup> Las VM de la serie Lsv2 no proporcionan almacenamiento caché de host para el disco de datos, ya que las cargas de trabajo de Lsv2 no se ven beneficiadas.  Sin embargo, las máquinas virtuales Lsv2 pueden admitir la opción de disco de sistema operativo efímero de máquina virtual de Azure (hasta 30 GiB).

<sup>5</sup> las máquinas virtuales con más de 64 vCPU requieren uno de estos sistemas operativos invitados compatibles:
- Windows Server 2016 o posterior
- Ubuntu 16.04 LTS o posterior, con Azure optimizado kernel (kernel 4.15 valor o una versión posterior)
- SLES 12 SP2 o posterior
- Versión RHEL o CentOS 6.7 a través de 6.10, con el paquete LIS proporcionada por Microsoft 4.3.1 (o posterior) instalado
- Versión RHEL o CentOS 7.3, con el paquete proporcionado por Microsoft LIS 4.2.1 (o posterior) instalado
- RHEL o CentOS versión 7.6 o posterior
- Oracle Linux con UEK4 o posterior
- Debian 9 con el kernel backports, Debian, 10 o posterior
- CoreOS con un kernel 4.14 o posterior


## <a name="size-table-definitions"></a>Definiciones de tabla de tamaño

- La capacidad de almacenamiento se muestra en unidades de GiB o 1024^3 bytes. Cuando compare discos que se miden en GB (1000^3 bytes) con discos que se miden en GiB (1024^3), recuerde que los números que representan la capacidad en GiB pueden parecer más pequeños. Por ejemplo, 1023 GiB = 1098.4 GB
- Se midió el rendimiento de disco en operaciones de entrada/salida por segundo (E/S por segundo) y MBps, donde Mbps = 10^6 bytes/s.
- Si desea obtener el mejor rendimiento para las VM, debe limitar el número de discos de datos a 2 discos por vCPU.
- **El ancho de banda de red esperado** es el [ancho de banda agregado máximo asignado por tipo de máquina virtual](../articles/virtual-network/virtual-machine-network-throughput.md) en todas las NIC y para todos los destinos. Los límites superiores no están garantizados pero están diseñados para proporcionar una guía a la hora de seleccionar el tipo correcto de máquina virtual para la aplicación deseada. El rendimiento de red real dependerá de diversos factores (como, por ejemplo, la congestión de la red, las cargas de la aplicación y la configuración de red). Para más información acerca de cómo optimizar el rendimiento de red, consulte [Optimización del rendimiento de red para Windows y Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Para lograr el rendimiento de red esperado en Linux o Windows, puede que sea necesario seleccionar una versión específica u optimizar la máquina virtual. Para más información, consulte [Pruebas confiables para el rendimiento de máquinas virtuales](../articles/virtual-network/virtual-network-bandwidth-testing.md).
