---
title: Colocar máquinas virtuales de Azure en Windows | Microsoft Docs
description: Obtenga información sobre cómo la colocación de recursos de máquina virtual de Azure puede mejorar la latencia.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: cynthn
ms.openlocfilehash: a6360e1ee8469f8674685f5975ec09db3e87a4ea
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850705"
---
# <a name="co-locate-resource-for-improved-latency"></a>Colocación de un recurso para mejorar la latencia

Al implementar la aplicación en Azure, la propagación de instancias entre regiones o zonas de disponibilidad crea una latencia de red, lo que puede afectar al rendimiento general de la aplicación. 


## <a name="preview-proximity-placement-groups"></a>Vista previa: Grupos de selección de ubicación de proximidad 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Pasos siguientes

Implemente una máquina virtual en un [grupo de selección de ubicación de proximidad](proximity-placement-groups.md) mediante Azure PowerShell.

