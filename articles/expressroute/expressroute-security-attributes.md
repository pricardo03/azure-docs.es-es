---
title: Atributos de seguridad comunes de Azure ExpressRoute
description: Lista de comprobación de los atributos de seguridad que se utilizan habitualmente para evaluar Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: d6156715fb87831d465197fd8eec59d245221e48
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082213"
---
# <a name="common-security-attributes-for-azure-expressroute"></a>Atributos de seguridad comunes de Azure ExpressRoute

La seguridad está integrada en todos los aspectos de los servicios de Azure. En este artículo, se explican los atributos de seguridad comunes integrados en Azure ExpressRoute.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>|  N/D | ExpressRoute no almacena datos de clientes. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sin | |
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
| Autorización|  Sí |Cuenta de servicio de Gateway for Microsoft (GWM) (controlador); acceso Just-in-Time (JIT) para desarrollo y operaciones. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas| 
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos| Sin |   |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante el proveedor de recursos de red (NRP). |
