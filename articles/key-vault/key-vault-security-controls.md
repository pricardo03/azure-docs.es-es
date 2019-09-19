---
title: Controles de seguridad para Azure Key Vault
description: Lista de comprobación de los controles de seguridad para evaluar Azure Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: d7f92a309a3d0b5d8e85f1e270d5590f46496a77
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886241"
---
# <a name="security-controls-for-azure-key-vault"></a>Controles de seguridad para Azure Key Vault

En este artículo se explican los controles de seguridad integrados en Azure Key Vault. 

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | Se usan puntos de conexión de servicio de red virtual (VNet). |
| Compatibilidad con la inserción de redes virtuales| Sin |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Se usan reglas de firewall de red virtual. |
| Compatibilidad con la tunelización forzada| Sin |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Se usa Log Analytics. |
| Registro y auditoría de plano de administración y de control| Sí | Se usa Log Analytics. |
| Registro y auditoría del plano de datos| Sí | Se usa Log Analytics. |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Se usa la directiva de acceso de Key Vault. |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | Se cifran todos los objetos. |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí | El cliente controla todas las claves de su almacén de claves. Cuando se especifican claves con el respaldo del módulo de seguridad de hardware (HSM), un HSM de nivel 2 de FIPS protege la clave, el certificado o el secreto. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Toda la comunicación se realiza a través de llamadas a API cifradas |
| Llamadas a API cifradas| Sí | Se usa HTTPS. |

## <a name="access-controls"></a>Controles de acceso

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Controles de acceso del plano de administración y de control | Sí | Control de acceso basado en rol (RBAC) de Azure Resource Manager |
| Controles de acceso del plano de datos (en cada nivel de servicio) | Sí | Directiva de acceso de Key Vault |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).