---
title: Atributos de seguridad para la mensajería de Azure Service Bus
description: Lista de comprobación de los atributos de seguridad para evaluar la mensajería de Azure Service Bus
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: axisc
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: aschhab
ms.openlocfilehash: 875754282f9e7a2b8dd92b0eb2d7e277ac842696
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70013105"
---
# <a name="security-attributes-for-azure-service-bus-messaging"></a>Atributos de seguridad para la mensajería de Azure Service Bus

En este artículo, se explican los atributos de seguridad integrados en la mensajería de Azure Service Bus.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)|  Sí, para el cifrado en reposo en el servidor, de forma predeterminada. | No se admiten todavía claves administradas por el cliente ni BYOK. El cifrado del lado del cliente es responsabilidad de este. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en red virtual y cifrado de red virtual a red virtual)| Sí | Compatible con el mecanismo estándar de HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de API se realizan mediante [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con los puntos de conexión de servicio| Sí (solo el nivel Premium) | Se admiten los puntos de conexión de servicio de red virtual solo para el [nivel Premium de Service Bus](service-bus-premium-messaging.md). |
| Compatibilidad con la inserción de redes virtuales| Sin | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí (solo el nivel Premium) |  |
| Compatibilidad con la tunelización forzada| Sin |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Compatible mediante [Azure Monitor y las alertas](service-bus-metrics-azure-monitor.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Administrada mediante [Managed Service Identity de Azure Active Directory](service-bus-managed-service-identity.md); consulte [Autenticación y autorización de Service Bus](service-bus-authentication-and-authorization.md).|
| Authorization| Sí | Admite la autorización mediante [RBAC](authenticate-application.md) y el token de SAS; consulte [Autenticación y autorización de Service Bus](service-bus-authentication-and-authorization.md). |



## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Están disponibles los registros de las operaciones; consulte [Registros de diagnóstico de Service Bus](service-bus-diagnostic-logs.md).  |
| Registro y auditoría del plano de datos| Sin |  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Es compatible con versiones del proveedor de recursos a través de la [API de Azure Resource Manager](/rest/api/resources/).|
