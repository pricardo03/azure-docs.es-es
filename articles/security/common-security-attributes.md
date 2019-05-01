---
title: Atributos de seguridad para servicios de Azure
description: Lista de comprobación de los atributos de seguridad que se utilizan habitualmente para evaluar Azure Service Fabric
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 94681351758f34cc53c425f3207660bdb09f6494
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717799"
---
# <a name="common-security-attributes-for-azure-services"></a>Atributos comunes de seguridad para servicios de Azure

La seguridad está integrada en todos los aspectos de los servicios de Azure. En este artículo recopila los atributos de seguridad común para los servicios de Azure seleccionados. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="azure-backupbackupbackup-security-attributesmd"></a>[Azure Backup](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sin  | Se usa HTTPS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sin  |  |
| Cifrado de nivel de columna (Azure Data Services)| Sin  |  |
| Llamadas API cifradas| Sí |  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sin  |  |
| Compatibilidad con inserción de redes virtuales| Sin  |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | La tunelización forzada es compatible con copia de seguridad de VM. La tunelización forzada no se admite para las cargas de trabajo que se ejecutan en VM. |
| Fuerza la tunelización de soporte técnico| Sin  |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Log Analytics se admite a través de los registros de diagnóstico. Para más información, consulte Monitor Azure Backup protected workloads using Log Analytics (Supervisión de cargas de trabajo protegidas por Azure Backup con Log Analytics, https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Autorización| Sí | Se usan roles RBAC integrados y creados por el cliente. Consulte Control de acceso basado en rol para administrar puntos de recuperación de copia de seguridad de Azure (/ / / copia de seguridad-rbac-rs-almacén de azure backup) para obtener más información. |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Todas las acciones que desencadena el cliente desde Azure Portal se registran en los registros de actividad. |
| Auditoría y registro del plano de datos| Sin  | No se puede acceder al plano de datos de Azure Backup directamente.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí|  |

## <a name="azure-key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Azure Key Vault](../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Se cifran todos los objetos. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Toda la comunicación se realiza a través de llamadas a API cifradas |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | El cliente controla todas las claves de su almacén de claves. Cuando se especifican las claves de módulo (HSM) copiada de seguridad de hardware, un HSM de FIPS nivel 2 protege la clave, el certificado o el secreto. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Se usa HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí | Uso de puntos de conexión de servicio de red Virtual (VNet). |
| Compatibilidad con inserción de redes virtuales| Sin  |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Usar reglas de firewall de red virtual. |
| Fuerza la tunelización de soporte técnico| Sin  |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Se usa Log Analytics. |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Autorización| Sí | Se usa la directiva de acceso de Key Vault. |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de plano de administración y de control| Sí | Se usa Log Analytics. |
| Auditoría y registro del plano de datos| Sí | Se usa Log Analytics. |

### <a name="access-controls"></a>Controles de acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Controles de acceso del plano de administración y de control | Sí | Control de acceso basado en rol (RBAC) de Azure Resource Manager |
| Controles de acceso del plano de datos (en cada nivel de servicio) | Sí | Directiva de acceso de Key Vault |

## <a name="azure-service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Azure Service Fabric](../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales (VM) en el que se basa el clúster. Cifrado de disco de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí |  |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales (VM) en el que se basa el clúster. Cifrado de disco de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Las llamadas de API de Service Fabric se hacen a través de Azure Resource Manager. Se requiere un token web de JSON (JWT) válido. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de redes virtuales| Sí |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Usando grupos de seguridad de red (NSG). |
| Fuerza la tunelización de soporte técnico| Sí | Las redes de Azure proporcionan la tunelización forzada. |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Uso de soporte técnico y soporte técnico de terceros de supervisión de Azure. |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Autorización| Sí | Administración de identidad y acceso (IAM) para llamadas a través de SFRP. Las llamadas directamente al punto de conexión del clúster son compatibles con dos roles: usuario y administrador. El cliente puede asignar las API a cualquiera de estos roles. |


### <a name="audit-trail"></a>Registro de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Auditoría y registro del plano de datos| N/D | El cliente es propietario del clúster.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | El control de versiones y la implementación de la configuración del servicio se hacen mediante Azure Deploy. El control de versiones del código (aplicación y tiempo de ejecución) se hace mediante Azure Build.
 |

## <a name="azure-storage"></a>Azure Storage

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí |  |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Admite los mecanismos de HTTPS/TLS estándar.  Los usuarios también pueden cifrar datos antes de transmitirse al servicio. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | Consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md).|
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí |  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de redes virtuales| N/D |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | |
| Fuerza la tunelización de soporte técnico| N/D |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Métricas de Azure Monitor disponibles ahora, los registros de versión preliminar inicial |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory, clave compartida, el token de acceso compartido. |
| Autorización| Sí | Autorización de soporte técnico a través de RBAC, ACL de POSIX y Tokens de SAS |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración | Sí | Registro de actividad de Azure Resource Manager |
| Auditoría y registro del plano de datos| Sí | Los registros de diagnóstico de servicio y vista previa a partir de registro de Azure Monitor  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Permite controlar las versiones del proveedor de recursos a través de API de Azure Resource Manager |