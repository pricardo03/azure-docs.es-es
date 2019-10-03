---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 0f528117b19a3f26b964dee7ae453fc184c8dddb
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123066"
---
Azure Compute ofrece tamaños de máquinas virtuales que están aislados para un tipo concreto de hardware y dedicados a un solo cliente.  Estos tamaños de máquina virtual son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes como, por ejemplo, las cargas de trabajo que incluyen elementos como el cumplimiento normativo y los requisitos legales.  Los clientes también puede elegir subdividir aún más los recursos de estas máquinas virtuales aisladas mediante la [compatibilidad de Azure para máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).

Usar un tamaño aislado garantiza que la máquina virtual será la única que se ejecute en esa instancia de servidor específica.  Las ofertas de máquinas virtuales aisladas actuales incluyen:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2
* Standard_F72s_v2

Puede más información sobre cada tamaño aislado disponible [aquí](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory).