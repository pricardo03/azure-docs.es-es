---
title: Atributos comunes de seguridad para copia de seguridad de Azure
description: Lista de comprobación de los atributos de seguridad que se utilizan habitualmente para evaluar Azure Backup
services: backup
author: msmbaldwin
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 86b8ab96a94a6ffc44c304d8a0a689301560a989
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002798"
---
# <a name="security-attributes-for-azure-backup"></a>Atributos de seguridad para copia de seguridad de Azure

En este artículo describe los atributos de seguridad integrados en Azure Backup. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| No | Se usa HTTPS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| No |  |
| Cifrado de nivel de columna (Azure Data Services)| No |  |
| Llamadas API cifradas| Sí |  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| No |  |
| Compatibilidad con inserción de redes virtuales| No |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | La tunelización forzada es compatible con copia de seguridad de VM. La tunelización forzada no se admite para las cargas de trabajo que se ejecutan en VM. |
| Fuerza la tunelización de soporte técnico| No |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Log Analytics se admite a través de los registros de diagnóstico. Consulte [protegido de copia de seguridad de Azure Monitor de cargas de trabajo con Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) para obtener más información. |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Autorización| Sí | Se usan roles RBAC integrados y creados por el cliente. Consulte [Control de acceso basado en rol para administrar puntos de recuperación de Azure Backup](/azure/backup/backup-rbac-rs-vault) para obtener más información. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Todas las acciones que desencadena el cliente desde Azure Portal se registran en los registros de actividad. |
| Auditoría y registro del plano de datos| No | No se puede acceder al plano de datos de Azure Backup directamente.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí|  |