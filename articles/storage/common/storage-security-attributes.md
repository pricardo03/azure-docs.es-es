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
ms.openlocfilehash: 33765d57f1a0e5788011b2d9d9c2f57d06713ddb
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680696"
---
# <a name="common-security-attributes-for-azure-storage"></a>Atributos comunes de seguridad para el almacenamiento de Azure

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en Azure Storage. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí |  |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Admite los mecanismos de HTTPS/TLS estándar.  Los usuarios también pueden cifrar datos antes de transmitirse al servicio. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sí | Consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí |  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de red virtual| N/D |  |
| Compatibilidad con aislamiento de red y firewalls| Sí | |
| Compatibilidad con tunelización forzada | N/D |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Métricas de Azure Monitor disponibles ahora, los registros de versión preliminar inicial |

## <a name="iam-support"></a>Compatibilidad con IAM

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Administración del acceso: autenticación| Sí | Azure Active Directory, clave compartida, el token de acceso compartido. |
| Administración del acceso: autorización| Sí | Autorización de soporte técnico a través de RBAC, ACL de POSIX y Tokens de SAS |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración | Sí | Registro de actividad de Azure Resource Manager |
| Registro y auditoría del plano de datos| Sí | Los registros de diagnóstico de servicio y vista previa a partir de registro de Azure Monitor  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Permite controlar las versiones del proveedor de recursos a través de API de Azure Resource Manager |