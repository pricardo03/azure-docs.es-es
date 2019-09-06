---
title: Configuraciones de nodos admitidas de Azure HDInsight
description: Obtenga información sobre las configuraciones mínimas y recomendadas para los nodos de clúster de HDInsight.
keywords: tamaños de vm, tamaños de clúster, configuración de clúster
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: e482cf9b5367beba00784e69c5bad88142df5225
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076076"
---
# <a name="what-are-the-default-and-recommended-node-configurations-for-azure-hdinsight"></a>¿Cuáles son las configuraciones de nodos predeterminadas y recomendadas para Azure HDInsight?

En este artículo se tratan las configuraciones de nodos predeterminadas y recomendadas para los clústeres de Azure HDInsight.

## <a name="default-and-recommended-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Configuración de nodos predeterminada y recomendada y tamaños de máquina virtual para clústeres

En las tablas siguientes se indican los tamaños de máquina virtual predeterminados y recomendados para clústeres de HDInsight.  Esta información es necesaria para entender qué tamaños de VM se deben usar al crear scripts de PowerShell o la CLI de Azure para implementar clústeres de HDInsight. 

Si necesita más de 32 nodos de trabajo en un clúster, seleccione un tamaño de nodo principal con al menos 8 núcleos y 14 GB de RAM. 

Los únicos tipos de clúster que tienen discos de datos son los clústeres Kafka y HBase con la característica Escrituras aceleradas habilitada. HDInsight admite los tamaños de disco P30 y S30 en estos escenarios.

Para obtener más detalles sobre la especificación de cada tipo de máquina virtual, consulte los siguientes documentos:

* [Tamaños de máquina virtual de uso general: serie Dv2 1-5](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dv2-series)
* [Tamaños de máquina virtual optimizada para memoria: serie Dv2 11-15](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#dv2-series-11-15)
* [Tamaños de máquina virtual de uso general: serie Av2 1-8](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#av2-series)

### <a name="all-supported-regions-except-brazil-south-and-japan-west"></a>Todas las regiones admitidas, excepto Sur de Brasil y Japón Occidental

> [!Note]
> Para obtener el identificador de SKU para su uso en PowerShell y otros scripts, agregue `Standard_` al principio de todas las SKU de máquina virtual en las tablas siguientes. Por ejemplo, `D12_v2` se convertiría en `Standard_D12_v2`.

| Tipo de clúster | Hadoop | HBase | Interactive Query | Storm | Spark | ML Server | Kafka |
|---|---|---|---|---|---|---|---|
| Principal: tamaño de máquina virtual predeterminado | D12_v2 | D12_v2 | D13_v2 | A4_v2 | D12_v2 | D12_v2 | D3_v2 |
| Principal: tamaños de máquina virtual recomendados | D13_v2,<br/>D14_v2,<br/>D5_v2 | D3_v2,<br/>D4_v2,<br/>D12_v2 | D13_v2,<br/>D14_v2 | A4_v2,<br/>A8_v2 | D12_v2,<br/>D13_v2,<br/>D14_v2 | D12_v2,<br/>D13_v2,<br/>D14_v2 | D3_v2,<br/>D4_v2,<br/>D12_v2 |
| Trabajo: tamaño de máquina virtual predeterminado | D4_v2 | D4_v2 | D14_v2 | D3_v2 | D13_v2 | D4_v2 | 4 D12_v2 con 2 discos S30 por agente |
| Trabajo: tamaños de máquina virtual recomendados | D5_v2,<br>D12_v2,<br/>D13_v2 | D3_v2,<br/>D4_v2,<br/>D13_v2 | D13_v2,<br/>D14_v2 | D3_v2<br/>D4_v2,<br/>D12_v2 | D12_v2,<br>D13_v2,<br>D14_v2 | D4_v2,<br/>D12_v2,<br>D13_v2,<br>D14_v2 | D3_v2,<br/>D4_v2,<br/>DS3_v2,<br/>DS4_v2 |
| Zookeeper: tamaño de máquina virtual predeterminado |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 | A4_v2 |
| Zookeeper: tamaños de máquina virtual recomendados |  | A4_v2, <br/>A8_v2, <br/>A2m_v2 | A4_v2,<br/>A8_v2,<br/>A2m_v2 | A4_v2,<br/>A2_v2,<br/>A8_v2 |  | A2_v2 | A4_v2,<br/> A8_v2,<br/>A2m_v2 |
| Servicio Machine Learning: tamaño de máquina virtual predeterminado |  |  |  |  |  | D4_v2 |  |
| Servicio Machine Learning: tamaño de máquina virtual recomendado |  |  |  |  |  | D4_v2,<br/> D12_v2,<br/> D13_v2,<br/>D14_v2 |  |

### <a name="brazil-south-and-japan-west-only"></a>Solo Sur de Brasil y Japón Occidental

| Tipo de clúster | Hadoop | HBase | Interactive Query | Storm | Spark | Machine Learning Services |
|---|---|---|---|---|---|---|
| Principal: tamaño de máquina virtual predeterminado | D12 | D12 | D13 | A4_v2 | D12 | D12 |
| Principal: tamaños de máquina virtual recomendados | D5_v2,<br/> D13_v2,<br/> D14_v2 | D3_v2,<br/> D4_v2,<br/> D12_v2 | D13_v2,<br/> D14_v2 | A4_v2,<br/> A8_v2 | D12_v2,<br/> D13_v2,<br/> D14_v2 | D12_v2,<br/> D13_v2,<br/> D14_v2 |
| Trabajo: tamaño de máquina virtual predeterminado | D4 | D4 | D14 | D3 | D13 | D4 |
| Trabajo: tamaños de máquina virtual recomendados | D5_v2,<br/> D12_v2,<br/> D13_v2 | D3_v2,<br/> D4_v2,<br/> D13_v2 | D13_v2,<br/> D14_v2 | D3_v2,<br/> D4_v2,<br/> D12_v2 | D12_v2,<br/> D13_v2,<br/> D14_v2 | D4_v2,<br/> D12_v2,<br/> D13_v2,<br/> D14_v2 |
| Zookeeper: tamaño de máquina virtual predeterminado |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| Zookeeper: tamaños de máquina virtual recomendados |  | A4_v2,<br/> A8_v2,<br/> A2m_v2 | A4_v2,<br/> A8_v2,<br/> A2m_v2 | A4_v2,<br/> A8_v2 |  | A2_v2 |
| Servicio Machine Learning: tamaños de máquina virtual predeterminados |  |  |  |  |  | D4 |
| Servicio Machine Learning: tamaños de máquina virtual recomendados |  |  |  |  |  | D4_v2,<br/> D12_v2,<br/> D13_v2,<br/> D14_v2 |

> [!NOTE]
> - Nota: Principal se conoce como *Nimbus* para el tipo de clúster de Storm.
> - El trabajo se conoce como *Supervisor* para el tipo de clúster de Storm.
> - El trabajo se conoce como *Región* para el tipo de clúster de HBase.

## <a name="next-steps"></a>Pasos siguientes

* [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](hdinsight-component-versioning.md)
