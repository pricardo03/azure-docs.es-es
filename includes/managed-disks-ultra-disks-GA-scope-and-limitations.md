---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/18/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: df488c2a3033015175943f7d092beaaa41e34046
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260875"
---
Por ahora, los discos Ultra tienen limitaciones adicionales, como se indica a continuación:

- Se admiten en las siguientes regiones, con un número variable de zonas de disponibilidad por región:
    - Este de EE. UU. 2
    - East US
    - Oeste de EE. UU. 2
    - Sudeste Asiático
    - Europa del Norte
    - Europa occidental
    - Sur de Reino Unido 2 
- Solo se podrán usar con las zonas de disponibilidad (los conjuntos de disponibilidad y las implementaciones de máquinas virtuales únicas fuera de las zonas no tendrán la capacidad de adjuntar un disco Ultra).
- Solo se admiten en las siguientes series de máquinas virtuales:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- No todos los tamaños de máquina virtual están disponibles en todas las regiones admitidas con discos Ultra.
- Solo están disponibles como discos de datos y solo admiten el tamaño de sector físico 4k.  
- Solo pueden crearse como discos vacíos.  
- Todavía no admiten instantáneas de disco, imágenes de máquinas virtuales, conjuntos de disponibilidad ni Azure Disk Encryption.
- Todavía no admiten la integración con Azure Backup o Azure Site Recovery.
- El límite máximo actual de IOPS en máquinas virtuales de disponibilidad general es 80 000.
- Si desea participar en una versión preliminar limitada de una máquina virtual que puede alcanzar 160 000 IOPS con discos ultra, envíe un correo electrónico a UltraDiskFeedback@microsoft.com.