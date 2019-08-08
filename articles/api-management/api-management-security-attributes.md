---
title: Atributos de seguridad de Azure API Management
description: Lista de comprobación de los atributos de seguridad que se utilizan para evaluar API Management
services: api-management
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 2049e2349e3a25ebd4d3f4db19ec47bbaeb067de
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442273"
---
# <a name="security-attributes-for-api-management"></a>Atributos de seguridad de API Management

En este artículo, se explican los atributos de seguridad integrados en API Management.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)| Sí (solo cifrado del servicio) | Los datos confidenciales, como certificados, claves y valores con nombre secreto, se cifran con claves que administra el servicio por instancia de servicio. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | El cifrado de [ExpressRoute](../expressroute/index.yml) y de red virtual lo proporcionan las [redes de Azure](../virtual-network/index.yml). |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin | Todas las claves de cifrado son por instancia de servicio y las administra el servicio. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas del plano de administración se hacen mediante [Azure Resource Manager](../azure-resource-manager/index.yml) con TLS. Se requiere un token web de JSON (JWT) válido.  Las llamadas del plano de datos pueden estar protegidas con TLS y uno de los mecanismos de autenticación admitidos (por ejemplo, el certificado de cliente o JWT).
 |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Se usan grupos de seguridad de red (NSG) y Azure Application Gateway (u otro dispositivo de software), respectivamente. |
| Compatibilidad con la tunelización forzada| Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Authorization| Sí | |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | [Registros de actividad en Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Registro y auditoría del plano de datos| Sí | [Registros de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) y (opcionalmente) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Se usa el [kit de recursos de DevOps de Azure API Management](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>Exámenes de vulnerabilidades de falsos positivos

Esta sección documenta las vulnerabilidades más comunes, que no afectan a Azure API Management.

| Punto vulnerable               | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed es una vulnerabilidad en la implementación de la extensión TLS SessionTicket que se encuentra en algunos productos de F5. Permite la pérdida ("hemorragia") de hasta 31 bytes de datos de la memoria sin inicializar. Esto se debe a que la pila de TLS rellena con datos un identificador de sesión, que pasa el cliente, para que tenga una longitud de 32 bits. |
