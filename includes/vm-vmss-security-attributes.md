---
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 520fd50b2b0864f43c08687f05de377679b36d84
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886945"
---
## <a name="preventative"></a>Prevención

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Consulte [Cifrado de una máquina virtual Linux en Azure](/azure/virtual-machines/linux/encrypt-disks) y [Cifrado de discos virtuales en una máquina virtual Windows](/azure/virtual-machines/windows/encrypt-disks). |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| Sí | Azure Virtual Machines admite [ExpressRoute](/azure/expressroute) y el cifrado de red virtual. Consulte [Cifrado en tránsito en máquinas virtuales](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sí | Las claves administradas por el cliente es un escenario de cifrado admitidos de Azure; consulte [Introducción al cifrado de Azure](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de HTTPS y SSL. |

## <a name="network-segmentation"></a>Segmentación de red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | |
| Compatibilidad con la inserción de redes virtuales| Sí | . |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |
| Compatibilidad con la tunelización forzada| Sí | Consulte [Configuración de la tunelización forzada mediante el modelo de implementación de Azure Resource Manager](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="detection"></a>Detección

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Supervisión y actualización de una máquina virtual Linux en Azure](/azure/virtual-machines/linux/tutorial-monitoring) y [Supervisión y actualización de una máquina virtual Windows en Azure](/azure/virtual-machines/windows/tutorial-monitoring). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí |  |
| Authorization| Sí |  |


## <a name="audit-trail"></a>Pista de auditoría

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos | Sin |  |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí |  | 
