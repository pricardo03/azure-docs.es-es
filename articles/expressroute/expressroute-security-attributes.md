---
title: Atributos de seguridad de Azure ExpressRoute
description: Lista de comprobación de los atributos de seguridad que se utilizan para evaluar Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: c9a46497c18b99ad7774036fd92e63d024b47045
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442196"
---
# <a name="security-attributes-for-azure-expressroute"></a>Atributos de seguridad de Azure ExpressRoute

En este artículo, se explican los atributos de seguridad integrados en Azure ExpressRoute.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado)|  N/D | ExpressRoute no almacena datos de clientes. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sin | |
| Control de claves de cifrado (CMK, BYOK, etc.)| N/D |  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D |  |
| Compatibilidad con inserción de red virtual| N/D | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Cada cliente está contenido en su propio dominio de enrutamiento y tunelizado a su propia red virtual |
| Compatibilidad con la tunelización forzada| N/D | Mediante el Protocolo de puerta de enlace de borde (BGP). |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Supervisión, métricas y alertas de ExpressRoute ](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Cuenta de servicio de Gateway for Microsoft (GWM) (controlador); acceso Just-in-Time (JIT) para desarrollo y operaciones. |
| Authorization|  Sí |Cuenta de servicio de Gateway for Microsoft (GWM) (controlador); acceso Just-in-Time (JIT) para desarrollo y operaciones. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas| 
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos| Sin |   |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante el proveedor de recursos de red (NRP). |
