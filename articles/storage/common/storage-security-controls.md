---
title: Controles de seguridad para los servicios de Azure Storage
description: Lista de comprobación de controles de seguridad para evaluar Azure Storage
services: storage
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.service: storage
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 2cc54077456fce1e7e0f47843a762beee8e715f7
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526763"
---
# <a name="security-controls-for-azure-storage"></a>Controles de seguridad para los servicios de Azure Storage

En este artículo, se explican los controles de seguridad integrados en Azure Storage. 

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí |  |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí | Consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Compatible con los mecanismos estándar de HTTPS/TLS.  Los usuarios también pueden cifrar datos antes de que se transmitan al servicio. |
| Llamadas a API cifradas| Sí |  |

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| N/D |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | |
| Compatibilidad con la tunelización forzada| N/D |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Métricas de Azure Monitor|
| Registro y auditoría del plano de administración y de control | Sí | Registro de actividad de Azure Resource Manager |
| Registro y auditoría del plano de datos| Sí | Registros de diagnóstico de servicio.|

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory, clave compartida, el token de acceso compartido. |
| Authorization| Sí | Compatible con la autorización mediante RBAC, listas de control de acceso de POSIX y tokens de SAS |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Compatible con el control de versiones del proveedor de recursos mediante las API de Azure Resource Manager |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../../security/fundamentals/security-controls.md).