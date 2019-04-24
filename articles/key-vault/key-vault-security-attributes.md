---
title: Atributos comunes de seguridad para Azure Key Vault
description: Lista de comprobación de los atributos de seguridad que se utilizan habitualmente para evaluar Azure Key Vault
services: key-vault
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 3ccfc38136ba3e8ec7c6130658032b7565988e5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461419"
---
# <a name="common-security-attributes-for-azure-key-vault"></a>Atributos comunes de seguridad para Azure Key Vault

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en Azure Key Vault. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Se cifran todos los objetos. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Toda la comunicación se realiza a través de llamadas a API cifradas |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | El cliente controla todas las claves en su almacén de claves. Cuando se especifican las claves de módulo (HSM) copiada de seguridad de hardware, un HSM de FIPS nivel 2 protege la clave, el certificado o el secreto. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Se usa HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí | Se usan puntos de conexión de servicio de Virtual Network (VNET). |
| compatibilidad con inserción de redes virtuales| Sin  |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Se usan reglas de firewall de red virtual. |
| Fuerza la tunelización de soporte técnico| Sin  |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Se usa Log Analytics. |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Autorización| Sí | Se usa la directiva de acceso de Key Vault. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de plano de administración y de control| Sí | Se usa Log Analytics. |
| Auditoría y registro del plano de datos| Sí | Se usa Log Analytics. |

## <a name="access-controls"></a>Controles de acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Controles de acceso del plano de administración y de control | Sí | Control de acceso basado en rol (RBAC) de Azure Resource Manager |
| Controles de acceso del plano de datos (en cada nivel de servicio) | Sí | Directiva de acceso de Key Vault |