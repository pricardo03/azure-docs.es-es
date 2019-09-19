---
title: Controles de seguridad para Azure Service Bus Relay
description: Lista de comprobación de los controles de seguridad para evaluar Azure Service Bus Relay
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: a131313f6c270debd9509a934fbf3ce74918ed42
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886276"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Controles de seguridad para Azure Service Bus Relay

En este artículo, se explican los controles de seguridad integrados en la mensajería de Azure Service Bus Relay.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sin |  |
| Compatibilidad con la tunelización forzada| N/D | La retransmisión es el túnel de TLS  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |
| Registro y auditoría del plano de administración y de control| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Registro y auditoría del plano de datos| Sí | Conexión correcta o con errores, y errores y registros.  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Mediante SAS. |
| Authorization|  Sí | Mediante SAS. |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft |  N/D | La retransmisión es un socket web y no conserva los datos. |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sin | Usa solo los certificados TLS de Microsoft.  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | El servicio requiere TLS. |
| Llamadas a API cifradas| Sí | HTTPS. |


## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml).|

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).