---
title: Atributos comunes de seguridad para el almacenamiento de Azure
description: Una lista de comprobación de los atributos de seguridad comunes para evaluar el almacenamiento de Azure
services: storage
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: storage
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 8fa43998d0b10eddacdc9e0dd512295559814255
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62101972"
---
# <a name="common-security-attributes-for-azure-storage"></a>Atributos comunes de seguridad para el almacenamiento de Azure

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en Azure Storage. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí |  |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Admite los mecanismos de HTTPS/TLS estándar.  Los usuarios también pueden cifrar datos antes de transmitirse al servicio. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | Consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí |  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí |  |
| compatibilidad con inserción de redes virtuales| N/D |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | |
| Fuerza la tunelización de soporte técnico| N/D |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Métricas de Azure Monitor disponibles ahora, los registros de versión preliminar inicial |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory, clave compartida, el token de acceso compartido. |
| Autorización| Sí | Autorización de soporte técnico a través de RBAC, ACL de POSIX y Tokens de SAS |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración | Sí | Registro de actividad de Azure Resource Manager |
| Auditoría y registro del plano de datos| Sí | Los registros de diagnóstico de servicio y vista previa a partir de registro de Azure Monitor  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Permite controlar las versiones del proveedor de recursos a través de API de Azure Resource Manager |