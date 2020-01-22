---
title: Compatibilidad a la hora de agregar máquinas virtuales de Azure a un conjunto de disponibilidad existente | Microsoft Docs
description: En este artículo se proporciona una matriz de compatibilidad sobre la serie de máquinas virtuales que se pueden combinar en el mismo conjunto de disponibilidad.
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: cb0034f2b353284e94d6f1508541b31040a5b076
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028409"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Compatibilidad a la hora de agregar máquinas virtuales de Azure a un conjunto de disponibilidad existente

En ocasiones, puede encontrar limitaciones al agregar nuevas máquinas virtuales (VM) a un conjunto de disponibilidad existente. El siguiente gráfico detalla las series de máquinas virtuales que puede mezclar en el mismo conjunto de disponibilidad.

Esta es la matriz de compatibilidad para mezclar distintos tipos de máquinas virtuales:

Serie y conjunto de disponibilidad|Segunda máquina virtual|Un|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Primera máquina virtual|||||||
|Un||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|Av2||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|D||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|Dv2||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|Dv3||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|

Todas las otras series no pueden estar en el mismo conjunto de disponibilidad porque requieren un hardware específico.

El tamaño de una máquina virtual A8/A9 no se puede combinar debido al requisito sobre la red de backend de RDMA dedicada.
