---
title: Atributos de seguridad de Azure Backup
description: Lista de comprobación de los atributos de seguridad para evaluar Azure Backup
ms.reviewer: mbaldwin
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: dacurwin
ms.openlocfilehash: e6ea701840b201c2794d81be5019d57ca531cd00
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688504"
---
# <a name="security-attributes-for-azure-backup"></a>Atributos de seguridad de Azure Backup

En este artículo, se explican los atributos de seguridad integrados en Azure Backup. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sin | Se usa HTTPS. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| Sin |  |
| Llamadas API cifradas| Sí |  |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin |  |
| Compatibilidad con la inserción de redes virtuales| Sin |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | La tunelización forzada es compatible con copia de seguridad de VM. La tunelización forzada no se admite para las cargas de trabajo que se ejecutan en VM. |
| Compatibilidad con la tunelización forzada| Sin |  |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Log Analytics se admite a través de los registros de diagnóstico. Para más información, consulte [Monitor Azure Backup protected workloads using Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) (Supervisión de cargas de trabajo protegidas por Azure Backup con Log Analytics). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Se usan roles RBAC integrados y creados por el cliente. Para más información, consulte [Uso del control de acceso basado en roles para administrar puntos de recuperación de Azure Backup](/azure/backup/backup-rbac-rs-vault). |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Todas las acciones que desencadena el cliente desde Azure Portal se registran en los registros de actividad. |
| Registro y auditoría del plano de datos| Sin | No se puede acceder al plano de datos de Azure Backup directamente.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí|  |