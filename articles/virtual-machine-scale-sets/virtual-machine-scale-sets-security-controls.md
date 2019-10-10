---
title: Controles de seguridad para Azure Virtual Machine Scale Sets
description: Lista de comprobación de los controles de seguridad para evaluar Azure Virtual Machine Scale Sets
services: virtual-machine-scale-sets
ms.service: virtual-machine-scale-sets
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 7d7e69e8ad0c5b14ac7ed8b941a7949f4f675812
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176767"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Controles de seguridad para Azure Virtual Machine Scale Sets

En este artículo se explican los controles de seguridad integrados en Azure Virtual Machine Scale Sets.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |
| Compatibilidad con la tunelización forzada| Sí | Consulte [Configuración de la tunelización forzada mediante el modelo de implementación de Azure Resource Manager](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Supervisión y actualización de una máquina virtual Linux en Azure](/azure/virtual-machines/linux/tutorial-monitoring) y [Supervisión y actualización de una máquina virtual Windows en Azure](/azure/virtual-machines/windows/tutorial-monitoring). |
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos | Sin |  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí |  |
| Authorization| Sí |  |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | Consulte [Cifrado de una máquina virtual Linux en Azure](/azure/virtual-machines/linux/disk-encryption-linux) y [Cifrado de discos virtuales en una máquina virtual Windows](/azure/virtual-machines/windows/encrypt-disks). |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| Sí | Azure Virtual Machines admite [ExpressRoute](/azure/expressroute) y el cifrado de red virtual. Consulte [Cifrado en tránsito en máquinas virtuales](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí | Las claves administradas por el cliente es un escenario de cifrado admitidos de Azure; consulte [Introducción al cifrado de Azure](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de HTTPS y SSL. |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí |  | 

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).