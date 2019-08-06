---
title: Atributos de seguridad para Azure Service Bus Relay
description: Lista de comprobación de los atributos de seguridad para evaluar la retransmisión de Azure Service Bus
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 2047f64914d4a286e6de38b7b2c8524d98eba562
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443881"
---
# <a name="security-attributes-for-azure-service-bus-relay"></a>Atributos de seguridad para Azure Service Bus Relay

En este artículo, se explican los atributos de seguridad integrados en la mensajería de Azure Service Bus Relay.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado)|  N/D | La retransmisión es un socket web y no conserva los datos. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | El servicio requiere TLS. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sin | Usa solo los certificados TLS de Microsoft.  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sin |  |
| Compatibilidad con aislamiento de red y firewalls| Sin |  |
| Compatibilidad con la tunelización forzada| N/D | La retransmisión es el túnel de TLS  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | A través de SAS. |
| Authorization|  Sí | A través de SAS. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Registro y auditoría del plano de datos| Sí | Conexión correcta o con errores y errores y registros.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml).|
