---
title: Atributos de seguridad de Azure VPN Gateway
description: Lista de comprobación de los atributos de seguridad para evaluar Azure VPN Gateway
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: c355f70975f441609304a4c9ee2ead75f0e0ce25
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444597"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Atributos de seguridad de Azure VPN Gateway

En este artículo, se explican los atributos de seguridad integrados en Azure VPN Gateway.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) | N/D | Datos de clientes en tránsito de VPN Gateway, NO se almacenan los datos de los clientes |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| Sí | Paquetes de clientes cifrados de VPN Gateway entre las instancias de Azure VPN Gateway y los clientes de VPN (P2S) o los dispositivos de VPN (S2S) locales de los clientes. Las instancias de Azure VPN Gateway también admiten el cifrado entre redes virtuales. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin | Las claves precompartidas especificadas por el cliente se cifran en reposo; pero aún no se integran con CMK. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D | |
| Compatibilidad con inserción de red virtual| N/D | . |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Las instancias de VPN Gateway son instancias de máquina virtual dedicadas para cada red virtual de cliente  |
| Compatibilidad con la tunelización forzada| Sí |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Registros de diagnósticos de Azure Monitor/alerta](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Métricas de Azure Monitor/alerta](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) para administrar el servicio y configurar Azure VPN Gateway. |
| Authorization| Sí | Admitir la autorización a través de [RBAC](../role-based-access-control/overview.md). |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Registro de actividad de Azure Resource Manager. |
| Registro y auditoría del plano de datos | Sí | [Registros de diagnóstico de Azure Monitor](../azure-resource-manager/resource-group-audit.md) para el registro y la auditoría de la conectividad de VPN. |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Para las operaciones de administración, el estado de una configuración de Azure VPN Gateway se puede exportar como una plantilla de Azure Resource Manager y con control de versiones a lo largo del tiempo. | 