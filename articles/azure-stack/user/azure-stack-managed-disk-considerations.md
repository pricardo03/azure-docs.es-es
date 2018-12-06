---
title: Diferencias y consideraciones para Managed Disks en Azure Stack | Microsoft Docs
description: Obtenga información sobre las diferencias y consideraciones al trabajar con Managed Disks en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 4bd36744cc417e85f49e58f9a08d2b9006da9fe4
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284036"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Managed Disks de Azure Stack: diferencias y consideraciones
En este artículo se resumen las diferencias conocidas entre Managed Disks de Azure Stack y Managed Disks para Azure. Para obtener información acerca de las diferencias de alto nivel entre Azure y Azure Stack, consulte el artículo [Key considerations](azure-stack-considerations.md) (Consideraciones clave).

Managed Disks simplifica la administración de discos para las máquinas virtuales de IaaS, ya que administra las [cuentas de almacenamiento](/azure/azure-stack/azure-stack-manage-storage-accounts) asociadas a los discos de la máquina virtual.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Hoja de referencia rápida: diferencias de Managed Disks

| Característica | Azure (global) | Azure Stack |
| --- | --- | --- |
|Cifrado de datos en reposo |Storage Service Encryption (SSE) de Azure, Azure Disk Encryption (ADE)     |Cifrado AES de 128 bits de BitLocker      |
|Imagen          | Compatibilidad con la imagen personalizada administrada |Todavía no se admite|
|Opciones de copia de seguridad |Compatibilidad con el servicio Azure Backup |Todavía no se admite |
|Opciones de recuperación ante desastres |Compatibilidad con Azure Site Recovery |Todavía no se admite|
|Tipos de disco     |SSD Premium, SSD estándar (versión preliminar) y HDD estándar |SSD Premium, HDD estándar |
|Discos premium  |Totalmente compatible |Se pueden aprovisionar, pero no hay límite de rendimiento o garantía  |
|E/S por segundo de discos premium  |Depende del tamaño del disco  |2300 E/S por segundo por disco |
|Rendimiento de discos premium |Depende del tamaño del disco |145 MB/segundo por disco |
|Tamaño del disco  |Disco Premium de Azure: P4 (32 GiB) a P80 (32 TiB)<br>Disco SSD estándar de Azure: E10 (128 GiB) a E80 (32 TiB)<br>Disco HDD estándar de Azure: S4 (32 GiB) a S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1024 GiB |
|Análisis de rendimiento de discos |Compatibilidad con métricas agregadas y por disco |Todavía no se admite |
|Migración      |Proporciona herramientas para migrar desde máquinas virtuales de Azure Resource Manager no administradas existentes sin necesidad de volver a crear la máquina virtual  |Todavía no se admite |


## <a name="metrics"></a>Métricas
También hay diferencias en las métricas de almacenamiento:
- Con Azure Stack, los datos de transacción de las métricas de almacenamiento no distinguen el ancho de banda de red interna o externa.
- Los datos de transacción de Azure Stack en las métricas de almacenamiento no incluyen el acceso de la máquina virtual a los discos montados.


## <a name="api-versions"></a>Versiones de API
Managed Disks de Azure Stack admite las versiones de API siguientes:
- 2017-03-30


## <a name="next-steps"></a>Pasos siguientes
[Más información sobre máquinas virtuales de Azure Stack](azure-stack-compute-overview.md)
