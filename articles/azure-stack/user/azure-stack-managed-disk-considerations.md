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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 05efa7eaf6d95cbf63efd17b00d321d8c8509f28
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246786"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Managed Disks de Azure Stack: diferencias y consideraciones

En este artículo se resumen las diferencias conocidas entre [Managed Disks de Azure Stack](azure-stack-manage-vm-disks.md) y [Managed Disks para Azure](../../virtual-machines/windows/managed-disks-overview.md). Para obtener información acerca de las diferencias de alto nivel entre Azure y Azure Stack, consulte el artículo [Key considerations](azure-stack-considerations.md) (Consideraciones clave).

Managed Disks simplifica la administración de discos para las máquinas virtuales de IaaS, ya que administra las [cuentas de almacenamiento](../azure-stack-manage-storage-accounts.md) asociadas a los discos de la máquina virtual.

> [!Note]  
> Managed Disks en Azure Stack está disponible desde la actualización 1808. Está habilitado de forma predeterminada al crear máquinas virtuales mediante el portal de Azure Stack a partir de la actualización 1811.
  

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
|Tamaño del disco  |Disco Premium de Azure: P4 (32 GiB) a P80 (32 TiB)<br>Disco SSD estándar de Azure: E10 (128 GiB) a E80 (32 TiB)<br>Disco HDD estándar de Azure: S4 (32 GiB) a S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GB<br>M15: 256 GiB<br>M20: 512 GB<br>M30: 1024 GiB |
|Copia de instantáneas de discos|Discos administrados de Azure de instantáneas conectados a una máquina virtual en ejecución compatible|Todavía no se admite |
|Análisis de rendimiento de discos |Compatibilidad con métricas agregadas y por disco |Todavía no se admite |
|Migración      |Proporciona herramientas para migrar desde máquinas virtuales de Azure Resource Manager no administradas existentes sin necesidad de volver a crear la máquina virtual  |Todavía no se admite |

> [!NOTE]  
> Las E/S por segundo y el rendimiento de Managed Disks en Azure Stack es un número extremo, en lugar de un número aprovisionado, que puede resultar afectado por el hardware y las cargas de trabajo que se ejecutan en Azure Stack.

## <a name="metrics"></a>Métricas

También hay diferencias en las métricas de almacenamiento:

- Con Azure Stack, los datos de transacción de las métricas de almacenamiento no distinguen el ancho de banda de red interna ni externa.
- Los datos de transacción de Azure Stack en las métricas de almacenamiento no incluyen el acceso de la máquina virtual a los discos montados.

## <a name="api-versions"></a>Versiones de API

Managed Disks de Azure Stack admite las versiones de API siguientes:

- 2017-03-30

## <a name="configuration"></a>Configuración

Después de aplicar la actualización 1808 o posterior, debe realizar la siguiente configuración antes de usar Managed Disks:

- Si una suscripción se ha creado antes de la actualización 1808, siga estos pasos para actualizar la suscripción. En caso contrario, la implementación de VM en esta suscripción podría producir un error con un mensaje de error "Error interno en el administrador de discos".
   1. En el portal del inquilino, vaya a **Suscripciones** y busque la suscripción. Haga clic en **Proveedores de recursos**, después en **Microsoft.Compute** y luego en **Volver a registrar**.
   2. En la misma suscripción, vaya a **Control de acceso (IAM)**, y compruebe que **Azure Stack – Managed Disk** (Azure Stack - Disco administrado) aparece en la lista.
- Si usa un entorno de varios inquilinos, pida a su operador de nube (puede ser de su propia organización o del proveedor de servicios) que vuelva a configurar cada uno de los directorios de invitado siguiendo los pasos en [este artículo](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory). En caso contrario, la implementación de VM en una suscripción asociada con ese directorio de invitado podría producir un error con un mensaje de error "Error interno en el administrador de discos".


## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre máquinas virtuales de Azure Stack](azure-stack-compute-overview.md)
