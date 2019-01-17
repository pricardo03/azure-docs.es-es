---
title: Límites de recursos para Azure NetApp Files| Microsoft Docs
description: Describe los límites de recursos de Azure Files de NetApp, incluidos los límites de los grupos de capacidad, los volúmenes y la subred delegada.
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
ms.topic: reference
ms.date: 01/03/2019
ms.author: b-juche
ms.openlocfilehash: f34afb1df2ae38353f29a80bfb6798c16856dbeb
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54056739"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Límites de recursos para Azure NetApp Files
Entender los límites de recursos de Azure NetApp Files ayuda a administrar los volúmenes.

## <a name="capacity_pools"></a>Grupos de capacidad

- El tamaño mínimo para un grupo de capacidades único es de 4 TiB, y el tamaño máximo es de 500 TiB. 
- Cada grupo de capacidad puede pertenecer a una sola cuenta de NetApp. Sin embargo, puede tener varios grupos de capacidad dentro de una cuenta de NetApp.  

## <a name="volumes"></a>Volúmenes

- El tamaño mínimo para un volumen único es de 100 GiB, y el tamaño máximo es de 92 TiB.
- Puede tener un máximo de 100 volúmenes por suscripción de Azure y por región.  

## <a name="delegated_subnet"></a>Subred delegada 

En cada red virtual de Azure (VNet), solo puede delegarse una subred a Azure NetApp Files.

## <a name="next-steps"></a>Pasos siguientes

[Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
