---
author: msmbaldwin
ms.service: backup
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 5a4cc3ea822c5613fc7a50b8e370d6846b683721
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513300"
---
## <a name="preventative"></a>Preventivo

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sin  | Uso de HTTPS. |
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
| Compatibilidad con supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Log Analytics se admite a través de los registros de diagnóstico. Para más información, consulte Monitor Azure Backup protected workloads using Log Analytics (Supervisión de cargas de trabajo protegidas por Azure Backup con Log Analytics, https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |

## <a name="iam-support"></a>Compatibilidad con IAM

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Administración del acceso: autenticación| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Administración del acceso: autorización| Sí | Se usan roles RBAC integrados y creados por el cliente. Para más información, consulte Uso del control de acceso basado en roles para administrar puntos de recuperación de Azure Backup (https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault). |


## <a name="audit-trail"></a>Registro de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración| Sí | Todas las acciones que desencadena el cliente desde Azure Portal se registran en los registros de actividad. |
| Registro y auditoría del plano de datos| Sin  | No se puede acceder al plano de datos de Azure Backup directamente.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí|  |