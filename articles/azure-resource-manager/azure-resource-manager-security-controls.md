---
title: Controles de seguridad de Azure Resource Manager
description: Lista de comprobación de los controles de seguridad para evaluar Azure Resource Manager
services: azure-resource-manager
author: msmbaldwin
manager: rkarlin
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 97fd6611d7e2a2787b865365c4c7579f89f17d01
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886263"
---
# <a name="security-controls-for-azure-resource-manager"></a>Controles de seguridad de Azure Resource Manager

En este artículo, se documentan los controles de seguridad integrados en Azure Resource Manager.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | HTTPS/TLS. |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | N/D | Azure Resource Manager no almacena ningún contenido de cliente, solo los datos de control. |
| Cifrado de nivel de columna (Azure Data Services)| Sí | |
| Llamadas a API cifradas| Sí | |

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sin |  |
| Compatibilidad con la tunelización forzada| Sin |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sin | |
| Registro y auditoría del plano de administración y de control| Sí | Los registros de actividad exponen todas operaciones de escritura (PUT, POST, DELETE) realizadas en los recursos; consulte [Visualización de registros de actividad para supervisar acciones sobre recursos](resource-group-audit.md). |
| Registro y auditoría del plano de datos| N/D | |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Basado en [Azure Active Directory](/azure/active-directory).|
| Authorization| Sí | |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí |  |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).