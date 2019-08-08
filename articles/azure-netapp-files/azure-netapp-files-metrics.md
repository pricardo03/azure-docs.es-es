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
ms.date: 08/06/2019
ms.author: b-juche
ms.openlocfilehash: 9a56fb27fdf9e196291942041d68b249d7f16648
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839237"
---
# <a name="metrics-for-azure-netapp-files"></a>Métricas de Azure NetApp Files

Azure NetApp Files proporciona métricas sobre el almacenamiento asignado, el uso de almacenamiento real, el rendimiento de volumen, el número de IOPS y la latencia. Mediante el análisis de estas métricas, puede comprender mejor el patrón de uso y el rendimiento de volumen de las cuentas de NetApp.  

## <a name="capacity_pools"></a>Métricas de uso de grupos de capacidad

<!-- 
- *Volume pool allocated size*  
    This is the size (GiB) of the provisioned capacity pool.  
--> 
- *Uso asignado del grupo de volumen*  
    Es la cuota de volumen (GiB) total en un grupo de capacidad determinado (es decir, el total de los tamaños aprovisionados de los volúmenes en el grupo de capacidad). Este es el tamaño que se selecciona durante la creación del volumen.  
- *Tamaño lógico total del grupo de volumen*  
    Es el espacio lógico (GiB) total usado en todos los volúmenes de un grupo de capacidad.  
<!-- 
- *Volume pool total snapshot size*  
    This is the total of incremental logical space used by the snapshots.  
-->

## <a name="volumes"></a>Métricas de uso de volúmenes

<!-- 
- *Volume allocated size*   
    This is the volume size (quota) provisioned in GiB.  
--> 
- *Tamaño lógico del volumen*   
    Es el espacio lógico total usado en un volumen (GiB). Este tamaño incluye el espacio lógico usado por las instantáneas y los sistemas de archivos activos.  
- *Tamaño de instantánea de volumen*   
    Es el espacio lógico incremental usado por las instantáneas de un volumen.  

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
* [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md)
