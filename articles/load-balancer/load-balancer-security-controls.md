---
title: Controles de seguridad para Azure Load Balancer
description: Lista de comprobación de los controles de seguridad para evaluar Load Balancer
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: e0be6635a0916183e1dfe776bef4c547578383dc
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886179"
---
# <a name="security-controls-for-azure-load-balancer"></a>Controles de seguridad para Azure Load Balancer

En este artículo se explican los controles de seguridad integrados en Azure Load Balancer.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D | |
| Compatibilidad con inserción de red virtual| N/D | |
| Compatibilidad con el aislamiento de red y los firewalls| N/D |  |
| Compatibilidad con la tunelización forzada| N/D | |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Registros de Azure Monitor para el equilibrador de carga básica público](load-balancer-monitor-log.md). |
| Registro y auditoría del plano de administración y de control| Sí | Consulte [Registros de Azure Monitor para el equilibrador de carga básica público](load-balancer-monitor-log.md). |
| Registro y auditoría del plano de datos | N/D |  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| N/D |  |
| Authorization| N/D |  |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | N/D | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| N/D | |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | N/D | |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de [Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| N/D |  | 

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).