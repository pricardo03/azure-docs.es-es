---
title: Límites de recursos para Azure NetApp Files| Microsoft Docs
description: Describe los límites de recursos para Azure NetApp Files, incluidos los límites de las cuentas de NetApp, los grupos de capacidad, los volúmenes, instantáneas y la subred delegada.
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
ms.topic: concepts
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: 196d85917e0a9900e141d58bff171beeb8540409
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430020"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Límites de recursos para Azure NetApp Files

Entender los límites de recursos de Azure NetApp Files ayuda a administrar los volúmenes.

- Cada suscripción de Azure puede tener un máximo de 10 cuentas de NetApp.
- Cada cuenta de NetApp puede tener un máximo de 25 grupos de capacidad.
- Cada grupo de capacidad puede pertenecer a una sola cuenta de NetApp.  
- El tamaño mínimo para un grupo de capacidades único es de 4 TiB, y el tamaño máximo es de 500 TiB. 
- Cada grupo de capacidad puede tener un máximo de 500 volúmenes.
- El tamaño mínimo para un volumen único es de 100 GiB, y el tamaño máximo es de 92 TiB.
- Cada volumen puede tener un máximo de 255 instantáneas.
- En cada instancia de Azure Virtual Network (red virtual), solo puede delegarse una subred a Azure NetApp Files.

**Pasos siguientes**

[Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
