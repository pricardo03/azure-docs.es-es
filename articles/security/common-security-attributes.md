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
ms.openlocfilehash: 460d8756c437a1212aef054cf069be2bccac8c8a
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001361"
---
# <a name="security-attributes-for-azure-services"></a>Atributos de seguridad para servicios de Azure

En este artículo recopila los atributos de seguridad común para los servicios de Azure seleccionados. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="azure-api-managementapi-managementapi-management-security-attributesmd"></a>[Azure API Management](../api-management/api-management-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí (solo para el cifrado del servicio) | Los datos confidenciales, como certificados, claves y valores con nombre secreto se cifran con administradas del servicio, por las claves de la instancia de servicio. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | [Expressroute](../expressroute/index.yml) y proporciona el cifrado de red virtual por [redes de Azure](../virtual-network/index.yml). |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| No | Todas las claves de cifrado son por instancia de servicio y servicio administrado. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de plano de administración se realizan a través de [Azure Resource Manager](../azure-resource-manager/index.yml) a través de TLS. Se requiere un token web de JSON (JWT) válido.  Las llamadas de plano de datos pueden estar protegidas con TLS y uno de los mecanismos de autenticación admitidos (por ejemplo, el certificado de cliente o JWT).
 |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| No | |
| Compatibilidad con inserción de redes virtuales| Sí | |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Uso de grupos de seguridad de red (NSG) y Azure Application Gateway (u otro dispositivo de software), respectivamente. |
| Fuerza la tunelización de soporte técnico| Sí | Las redes de Azure proporcionan la tunelización forzada. |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Autorización| Sí | |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | [Registros de actividad de Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Auditoría y registro del plano de datos| Sí | [Registros de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) y (opcionalmente) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Mediante el [Kit de recursos de DevOps de Azure API Management](https://aka.ms/apimdevops) |


## <a name="azure-app-serviceapp-serviceapp-service-security-attributesmd"></a>[Azure App Service](../app-service/app-service-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Contenido del archivo de sitio Web se almacena en Azure Storage, que cifra automáticamente el contenido en reposo. Consulte [cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md).<br><br>Los secretos de la proporcionada por el usuario se cifran en reposo. Los secretos se cifran en reposo mientras se almacenan en bases de datos de configuración de App Service.<br><br>Los discos conectados localmente, opcionalmente, pueden utilizarse como almacenamiento temporal por los sitios Web (D:\local y % TMP %). Los discos conectados localmente no se cifran en reposo. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y redes)| Sí | Los clientes pueden configurar los sitios web para requerir y usar HTTPS para el tráfico entrante. Consulte el blog [cómo hacer que Azure App Service HTTPS solo](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | Los clientes pueden elegir almacenar secretos de aplicación en Key Vault y recuperarlos en tiempo de ejecución. Consulte [hace referencia un uso de Key Vault para App Service y Azure Functions (versión preliminar)](../app-service/app-service-key-vault-references.md).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Se realizan llamadas de administración para configurar App Service a través de [Azure Resource Manager](../azure-resource-manager/index.yml) llamadas a través de HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí | Disponible actualmente en versión preliminar de App Service. Consulte [restricciones de acceso de Azure App Service](../app-service/app-service-ip-restrictions.md). |
| Compatibilidad con inserción de redes virtuales| Sí | Entornos de App Service son implementaciones privadas de App Service dedicado a un solo cliente insertado en una red virtual de cliente. Consulte [Introducción a App Service Environment](../app-service/environment/intro.md). |
| Compatibilidad con aislamiento de red y Firewalling| Sí | La variación pública de varios inquilinos de App Service, los clientes pueden configurar las ACL (restricciones de IP) para bloquear el tráfico entrante permitido de red.  Consulte [restricciones de acceso de Azure App Service](../app-service/app-service-ip-restrictions.md).  App Service Environment se implementa directamente en redes virtuales y, por tanto, se puede proteger con NSG. |
| Fuerza la tunelización de soporte técnico| Sí | Entornos de App Service se pueden implementar en una red virtual de cliente donde se configura la tunelización forzada. Los clientes tienen que seguir las instrucciones de [configurar App Service Environment con tunelización forzada](../app-service/environment/forced-tunnel-support.md). |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | App Service se integra con Application Insights para lenguajes compatibles con Application Insights (versión completa de .NET Framework, .NET Core, Java y Node.JS).  Consulte [rendimiento de supervisión de Azure App Service](../azure-monitor/app/azure-web-apps.md). App Service también envía las métricas de aplicación en Azure Monitor. Consulte [supervisar aplicaciones en Azure App Service](../app-service/web-sites-monitor.md). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Los clientes pueden crear aplicaciones en App Service que se integran automáticamente con [Azure Active Directory (Azure AD)](../active-directory/index.md) , así como otros proveedores de identidades compatible con OAuth; vea [autenticación y autorización en Azure App Service](../app-service/overview-authentication-authorization.md). Para administración de acceso a los recursos de App Service, todo el acceso se controla mediante una combinación de entidad de seguridad de Azure AD autenticado y roles de RBAC de Azure Resource Manager. |
| Autorización| Sí | Para administración de acceso a los recursos de App Service, todo el acceso se controla mediante una combinación de entidad de seguridad de Azure AD autenticado y roles de RBAC de Azure Resource Manager.  |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Todas las operaciones de administración realizadas en objetos de App Service se producen a través de [Azure Resource Manager](../azure-resource-manager/index.yml). Los registros históricos de estas operaciones están disponibles tanto en el portal a través de la CLI; consulte [operaciones del proveedor de recursos de Azure Resource Manager](../role-based-access-control/resource-provider-operations.md#microsoftweb) y [registro de actividad de monitor az](/cli/azure/monitor/activity-log). |
| Auditoría y registro del plano de datos | No | El plano de datos de App Service es un recurso compartido de archivos remoto que contiene un sitio de web de cliente s implementar contenido.  No hay ninguna auditoría del recurso compartido de archivos remoto. |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Para las operaciones de administración, el estado de una configuración de App Service se puede exportar como una plantilla de Azure Resource Manager y con control de versiones con el tiempo. Para las operaciones en tiempo de ejecución, los clientes pueden mantener versiones diferentes en vivo de una aplicación con la característica de ranuras de implementación de App Service. | 


## <a name="azure-resource-managerazure-resource-managerazure-resource-manager-security-attributesmd"></a>[Azure Resource Manager](../azure-resource-manager/azure-resource-manager-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí |  |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| N/D | Azure Resource Manager no almacena ningún contenido de cliente, solo los datos de control. |
| Cifrado de nivel de columna (Azure Data Services)| Sí | |
| Llamadas a API cifradas| Sí | |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| No | |
| Compatibilidad con inserción de redes virtuales| Sí | |
| Aislamiento de red y la compatibilidad con Firewall| No |  |
| Fuerza la tunelización de soporte técnico| No |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| No | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | [Azure Active Directory](/azure/active-directory) basado.|
| Autorización| Sí | |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Registros de actividad exponer todas operaciones de escritura (PUT, POST, DELETE) realizadas en los recursos; consulte [ver registros de actividad para auditar las acciones sobre los recursos](../azure-resource-manager/resource-group-audit.md). |
| Auditoría y registro del plano de datos| N/D | |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí |  |


## <a name="azure-backupbackupbackup-security-attributesmd"></a>[Azure Backup](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| No | Se usa HTTPS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| No |  |
| Cifrado de nivel de columna (Azure Data Services)| No |  |
| Llamadas API cifradas| Sí |  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| No |  |
| Compatibilidad con inserción de redes virtuales| No |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | La tunelización forzada es compatible con copia de seguridad de VM. La tunelización forzada no se admite para las cargas de trabajo que se ejecutan en VM. |
| Fuerza la tunelización de soporte técnico| No |  |

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
| Auditoría y registro del plano de datos| No | No se puede acceder al plano de datos de Azure Backup directamente.  |

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
| Compatibilidad con inserción de redes virtuales| No |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Usar reglas de firewall de red virtual. |
| Fuerza la tunelización de soporte técnico| No |  |

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

## <a name="azure-service-bus-messagingservice-bus-messagingservice-bus-messaging-security-attributesmd"></a>[Azure mensajería de Service Bus](../service-bus-messaging/service-bus-messaging-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>|  Sí para el cifrado en reposo de lado del servidor de forma predeterminada. | No se admiten claves administradas por el cliente y BYOK todavía. Cifrado del lado cliente es responsabilidad del cliente |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Es compatible con el mecanismo estándar de HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| No |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de API se realizan a través de [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí (solo en el nivel Premium) | Se admiten los puntos de conexión de servicio de red virtual para [nivel Premium de Service Bus](../service-bus-messaging/service-bus-premium-messaging.md) solo. |
| Compatibilidad con inserción de redes virtuales| No | |
| Aislamiento de red y la compatibilidad con Firewall| Sí (solo en el nivel Premium) |  |
| Fuerza la tunelización de soporte técnico| No |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Compatible a través de [Azure Monitor y alertas](../service-bus-messaging/service-bus-metrics-azure-monitor.md). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Administrar a través de [Azure Active Directory Managed Service Identity](../service-bus-messaging/service-bus-managed-service-identity.md); vea [autorización y autenticación de Service Bus](../service-bus-messaging/service-bus-authentication-and-authorization.md).|
| Autorización| Sí | Admite la autorización a través de [RBAC](../service-bus-messaging/service-bus-role-based-access-control.md) (versión preliminar) y el token de SAS; vea [autorización y autenticación de Service Bus](../service-bus-messaging/service-bus-authentication-and-authorization.md). |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Registros de operaciones están disponibles; consulte [los registros de diagnóstico de Service Bus](../service-bus-messaging/service-bus-diagnostic-logs.md).  |
| Auditoría y registro del plano de datos| No |  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | Es compatible con versiones del proveedor de recursos a través de la [API de Azure Resource Manager](/rest/api/resources/).|


## <a name="azure-service-bus-relayservice-bus-relayservice-bus-relay-security-attributesmd"></a>[Azure Service Bus Relay](../service-bus-relay/service-bus-relay-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>|  N/D | Retransmisión es un socket web y no conserva los datos. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | El servicio requiere TLS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| No | Usa solo los certificados de Microsoft TLS.  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| No |  |
| Aislamiento de red y la compatibilidad con Firewall| No |  |
| Fuerza la tunelización de soporte técnico| N/D | Retransmisión es el túnel TLS  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Via SAS. |
| Autorización|  Sí | Via SAS. |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | A través de [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Auditoría y registro del plano de datos| Sí | Éxito de la conexión o error y errores y ha iniciado.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | A través de [Azure Resource Manager](../azure-resource-manager/index.yml).|


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

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Auditoría y registro del plano de datos| N/D | El cliente es propietario del clúster.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | El control de versiones y la implementación de la configuración del servicio se hacen mediante Azure Deploy. El control de versiones del código (aplicación y tiempo de ejecución) se hace mediante Azure Build.
 |

## <a name="azure-sql-databasesql-databasesql-database-security-attributesmd"></a>[Azure SQL Database](../sql-database/sql-database-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Conoce como "cifrado en uso", como se describe en el artículo [Always Encrypted](../sql-database/sql-database-always-encrypted.md). Cifrado del lado del servicio usa [cifrado de datos transparente](../sql-database/transparent-data-encryption-azure-sql.md) (TDE).|
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Se usa HTTPS. |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | Se ofrecen control de claves administradas del servicio tanto administradas por el cliente (a través de este último [Azure Key Vault](../key-vault/index.yml). |
| Cifrado de nivel de columna (Azure Data Services)| Sí | A través de [Always Encrypted](../sql-database/sql-database-always-encrypted.md). |
| Llamadas a API cifradas| Sí | Uso de HTTPS/SSL. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí | Se aplica a [base de datos única](../sql-database/sql-database-single-index.yml) solo. |
| Compatibilidad con inserción de redes virtuales| Sí | Se aplica a [instancia administrada](../sql-database/sql-database-managed-instance.md) solo. |
| Aislamiento de red y la compatibilidad con Firewall| Sí | En ambas bases de datos - y nivel de servidor; de Firewall aislamiento de red [instancia administrada](../sql-database/sql-database-managed-instance.md) solo |
| Fuerza la tunelización de soporte técnico| Sí | [instancia administrada](../sql-database/sql-database-managed-instance.md) a través de [Azure ExpressRoute](../expressroute/index.yml) VPN |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | La solución SIEM de terceros de Imperva (SecureSphere) también se admite, a través [Azure Event Hubs](../event-hubs/index.yml) integración a través de [auditoría SQL](../sql-database/sql-database-auditing.md). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory. |
| Autorización| Sí |  |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Sí, solo algunos eventos. |
| Auditoría y registro del plano de datos | Sí | A través de [auditoría SQL](../sql-database/sql-database-auditing.md). |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| No  | | 

### <a name="additional-security-attributes-for-sql-database"></a>Atributos de seguridad adicionales para la base de datos de SQL

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Preventivas: evaluación de vulnerabilidad | Sí | Consulte [evaluación de vulnerabilidad de SQL service le ayuda a identificar las vulnerabilidades de la base de datos](../sql-database/sql-vulnerability-assessment.md). |
| Preventivas: clasificación y detección de datos  | Sí | Consulte [clasificación y detección de datos de Azure SQL Database y SQL Data Warehouse](../sql-database/sql-database-data-discovery-and-classification.md). |
| Detección: detección de amenazas | Sí | Consulte [protección contra amenazas para Azure SQL Database avanzada](../sql-database/sql-database-threat-detection-overview.md). |

## <a name="azure-storagestoragecommonstorage-security-attributesmd"></a>[Azure Storage](../storage/common/storage-security-attributes.md)

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