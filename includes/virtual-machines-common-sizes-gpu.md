---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 11/14/2018
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: b6df4ada1aa13e20c7ad52d2b58cdf9c783f9e24
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51716274"
---
Los tamaños de máquina virtual optimizada para GPU son máquinas virtuales especializadas con GPU de NVIDIA. Estos tamaños están diseñados para cargas de trabajo de proceso intensivo, uso intensivo de gráficos y visualización. En este artículo se proporciona información sobre el número y el tipo de GPU, vCPU, discos de datos y NIC. El ancho de banda de red y el rendimiento del almacenamiento también se incluyen para cada tamaño de esta agrupación. 

* Los tamaños **NC, NCv2, NCv3, ND, and NDv2** están optimizados para aplicaciones y algoritmos que usan mucho la red y los procesos. Algunos ejemplos son las simulaciones y las aplicaciones basadas en CUDA y en OpenCL, la inteligencia artificial y el aprendizaje profundo. La serie NCv3 se centra en las cargas de trabajo de informática de alto rendimiento e incorpora la GPU NVIDIA Tesla V100.  La serie ND se centra en escenarios de aprendizaje e inferencia para el aprendizaje profundo. Usa la GPU NVIDIA Tesla P40.
* Los tamaños **NV y NVv2** están optimizados y diseñados para escenarios de visualización remota, streaming, juegos, codificación y VDI mediante plataformas como OpenGL y DirectX.  Estas máquinas virtuales están respaldadas por la GPU NVIDIA Tesla M60.


## <a name="nc-series"></a>Serie NC

Premium Storage: no se admite

Almacenamiento en caché de Premium Storage: no se admite

Las VM de la serie NC disponen de una tarjeta [Tesla K80 de NVIDIA](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf). Los usuarios pueden trabajar con datos con mayor rapidez aprovechando CUDA para las aplicaciones de exploración de energía, simulaciones de accidentes, la representación de trazado de rayos, el aprendizaje profundo y mucho más. La configuración NC24r proporciona una interfaz de red de baja latencia y alto rendimiento optimizada para cargas de trabajo de computación paralelas estrechamente unidas.


| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 8 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 | 64 | 4 |

1 GPU = media tarjeta K80.

*Compatible con RDMA

## <a name="ncv2-series"></a>Serie NCv2

Premium Storage: se admite

Almacenamiento en caché de Premium Storage: se admite

Las VM de la serie NCv2 disponen de tecnología de GPU [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf). Estas GPU pueden duplicar el rendimiento del trabajo de computación de la serie NC. Los clientes pueden aprovechar estas GPU actualizadas para cargas de trabajo de HPC tradicionales, como la creación de modelos de embalses, la secuenciación de ADN, el análisis de proteínas, la realización de simulaciones Monte Carlo y otras. La configuración NC24rs v2 proporciona una interfaz de red de baja latencia y alto rendimiento optimizada para cargas de trabajo de computación paralelas estrechamente unidas.

> [!IMPORTANT]
> Para esta familia de tamaño, la cuota de vCPU (core) en su suscripción está establecida inicialmente en 0 en cada región. [Solicite un aumento de cuota de vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta familia en una [región donde esté disponible](https://azure.microsoft.com/regions/services/).
>

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | ---  | --- |
| Standard_NC6s_v2 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 2948 | 4 | 64 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = una tarjeta P100.

*Compatible con RDMA

## <a name="ncv3-series"></a>Serie NCv3

Premium Storage: se admite

Almacenamiento en caché de Premium Storage: se admite

Las VM de la serie NCv3 disponen de tecnología de GPU [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf). Estas GPU pueden aumentar 1,5 veces el rendimiento del trabajo de computación de la serie NCv2. Los clientes pueden aprovechar estas GPU actualizadas para cargas de trabajo de HPC tradicionales, como la creación de modelos de embalses, la secuenciación de ADN, el análisis de proteínas, la realización de simulaciones Monte Carlo y otras. La configuración NC24rs v3 proporciona una interfaz de red de baja latencia y alto rendimiento optimizada para cargas de trabajo de computación paralelas estrechamente unidas.

> [!IMPORTANT]
> Para esta familia de tamaño, la cuota de vCPU (core) en su suscripción está establecida inicialmente en 0 en cada región. [Solicite un aumento de cuota de vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta familia en una [región donde esté disponible](https://azure.microsoft.com/regions/services/).
>

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 64 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = una tarjeta V100

*Compatible con RDMA

## <a name="ndv2-series-preview"></a>Serie NDv2 (versión preliminar)


Premium Storage: se admite

Almacenamiento en caché de Premium Storage: se admite

InfiniBand: no se admite


La máquina virtual de la serie NDv2 es una nueva incorporación a la familia de GPU diseñada para las necesidades de las cargas de informática de alto rendimiento, inteligencia artificial y aprendizaje automático. Cuenta con 8 GPU NVIDIA Tesla V100 NVLINK interconectadas, 40 núcleos Intel Skylake y 672 GiB de memoria del sistema. La instancia de NDv2 proporciona un excelente rendimiento de FP32 y FP64 para cargas de trabajo de HPC e inteligencia artificial mediante Cuda, TensorFlow, Pytorch, Caffe y otros marcos.

[Regístrese y acceda a estas máquinas durante la versión preliminar](https://aka.ms/ndv2signup).
<br>


| Tamaño              | vCPU | GPU              | Memoria  | NICs (Máx) | Máx. del disco           | Máx. Discos de datos (1023 GB cada uno) | Ancho de banda de red máx. | 
|-------------------|-------------|-------------------|--------|------------------|---------|------------|--------------------------|--------------------|--------------------------------|-----------------------------------------|-----------------------|------------|
| Standard_ND40s_v2 | 40     | 8 V100 (NVlilnk) | 672 GiB | 8          | 1344 / 2948XIO temporal | 32    | 24 000 Mbps             | 

## <a name="nd-series"></a>Serie ND

Premium Storage: se admite

Almacenamiento en caché de Premium Storage: se admite

Las máquinas virtuales de serie ND son una novedad incorporada a la familia GPU diseñada para cargas de trabajo inteligencia artificial y aprendizaje profundo. Ofrecen un rendimiento excelente para el aprendizaje y la inferencia. Las instancias de ND disponen de tecnología de GPU [Tesla P40 de NVIDIA](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf). Estas instancias brindan un rendimiento excelente para operaciones de punto flotante de precisión única, para cargas de trabajo de inteligencia artificial que usan Microsoft Cognitive Toolkit, TensorFlow, Caffe y otros marcos. La serie ND también ofrece una memoria de la GPU de un tamaño muy superior (24 GB), lo que permite adaptarse a modelos de redes neurales mucho más grandes. Al igual que la serie NC, la serie ND presenta una configuración con una baja latencia secundaria, una red de alta productividad mediante RDMA y conectividad InfiniBand para que pueda ejecutar trabajos de aprendizaje a gran escala que abarquen muchas GPU.

> [!IMPORTANT]
> Para esta familia de tamaño, la cuota de vCPU (core) por región en su suscripción está establecida inicialmente en 0. [Solicite un aumento de cuota de vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta familia en una [región donde esté disponible](https://azure.microsoft.com/regions/services/).
>

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 736 | 1 | 24 | 12 | 4 |
| Standard_ND12s |12 |224 | 1474 | 2 | 48 | 24 | 8 | 
| Standard_ND24s |24 |448 | 2948 | 4 | 96 | 32 | 8 |
| Standard_ND24rs* |24 |448 | 2948 | 4 | 96 | 32 | 8 |

1 GPU = una tarjeta de P40.

*Compatible con RDMA

## <a name="nv-series"></a>Serie NV

Premium Storage: no se admite

Almacenamiento en caché de Premium Storage: no se admite

Las máquinas virtuales de la serie NV dispone de tarjetas GPU [Tesla M60 de NVIDIA](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) y tecnología NVIDIA GRID para aplicaciones aceleradas de escritorio y escritorios virtuales donde los clientes pueden visualizar sus datos o simulaciones. Los usuarios pueden visualizar sus flujos de trabajo con muchos gráficos en las instancias de NV para obtener una excelente funcionalidad gráfica y ejecutar, además, cargas de trabajo de precisión únicas, como la codificación y la representación. 

Cada GPU de instancias de NV viene con una licencia de GRID. Esta licencia le ofrece flexibilidad para utilizar una instancia de NV como estación de trabajo virtual para un solo usuario; también se pueden conectar 25 usuarios simultáneos a la VM para un escenario de aplicación virtual.

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC | Estaciones de trabajo virtuales | Aplicaciones virtuales | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV6 |6 |56 |340 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = media tarjeta M60.

## <a name="nvv2-series-preview"></a>Serie NVv2 (versión preliminar)

Premium Storage: se admite

Almacenamiento en caché de Premium Storage: se admite

Las máquinas virtuales de la serie NVv2 cuentan con la tecnología de las GPU [Nvidia Test M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) y la tecnología GRID de NVIDIA con CPU Intel Broadwell. Estas máquinas virtuales están orientadas a escritorios virtuales y aplicaciones gráficas aceleradas mediante GPU donde los clientes desean ver sus datos, simular resultados para verlos, trabajar en CAD o representar y transmitir contenido. Además, estas máquinas virtuales pueden ejecutar cargas de trabajo de precisión única, como la codificación y la representación. Las máquinas virtuales NVv2 son compatibles con Premium Storage y traen el doble de memoria del sistema (RAM) si se comparan con la serie NV anterior.  

Cada GPU de las instancias de NVv2 viene con una licencia de GRID. Esta licencia le ofrece flexibilidad para utilizar una instancia de NV como estación de trabajo virtual para un solo usuario; también se pueden conectar 25 usuarios simultáneos a la VM para un escenario de aplicación virtual.

| Tamaño | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC | Estaciones de trabajo virtuales | Aplicaciones virtuales | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1 | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

1 GPU = media tarjeta M60.

 
