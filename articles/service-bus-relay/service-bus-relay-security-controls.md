---
title: Controles de seguridad para Azure Service Bus Relay
description: Lista de comprobación de los controles de seguridad para evaluar Azure Service Bus Relay
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: 7f4e2a31673905a7e28d1dbb5520650aefc6f368
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219989"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Controles de seguridad para Azure Service Bus Relay

En este artículo, se explican los controles de seguridad integrados en la mensajería de Azure Service Bus Relay.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Compatibilidad con punto de conexión de servicio| Sin |  |   |
| Compatibilidad con el aislamiento de red y los firewalls| Sin |  |   |
| Compatibilidad con la tunelización forzada| N/D | La retransmisión es el túnel de TLS  |   |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |   |
| Registro y auditoría del plano de administración y de control| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml). |   |
| Registro y auditoría del plano de datos| Sí | Conexión correcta o con errores, y errores y registros.  |   |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Authentication| Sí | Mediante SAS. | [Autenticación y autorización en Azure Relay](relay-authentication-and-authorization.md) |
| Authorization|  Sí | Mediante SAS. | [Autenticación y autorización en Azure Relay](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft |  N/D | La retransmisión es un socket web y no conserva los datos. |   |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sin | Usa solo los certificados TLS de Microsoft.  |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |   |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | El servicio requiere TLS. |   |
| Llamadas a API cifradas| Sí | HTTPS. |


## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml).|   |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).