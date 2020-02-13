---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 06/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 8f256749c363e2900fe62bda557f7cb4caa72e3e
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77179076"
---
Los tamaños de máquina virtual optimizada para GPU son máquinas virtuales especializadas con GPU de NVIDIA. Estos tamaños están diseñados para cargas de trabajo de proceso intensivo, uso intensivo de gráficos y visualización. En este artículo se proporciona información sobre el número y el tipo de GPU, vCPU, discos de datos y NIC. El ancho de banda de red y el rendimiento del almacenamiento también se incluyen para cada tamaño de esta agrupación.

* Los tamaños de **NC, NCv2 y NCv3** están optimizados para aplicaciones y algoritmos que usan mucho la red y el procesador. Algunos ejemplos son las simulaciones y las aplicaciones basadas en CUDA y en OpenCL, la inteligencia artificial y el aprendizaje profundo. La serie NCv3 se centra en las cargas de trabajo de informática de alto rendimiento e incorpora la GPU NVIDIA Tesla V100. La serie NC utiliza el procesador Intel Xeon E5-2690 v3 a 2,60 GHz v3 (Haswell), y las series NCv2 y NCv3 utilizan el procesador Intel Xeon E5-2690 v4 (Broadwell).

* **ND y NDv2**. La serie ND se centra en escenarios de entrenamiento e inferencia para el aprendizaje profundo. Utiliza la GPU NVIDIA Tesla P40 y el procesador Intel Xeon E5-2690 v4 (Broadwell). La serie NDv2 usa el procesador Intel Xeon Platinum 8168 (Skylake).

* Los tamaños **NV y NVv3** están optimizados y diseñados para escenarios de visualización remota, streaming, juegos, codificación y VDI mediante plataformas como OpenGL y DirectX.  Estas máquinas virtuales están respaldadas por la GPU NVIDIA Tesla M60.

* Los tamaños **NVv4** están optimizados y diseñados para VDI y la visualización remota. Con las GPU con particiones, NVv4 ofrece el tamaño adecuado para las cargas de trabajo que requieren recursos de GPU más pequeños.  Estas máquinas virtuales usan la GPU AMD Radeon Instinct MI25.


## <a name="nc-series"></a>Serie NC

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

Las máquinas virtuales de la serie NC funcionan con la tarjeta [NVIDIA Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) y el procesador Intel Xeon E5-2690 v3 (Haswell). Los usuarios pueden trabajar con datos con mayor rapidez aprovechando CUDA para las aplicaciones de exploración de energía, simulaciones de accidentes, la representación de trazado de rayos, el aprendizaje profundo y mucho más. La configuración NC24r proporciona una interfaz de red de baja latencia y alto rendimiento optimizada para cargas de trabajo de computación paralelas estrechamente unidas.

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 12 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 24 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = media tarjeta K80.

*Compatible con RDMA

## <a name="ncv2-series"></a>Serie NCv2

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Las VM de la serie NCv2 disponen de tecnología de GPU [NVIDIA Tesla P100](https://www.nvidia.com/en-us/data-center/tesla-p100/). Estas GPU pueden duplicar el rendimiento del trabajo de computación de la serie NC. Los clientes pueden aprovechar estas GPU actualizadas para cargas de trabajo de HPC tradicionales, como la creación de modelos de embalses, la secuenciación de ADN, el análisis de proteínas, la realización de simulaciones Monte Carlo y otras. Además de las GPU, las máquinas virtuales de la serie NCv2 también están equipadas con CPU Intel Xeon E5-2690 v4 (Broadwell).

La configuración NC24rs v2 proporciona una interfaz de red de baja latencia y alto rendimiento optimizada para cargas de trabajo de computación paralelas estrechamente unidas.

> [!IMPORTANT]
> Para esta familia de tamaño, la cuota de vCPU (core) en su suscripción está establecida inicialmente en 0 en cada región. [Solicite un aumento de cuota de vCPU](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md) para esta familia en una [región donde esté disponible](https://azure.microsoft.com/regions/services/).
>

| Size | vCPU | Memoria: GiB | Almacenamiento temporal (SSD): GiB | GPU | Memoria de GPU: GiB | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | ---  | ---| --- |
| Standard_NC6s_v2 | 6 |112 | 736 | 1 | 16 | 12 | 20000/ 200 | 4 |
| Standard_NC12s_v2 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v2 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v2* | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = una tarjeta P100.

*Compatible con RDMA

## <a name="ncv3-series"></a>Serie NCv3

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Las VM de la serie NCv3 disponen de tecnología de GPU [NVIDIA Tesla V100](https://www.nvidia.com/en-us/data-center/tesla-v100/). Estas GPU pueden aumentar 1,5 veces el rendimiento del trabajo de computación de la serie NCv2. Los clientes pueden aprovechar estas GPU actualizadas para cargas de trabajo de HPC tradicionales, como la creación de modelos de embalses, la secuenciación de ADN, el análisis de proteínas, la realización de simulaciones Monte Carlo y otras. La configuración NC24rs v3 proporciona una interfaz de red de baja latencia y alto rendimiento optimizada para cargas de trabajo de computación paralelas estrechamente unidas. Además de las GPU, las máquinas virtuales de la serie NCv3 también están equipadas con CPU Intel Xeon E5-2690 v4 (Broadwell).

> [!IMPORTANT]
> Para esta familia de tamaño, la cuota de vCPU (core) en su suscripción está establecida inicialmente en 0 en cada región. [Solicite un aumento de cuota de vCPU](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md) para esta familia en una [región donde esté disponible](https://azure.microsoft.com/regions/services/).
>

| Size | vCPU | Memoria: GiB | Almacenamiento temporal (SSD): GiB | GPU | Memoria de GPU: GiB | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = una tarjeta V100

*Compatible con RDMA

## <a name="updated-ndv2-series-preview"></a>Serie NDv2 actualizada (versión preliminar)

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

InfiniBand: Compatible

La máquina virtual de la serie NDv2 es una nueva incorporación a la familia de GPU diseñada para las necesidades de las cargas de trabajo más exigentes de informática de alto rendimiento, inteligencia artificial aprendizaje automático y simulación de aceleración por GPU. 

NDv2 cuenta con 8 GPU NVIDIA Tesla V100 conectadas mediante NVLINK, cada una con 32 GB de memoria de GPU. Cada máquina virtual NDv2 también tiene 40 núcleos Intel Xeon Platinum 8168 (Skylake) sin hyperthreading y 672 GiB de memoria del sistema. 

Las instancias de NDv2 proporcionan un rendimiento excelente para las cargas de trabajo de IA y HPC mediante kernels de cálculo optimizados para GPU de CUDA y las numerosas herramientas de inteligencia artificial, Machine Learning y análisis que admiten la aceleración GPU de serie, como TensorFlow, Pytorch, Caffe, RAPIDS y otras plataformas. 

Fundamentalmente, NDv2 se ha creado para cargas de trabajo de escalado vertical (con 8 GPU por máquina virtual) y escalado horizontal (con varias máquinas virtuales que funcionan de forma conjunta) que usan muchos cálculos. La serie NDv2 ahora admite redes de back-end InfiniBand EDR de 100 Gigabits, similares a las que están disponibles en la serie HB de máquinas virtuales HPC, para permitir la agrupación en clústeres de alto rendimiento en escenarios paralelos, incluido el entrenamiento distribuido para IA y ML. Esta red de back-end es compatible con todos los protocolos de InfiniBand principales, incluidos los empleados por las bibliotecas NCCL2 de NVIDIA, lo que permite la agrupación en clústeres de las GPU sin problemas.

> Al [habilitar InfiniBand](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband) en la máquina virtual ND40rs_v2, use el controlador OFED de Mellanox 4.7-1.0.0.1.

> Debido a la mayor cantidad de memoria de GPU, la nueva máquina virtual ND40rs_v2 necesita el uso de [máquinas virtuales de generación 2](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) y de imágenes de Marketplace. 

> [Regístrese para solicitar acceso temprano a la versión preliminar de la máquina virtual NDv2.](https://aka.ms/AzureNDrv2Preview)

> Tenga en cuenta lo siguiente: ND40s_v2 con 16 GB de memoria por GPU ya no está disponible para la versión preliminar y se ha reemplazado por la versión actualizada de ND40rs_v2.
<br>

| Size | vCPU | Memoria: GiB | Almacenamiento temporal (SSD): GiB | GPU | Memoria de GPU: GiB | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Ancho de banda de red máx. | Nº máx. NIC |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40rs_v2 | 40 | 672 | 2948 | 8 V100 32 GB (NVLink) | 16 | 32 | 80000/800 | 24 000 Mbps | 8 |

## <a name="nd-series"></a>Serie ND

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Las máquinas virtuales de serie ND son una novedad incorporada a la familia GPU diseñada para cargas de trabajo inteligencia artificial y aprendizaje profundo. Ofrecen un rendimiento excelente para el aprendizaje y la inferencia. Las instancias de ND funcionan con GPU [NVIDIA Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) y CPU Intel Xeon E5-2690 v4 (Broadwell). Estas instancias brindan un rendimiento excelente para operaciones de punto flotante de precisión única, para cargas de trabajo de inteligencia artificial que usan Microsoft Cognitive Toolkit, TensorFlow, Caffe y otros marcos. La serie ND también ofrece una memoria de la GPU de un tamaño muy superior (24 GB), lo que permite adaptarse a modelos de redes neurales mucho más grandes. Al igual que la serie NC, la serie ND presenta una configuración con una baja latencia secundaria, una red de alta productividad mediante RDMA y conectividad InfiniBand para que pueda ejecutar trabajos de aprendizaje a gran escala que abarquen muchas GPU.

> [!IMPORTANT]
> Para esta familia de tamaño, la cuota de vCPU (core) por región en su suscripción está establecida inicialmente en 0. [Solicite un aumento de cuota de vCPU](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md) para esta familia en una [región donde esté disponible](https://azure.microsoft.com/regions/services/).
>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s | 6 |112 | 736 | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s | 12 |224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 | 
| Standard_ND24s | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |
| Standard_ND24rs* | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

1 GPU = una tarjeta de P40.

*Compatible con RDMA

## <a name="nv-series"></a>Serie NV

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

Las máquinas virtuales de la serie NV dispone de tarjetas GPU [Tesla M60 de NVIDIA](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) y tecnología NVIDIA GRID para aplicaciones aceleradas de escritorio y escritorios virtuales donde los clientes pueden visualizar sus datos o simulaciones. Los usuarios pueden visualizar sus flujos de trabajo con muchos gráficos en las instancias de NV para obtener una excelente funcionalidad gráfica y ejecutar, además, cargas de trabajo de precisión únicas, como la codificación y la representación. Las máquinas virtuales de la serie NV también funcionan con CPU Intel Xeon E5-2690 v3 (Haswell).

Cada GPU de instancias de NV viene con una licencia de GRID. Esta licencia le ofrece flexibilidad para utilizar una instancia de NV como estación de trabajo virtual para un solo usuario; también se pueden conectar 25 usuarios simultáneos a la VM para un escenario de aplicación virtual.

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC | Estaciones de trabajo virtuales | Aplicaciones virtuales |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = media tarjeta M60.

## <a name="nvv3-series--sup1sup"></a>Serie NVv3 <sup>1</sup>

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Las máquinas virtuales de la serie NVv3 cuentan con la tecnología de las GPU [Nvidia Test M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) y la tecnología GRID de NVIDIA con CPU Intel E5-2690 v4 (Broadwell). Estas máquinas virtuales están orientadas a escritorios virtuales y aplicaciones gráficas aceleradas mediante GPU donde los clientes desean ver sus datos, simular resultados para verlos, trabajar en CAD o representar y transmitir contenido. Además, estas máquinas virtuales pueden ejecutar cargas de trabajo de precisión única, como la codificación y la representación. Las máquinas virtuales NVv3 son compatibles con Premium Storage y traen el doble de memoria del sistema (RAM) si se comparan con la serie NV anterior.  

Cada GPU de las instancias de NVv3 viene con una licencia de GRID. Esta licencia le ofrece flexibilidad para utilizar una instancia de NV como estación de trabajo virtual para un solo usuario; también se pueden conectar 25 usuarios simultáneos a la VM para un escenario de aplicación virtual.

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. NIC | Estaciones de trabajo virtuales | Aplicaciones virtuales | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV12s_v3 |12 |112 |736 | 1 | 8 | 12 | 20000/200 | 4 | 1 | 25 |
| Standard_NV24s_v3 |24 |224 |1474 | 2 | 16 | 24 | 40000/400 | 8 | 2 | 50 |
| Standard_NV48s_v3 |48 |448 |2948 | 4 | 32 | 32 | 80000/800 | 8 | 4 | 100 |

1 GPU = media tarjeta M60.

<sup>1</sup> Las máquinas virtuales de la serie NVv3 cuentan con la tecnología Hyper-Threading de Intel.

## <a name="nvv4-series-preview--sup1sup"></a>Serie NVv4 (versión preliminar) <sup>1</sup>

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Las máquinas virtuales de la serie NVv4 usan la tecnología de las GPU [AMD Radeon instinto MI25](https://www.amd.com/en/products/professional-graphics/instinct-mi25) y las CPU AMD EPYC 7V12 (Rome). Con la serie NVv4, Azure introduce máquinas virtuales con GPU parciales. Elija la máquina virtual del tamaño adecuado para aplicaciones con aceleración gráfica por GPU y escritorios virtuales a partir de un octavo de una GPU con un búfer de cuadros de 2 GiB y hasta una GPU completa con un búfer de cuadros de 16 GiB. Las máquinas virtuales NVv4 actualmente solo admiten el sistema operativo invitado Windows.

[Regístrese y acceda a estas máquinas durante la versión preliminar](https://aka.ms/nvv4signup).
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV4as_v4 |4 |14 |88 | 1/8 | 2 | 4 | 2 |
| Standard_NV8as_v4 |8 |28 |176 | 1/4 | 4 | 8 | 4 |
| Standard_NV16as_v4 |16 |56 |352 | 1/2 | 8 | 16 | 8 | 
| Standard_NV32as_v4 |32 |112 |704 | 1 | 16 | 32 | 8 | 



<sup>1</sup> las máquinas virtuales de la serie NVv4 incluyen tecnología multithreading simultánea de AMD

