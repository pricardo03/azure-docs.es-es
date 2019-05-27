---
title: Atributos comunes de seguridad de Azure Resource Manager
description: Una lista de comprobación de los atributos de seguridad comunes para evaluar Azure Resource Manager
services: azure-resource-manager
author: msmbaldwin
manager: barbkess
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mbaldwin
ms.openlocfilehash: a771d4c2ae22b7bf149c13c80fe5286ef52a4545
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002254"
---
# <a name="security-attributes-for-azure-resource-manager"></a>Atributos de seguridad de Azure Resource Manager

En este artículo describe los atributos de seguridad integrados en Azure Resource Manager.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí |  |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| N/D | Azure Resource Manager no almacena ningún contenido de cliente, solo los datos de control. |
| Cifrado de nivel de columna (Azure Data Services)| Sí | |
| Llamadas a API cifradas| Sí | |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| No | |
| Compatibilidad con inserción de redes virtuales| Sí | |
| Aislamiento de red y la compatibilidad con Firewall| No |  |
| Fuerza la tunelización de soporte técnico| No |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| No | |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | [Azure Active Directory](/azure/active-directory) basado.|
| Autorización| Sí | |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Registros de actividad exponer todas operaciones de escritura (PUT, POST, DELETE) realizadas en los recursos; consulte [ver registros de actividad para auditar las acciones sobre los recursos](resource-group-audit.md). |
| Auditoría y registro del plano de datos| N/D | |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí |  |
