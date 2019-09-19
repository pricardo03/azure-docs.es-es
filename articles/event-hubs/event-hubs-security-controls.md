---
title: Controles de seguridad para Azure Event Hubs
description: Lista de comprobación de los controles de seguridad para evaluar Azure Event Hubs
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 3f02315012840b20dc4aa8639ade954f23a5526e
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886233"
---
# <a name="security-controls-for-azure-event-hubs"></a>Controles de seguridad para Azure Event Hubs

En este artículo se explican los controles de seguridad integrados en Azure Event Hubs.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| Sin | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |
| Compatibilidad con la tunelización forzada| Sin |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos| Sí |   |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Authorization|  Sí | |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft |  Sí | |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | |
| Llamadas a API cifradas| Sí |  |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).
