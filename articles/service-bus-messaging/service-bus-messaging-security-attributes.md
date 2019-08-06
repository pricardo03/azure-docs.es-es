---
title: Atributos de seguridad para la mensajería de Azure Service Bus
description: Lista de comprobación de los atributos de seguridad para evaluar la mensajería de Azure Service Bus
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 0e1d6e041b47a261b549fb8b608cf09d0d6362dd
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443891"
---
# <a name="security-attributes-for-azure-service-bus-messaging"></a>Atributos de seguridad para la mensajería de Azure Service Bus

En este artículo, se explican los atributos de seguridad integrados en la mensajería de Azure Service Bus.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)|  Sí para el cifrado en reposo del lado servidor de forma predeterminada. | No se admiten todavía claves administradas por el cliente y BYOK. El cifrado del lado cliente es responsabilidad del cliente. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en red virtual y cifrado de red virtual a red virtual)| Sí | Es compatible con el mecanismo estándar de HTTPS/TLS. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sin |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de API se hacen a través de [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí (nivel Premium solo) | Se admiten los puntos de conexión de servicio de red virtual solo para el [nivel Premium de Service Bus](service-bus-premium-messaging.md). |
| Compatibilidad con inserción de red virtual| Sin | |
| Compatibilidad con aislamiento de red y firewalls| Sí (nivel Premium solo) |  |
| Compatibilidad con la tunelización forzada| Sin |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Compatible a través de [Azure Monitor y alertas](service-bus-metrics-azure-monitor.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Administrada a través de [Azure Active Directory Managed Service Identity](service-bus-managed-service-identity.md); vea [Autenticación y autorización de Service Bus](service-bus-authentication-and-authorization.md).|
| Authorization| Sí | Admite la autorización a través de [RBAC](service-bus-role-based-access-control.md) (versión preliminar) y el token de SAS; vea [Autenticación y autorización de Service Bus](service-bus-authentication-and-authorization.md). |



## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Están disponibles los registros de operaciones; consulte [Registros de diagnóstico de Service Bus](service-bus-diagnostic-logs.md).  |
| Registro y auditoría del plano de datos| Sin |  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Es compatible con versiones del proveedor de recursos a través de la [API de Azure Resource Manager](/rest/api/resources/).|
