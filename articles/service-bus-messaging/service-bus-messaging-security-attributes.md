---
title: Atributos comunes de seguridad para la mensajería de Azure Service Bus
description: Una lista de comprobación de los atributos de seguridad comunes para evaluar la mensajería de Azure Service Bus
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 11977a5aa36b023e468ce6a54862b5138995c417
ms.sourcegitcommit: e729629331ae10097a081a03029398525f4147a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64513513"
---
# <a name="common-security-attributes-for-azure-service-bus-messaging"></a>Atributos comunes de seguridad para la mensajería de Azure Service Bus

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en mensajería de Azure Service Bus.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>|  Sí para el cifrado en reposo de lado del servidor de forma predeterminada. | No se admiten claves administradas por el cliente y BYOK todavía. Cifrado del lado cliente es responsabilidad del cliente |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Es compatible con el mecanismo estándar de HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sin  |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de API se realizan a través de [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí (solo en el nivel Premium) | Se admiten los puntos de conexión de servicio de red virtual para [nivel Premium de Service Bus](service-bus-premium-messaging.md) solo. |
| Compatibilidad con inserción de redes virtuales| Sin  | |
| Aislamiento de red y la compatibilidad con Firewall| Sí (solo en el nivel Premium) |  |
| Fuerza la tunelización de soporte técnico| Sin  |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Compatible a través de [Azure Monitor y alertas](service-bus-metrics-azure-monitor.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Administrar a través de [Azure Active Directory Managed Service Identity](service-bus-managed-service-identity.md); vea [autorización y autenticación de Service Bus](service-bus-authentication-and-authorization.md).|
| Autorización| Sí | Admite la autorización a través de [RBAC](service-bus-role-based-access-control.md) (versión preliminar) y el token de SAS; vea [autorización y autenticación de Service Bus](service-bus-authentication-and-authorization.md). |



## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Registros de operaciones están disponibles; consulte [los registros de diagnóstico de Service Bus](service-bus-diagnostic-logs.md).  |
| Auditoría y registro del plano de datos| Sin  |  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Es compatible con versiones del proveedor de recursos a través de la [API de Azure Resource Manager](/rest/api/resources/).|
