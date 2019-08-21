---
title: Métricas de Azure NetApp Files | Microsoft Docs
description: Se describen las métricas de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 7cf382f511d2ba8452d77bf207f36b749cb31e94
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848788"
---
# <a name="metrics-for-azure-netapp-files"></a>Métricas de Azure NetApp Files

Azure NetApp Files proporciona métricas sobre el almacenamiento asignado, el uso de almacenamiento real, el rendimiento de volumen, el número de IOPS y la latencia. Mediante el análisis de estas métricas, puede comprender mejor el patrón de uso y el rendimiento de volumen de las cuentas de NetApp.  

## <a name="capacity_pools"></a>Métricas de uso de grupos de capacidad

<!-- 
- *Volume pool allocated size*  
    The size (GiB) of the provisioned capacity pool  
--> 
- *Uso asignado del grupo de volumen*  
    La cuota de volumen (GiB) total en un grupo de capacidad determinado (es decir, el total de los tamaños aprovisionados de los volúmenes en el grupo de capacidad).  
    Este es el tamaño que se selecciona durante la creación del volumen.  
- *Tamaño lógico total del grupo de volumen*  
    El espacio lógico (GiB) total usado en todos los volúmenes de un grupo de capacidad.  
<!-- 
- *Volume pool total snapshot size*  
    The total of incremental logical space used by the snapshots  
-->

## <a name="volumes"></a>Métricas de uso de volúmenes

<!-- 
- *Volume allocated size*   
    The volume size (quota) provisioned in GiB  
--> 
- *Tamaño lógico del volumen*   
    El espacio lógico total usado en un volumen (GiB).  
    Este tamaño incluye el espacio lógico usado por las instantáneas y los sistemas de archivos activos.  
- *Tamaño de instantánea de volumen*   
   El espacio lógico incremental usado por las instantáneas de un volumen.  

## <a name="performance-metrics-for-volumes"></a>Métricas de rendimiento de los volúmenes

- *AverageReadLatency*   
    Tiempo promedio de las lecturas del volumen en milisegundos.
- *AverageWriteLatency*   
    Tiempo promedio de las escrituras del volumen en milisegundos.
- *ReadIops*   
    Número de lecturas en el volumen por segundo.
- *WriteIops*   
    Número de escrituras en el volumen por segundo.

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
* [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md)
