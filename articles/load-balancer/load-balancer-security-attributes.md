---
title: Atributos de seguridad de Azure Load Balancer
description: Una lista de comprobación de los atributos de seguridad para evaluar Load Balancer
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 1892b918ed35221bc73d5070d5a73ecc359aa4c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799861"
---
# <a name="security-attributes-for-azure-load-balancer"></a>Atributos de seguridad de Azure Load Balancer

En este artículo, se explican los atributos de seguridad comunes integrados en Azure Load Balancer.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) | N/D | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| N/D | |
| Control de clave de cifrado (CMK, BYOK, etc.)| N/D | |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de [Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D | |
| Compatibilidad con inserción de red virtual| N/D | . |
| Compatibilidad con el aislamiento de red y los firewalls| N/D |  |
| Compatibilidad con la tunelización forzada| N/D | |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Registros de Azure Monitor para el equilibrador de carga básica público](load-balancer-monitor-log.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| N/D |  |
| Authorization| N/D |  |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de plano de administración y de control| Sí | Consulte [Registros de Azure Monitor para el equilibrador de carga básica público](load-balancer-monitor-log.md). |
| Registro y auditoría del plano de datos | N/D |  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| N/D |  | 
