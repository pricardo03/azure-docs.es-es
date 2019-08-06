---
title: Atributos de seguridad de Azure Storage
description: Lista de comprobación de los atributos de seguridad para evaluar Azure Storage
services: storage
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: storage
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 16ec2757955b53a8bfa73ba724100f7fa61d2867
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444297"
---
# <a name="security-attributes-for-azure-storage"></a>Atributos de seguridad de Azure Storage

En este artículo, se explican los atributos de seguridad integrados en Azure Storage. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)| Sí |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en red virtual y cifrado de red virtual a red virtual)| Sí | Compatible con los mecanismos estándar de HTTPS/TLS.  Los usuarios también pueden cifrar datos antes de que se transmitan al servicio. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sí | Consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí |  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con los puntos de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| N/D |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | |
| Compatibilidad con la tunelización forzada| N/D |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Las métricas de Azure Monitor ya están disponibles ahora, los registros empiezan la versión preliminar |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory, clave compartida, el token de acceso compartido. |
| Authorization| Sí | Compatible con la autorización mediante RBAC, listas de control de acceso de POSIX y tokens de SAS |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control | Sí | Registro de actividad de Azure Resource Manager |
| Registro y auditoría del plano de datos| Sí | Los registros de diagnóstico del servicio y el registro de Azure Monitor empiezan la versión preliminar  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Compatible con el control de versiones del proveedor de recursos mediante las API de Azure Resource Manager |