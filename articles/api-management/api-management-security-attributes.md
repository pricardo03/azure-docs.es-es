---
title: Atributos comunes de seguridad de Azure API Management
description: Una lista de comprobación de los atributos de seguridad comunes para evaluar la administración de API
services: api-management
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 040d628f9fe89f68a1c5ab91a1522c6a3cb724d9
ms.sourcegitcommit: e729629331ae10097a081a03029398525f4147a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64508149"
---
# <a name="common-security-attributes-for-api-management"></a>Atributos comunes de seguridad para la administración de API

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en API Management.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí (solo para el cifrado del servicio) | Los datos confidenciales, como certificados, claves y valores con nombre secreto se cifran con administradas del servicio, por las claves de la instancia de servicio. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | [Expressroute](../expressroute/index.yml) y proporciona el cifrado de red virtual por [redes de Azure](../virtual-network/index.yml). |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sin  | Todas las claves de cifrado son por instancia de servicio y servicio administrado. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de plano de administración se realizan a través de [Azure Resource Manager](../azure-resource-manager/index.yml) a través de TLS. Se requiere un token web de JSON (JWT) válido.  Las llamadas de plano de datos pueden estar protegidas con TLS y uno de los mecanismos de autenticación admitidos (por ejemplo, el certificado de cliente o JWT).
 |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sin  | |
| Compatibilidad con inserción de redes virtuales| Sí | |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Uso de grupos de seguridad de red (NSG) y Azure Application Gateway (u otro dispositivo de software), respectivamente. |
| Fuerza la tunelización de soporte técnico| Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Autorización| Sí | |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | [Registros de actividad de Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Auditoría y registro del plano de datos| Sí | [Registros de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) y (opcionalmente) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Mediante el [Kit de recursos de DevOps de Azure API Management](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>Exámenes de vulnerabilidades de falsos positivos

Esta sección documentan las vulnerabilidades más comunes, que no afectan a la administración de API de Azure.

| Punto vulnerable               | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed es una vulnerabilidad en la implementación de la extensión TLS SessionTicket que se encuentra en algunos productos de F5. Permite la pérdida ("hemorragia") de hasta 31 bytes de datos de la memoria sin inicializar. Esto se debe a la pila TLS relleno de un identificador de sesión que se pasa desde el cliente, con los datos para que sea de 32 bits más larga. |