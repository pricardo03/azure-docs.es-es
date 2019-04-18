---
title: Atributos comunes de seguridad para copia de seguridad de Azure
description: Lista de comprobación de los atributos de seguridad que se utilizan habitualmente para evaluar Azure Backup
services: backup
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 93dd5372bffb278894987cd3cc2b8322cbc5e577
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679694"
---
# <a name="common-security-attributes-for-azure-backup"></a>Atributos comunes de seguridad para copia de seguridad de Azure

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en Azure Backup. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sin  | Uso de HTTPS. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sin  |  |
| Cifrado de nivel de columna (Azure Data Services)| Sin  |  |
| Llamadas API cifradas| Sí |  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sin  |  |
| Compatibilidad con inserción de red virtual| Sin  |  |
| Compatibilidad con aislamiento de red y firewalls| Sí | La tunelización forzada es compatible con copia de seguridad de VM. La tunelización forzada no se admite para las cargas de trabajo que se ejecutan en VM. |
| Compatibilidad con la tunelización forzada | Sin  |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Log Analytics se admite a través de los registros de diagnóstico. Consulte [protegido de copia de seguridad de Azure Monitor de cargas de trabajo con Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) para obtener más información. |

## <a name="iam-support"></a>Compatibilidad con IAM

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Administración del acceso: autenticación| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Administración del acceso: autorización| Sí | Se usan roles RBAC integrados y creados por el cliente. Consulte [Control de acceso basado en rol para administrar puntos de recuperación de Azure Backup](/azure/backup/backup-rbac-rs-vault) para obtener más información. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración| Sí | Todas las acciones que desencadena el cliente desde Azure Portal se registran en los registros de actividad. |
| Registro y auditoría del plano de datos| Sin  | No se puede acceder al plano de datos de Azure Backup directamente.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí|  |