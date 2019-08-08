---
title: Atributos de seguridad para los servicios de Azure
description: Lista de comprobación de los atributos de seguridad para evaluar los servicios de Azure
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.subservice: security-fundamentals
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: mbaldwin
ms.openlocfilehash: 43eb7e5c4cab722eb97f9e2fe819c9c79bae45d9
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828771"
---
# <a name="security-attributes-for-azure-services"></a>Atributos de seguridad para los servicios de Azure

En este artículo se recopilan los atributos de seguridad para los servicios de Azure seleccionados. Un atributo de seguridad es una calidad o característica de un servicio de Azure. Contribuye a la capacidad del servicio para evitar, detectar y responder a las vulnerabilidades de seguridad.

Los atributos de seguridad se clasifican en las siguientes categorías:
* Prevención
* Segmentación de red
* Detección
* Compatibilidad con la administración de identidades y acceso
* Pista de auditoría
* Controles de acceso (si se utilizan)
* Administración de la configuración (si se utiliza)

En cada categoría, se muestra "Sí" o "No" para indicar si se usa un atributo. En algunos servicios, se muestra "N/D" para indicar que un atributo no es aplicable. También se puede proporcionar una nota o un vínculo para aportar más información sobre un atributo.

## <a name="api-managementazureapi-managementapi-management-security-attributes"></a>[API Management](/azure/api-management/api-management-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Los datos confidenciales, como certificados, claves y valores con nombre secreto, se cifran con claves que administra el servicio por instancia de servicio. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | El cifrado de [ExpressRoute](/azure/expressroute/index) y de red virtual lo proporcionan las [redes de Azure](/azure/virtual-network/index). |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin | Todas las claves de cifrado son por instancia de servicio y las administra el servicio. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas del plano de administración se hacen mediante [Azure Resource Manager](/azure/azure-resource-manager/index) con TLS. Se requiere un token web de JSON (JWT) válido.  Las llamadas del plano de datos pueden estar protegidas con TLS y uno de los mecanismos de autenticación admitidos (por ejemplo, el certificado de cliente o JWT).
 |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Se usan grupos de seguridad de red (NSG) y Azure Application Gateway (u otro dispositivo de software), respectivamente. |
| Compatibilidad con la tunelización forzada| Sí | Las redes de Azure proporcionan la tunelización forzada. |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Authorization| Sí | |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | [Registros de actividad en Azure Monitor](/azure/azure-monitor/platform/activity-logs-overview) |
| Registro y auditoría del plano de datos| Sí | [Registros de diagnóstico de Azure Monitor](/azure/azure-monitor/platform/diagnostic-logs-overview) y (opcionalmente) [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview).|

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Se usa el [kit de recursos de DevOps de Azure API Management](https://aka.ms/apimdevops) |

### <a name="vulnerability-scans-false-positives"></a>Exámenes de vulnerabilidades de falsos positivos

Esta sección documenta las vulnerabilidades más comunes, que no afectan a Azure API Management.

| Punto vulnerable               | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed es una vulnerabilidad en la implementación de la extensión TLS SessionTicket que se encuentra en algunos productos de F5. Permite la pérdida ("hemorragia") de hasta 31 bytes de datos de la memoria sin inicializar. Esto se debe a que la pila de TLS rellena con datos un identificador de sesión, que pasa el cliente, para que tenga una longitud de 32 bits. |


## <a name="app-serviceazureapp-serviceapp-service-security-attributes"></a>[App Service](/azure/app-service/app-service-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | El contenido de los archivos de sitio web se almacena en Azure Storage, que cifra automáticamente el contenido en reposo. Consulte [Cifrado de Azure Storage para datos en reposo](/azure/storage/common/storage-service-encryption).<br><br>Los secretos suministrados por el cliente se cifran en reposo. Los secretos se cifran en reposo mientras se almacenan en las bases de datos de configuración de App Service.<br><br>Los sitios web pueden utilizar los discos conectados localmente como almacenamiento temporal (D:\local y %TMP%). Los discos conectados localmente no se cifran en reposo. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | Los clientes pueden configurar los sitios web para que exijan y usen HTTPS para el tráfico entrante. Consulte la entrada de blog [How to make an Azure App Service HTTPS only](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (Cómo hacer que Azure App Service use solo HTTPS). |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sí | Los clientes pueden elegir almacenar secretos de aplicación en Key Vault y recuperarlos en el entorno en tiempo de ejecución. Consulte [Uso de referencias de Key Vault para App Service y Azure Functions (versión preliminar)](/azure/app-service/app-service-key-vault-references).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de administración para configurar App Service se realizan mediante llamadas de [Azure Resource Manager](/azure/azure-resource-manager/index) con HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | Disponible actualmente en versión preliminar para App Service. Consulte [Restricciones de acceso de Azure App Service](/azure/app-service/app-service-ip-restrictions). |
| Compatibilidad con la inserción de redes virtuales| Sí | App Service Environment son implementaciones privadas de App Service dedicadas a un solo cliente insertado en una red virtual del cliente. Consulte [Introducción a App Service Environment](/azure/app-service/environment/intro). |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | En la variación pública de varios inquilinos de App Service, los clientes pueden configurar las ACL (restricciones de IP) para bloquear el tráfico entrante permitido.  Consulte [Restricciones de acceso de Azure App Service](/azure/app-service/app-service-ip-restrictions).  App Service Environment se implementa directamente en las redes virtuales y, por tanto, se puede proteger con NSG. |
| Compatibilidad con la tunelización forzada| Sí | App Service Environment se puede implementar en la red virtual de un cliente que tenga configurada la tunelización forzada. Los clientes tienen que seguir las instrucciones de [Configuración de App Service Environment con tunelización forzada](/azure/app-service/environment/forced-tunnel-support). |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | App Service se integra con Application Insights para los lenguajes compatibles con Application Insights (edición completa de .NET Framework, .NET Core, Java y Node.JS).  Consulte [Supervisar el rendimiento de Azure App Service](/azure/azure-monitor/app/azure-web-apps). App Service también envía las métricas de aplicación a Azure Monitor. Consulte [Supervisión de aplicaciones en Azure App Service](/azure/app-service/web-sites-monitor). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Los clientes pueden crear aplicaciones en App Service que se integren automáticamente con [Azure Active Directory (Azure AD)](/azure/active-directory/index), así como otros proveedores de identidades compatibles con OAuth; consulte [Autenticación y autorización en Azure App Service](/azure/app-service/overview-authentication-authorization). Para el acceso de administración a los recursos de App Service, todo el acceso se controla mediante una combinación de los roles RBAC de Azure Resource Manager y de las entidades de seguridad autenticadas de Azure AD. |
| Authorization| Sí | Para el acceso de administración a los recursos de App Service, todo el acceso se controla mediante una combinación de los roles RBAC de Azure Resource Manager y de las entidades de seguridad autenticadas de Azure AD.  |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Todas las operaciones de administración realizadas en objetos de App Service se producen a través de [Azure Resource Manager](/azure/azure-resource-manager/index). Los registros históricos de estas operaciones están disponibles tanto en el portal como a través de la CLI; consulte [Operaciones del proveedor de recursos de Azure Resource Manager](/azure/role-based-access-control/resource-provider-operations#microsoftweb) y [az monitor activity-log](/cli/azure/monitor/activity-log). |
| Registro y auditoría del plano de datos | Sin | El plano de datos de App Service es un recurso compartido de archivos remoto que incluye el contenido del sitio web implementado de un cliente.  No hay ninguna auditoría del recurso compartido de archivos remoto. |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Para las operaciones de administración, el estado de una configuración de App Service se puede exportar como plantilla de Azure Resource Manager y con control de versiones a lo largo del tiempo. Para las operaciones en tiempo de ejecución, los clientes pueden mantener versiones diferentes en vivo de una aplicación con la característica de espacios de implementación de App Service. | 



## <a name="azure-resource-managerazureazure-resource-managerazure-resource-manager-security-attributes"></a>[Azure Resource Manager](/azure/azure-resource-manager/azure-resource-manager-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etc.)| N/D | Azure Resource Manager no almacena ningún contenido de cliente, solo los datos de control. |
| Cifrado de nivel de columna (Azure Data Services)| Sí | |
| Llamadas a API cifradas| Sí | |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sin |  |
| Compatibilidad con la tunelización forzada| Sin |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sin | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Basado en [Azure Active Directory](/azure/active-directory).|
| Authorization| Sí | |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Los registros de actividad exponen todas operaciones de escritura (PUT, POST, DELETE) realizadas en los recursos; consulte [Visualización de registros de actividad para supervisar acciones sobre recursos](/azure/azure-resource-manager/resource-group-audit). |
| Registro y auditoría del plano de datos| N/D | |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí |  |


## <a name="azure-backupazurebackupbackup-security-attributes"></a>[Azure Backup](/azure/backup/backup-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)| Sí | Uso de cifrado del servicio de almacenamiento para las cuentas de almacenamiento. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sin | Se usa HTTPS. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| Sin |  |
| Llamadas API cifradas| Sí |  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin |  |
| Compatibilidad con la inserción de redes virtuales| Sin |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | La tunelización forzada es compatible con copia de seguridad de VM. La tunelización forzada no se admite para las cargas de trabajo que se ejecutan en VM. |
| Compatibilidad con la tunelización forzada| Sin |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Log Analytics se admite a través de los registros de diagnóstico. Para más información, consulte [Monitor Azure Backup protected workloads using Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) (Supervisión de cargas de trabajo protegidas por Azure Backup con Log Analytics). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Se usan roles RBAC integrados y creados por el cliente. Para más información, consulte [Uso del control de acceso basado en roles para administrar puntos de recuperación de Azure Backup](/azure/backup/backup-rbac-rs-vault). |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Todas las acciones que desencadena el cliente desde Azure Portal se registran en los registros de actividad. |
| Registro y auditoría del plano de datos| Sin | No se puede acceder al plano de datos de Azure Backup directamente.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí|  |

## <a name="cosmos-dbazurecosmos-dbcosmos-db-security-attributes"></a>[Cosmos DB](/azure/cosmos-db/cosmos-db-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/no | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Todas las bases de datos de Cosmos DB y las copias de seguridad se cifran de forma predeterminada; consulte [Cifrado de datos en Azure Cosmos DB](/azure/cosmos-db/database-encryption-at-rest). No se admite el cifrado del servidor con claves administradas por el cliente. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Todos los datos de Azure Cosmos DB se cifran en tránsito. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| Sí | Solo en las instancias de Table API Premium. No todas las API admiten esta característica. Consulte [Introducción a Azure Cosmos DB: Table API](/azure/cosmos-db/table-introduction). |
| Llamadas a API cifradas| Sí | Todas las conexiones a Azure Cosmos DB admiten HTTPS. Azure Cosmos DB también admite las conexiones TLS 1.2, pero esto no es obligatorio todavía. Si los clientes desactivan la conexión TLS de nivel inferior por su parte, pueden garantizar la conexión a Cosmos DB.  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/no | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| Sí | Con el punto de conexión de servicio de red virtual, puede configurar la cuenta de Azure Cosmos DB para permitir el acceso solo desde una subred específica de una red virtual (VNet). También puede combinar el acceso de red virtual con reglas de firewall.  Consulte [Acceso a Azure Cosmos DB desde redes virtuales](/azure/cosmos-db/vnet-service-endpoint) |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Gracias a la compatibilidad con el firewall, puede configurar su cuenta de Azure Cosmos para permitir el acceso solo desde un conjunto de direcciones IP aprobado, un intervalo de direcciones IP o servicios en la nube. Consulte [Configuración del firewall de IP en Azure Cosmos DB](/azure/cosmos-db/how-to-configure-firewall).|
| Compatibilidad con la tunelización forzada| Sí | Se puede configurar en el cliente en la red virtual donde se encuentran las máquinas virtuales.   |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Se registran todas las solicitudes que se envían a Azure Cosmos DB. Se admiten la [supervisión de Azure](/azure/azure-monitor/overview), las métricas de Azure y el registro de auditoría de Azure.  Puede registrar la información correspondiente a las solicitudes de plano de datos, las estadísticas de tiempo de ejecución de consultas, el texto de la consulta y las solicitudes de MongoDB. También puede configurar alertas. |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Authentication| Sí | Sí en el nivel de cuenta de base de datos; en el nivel del plano de datos, Cosmos DB usa tokens de recursos y acceso con clave. |
| Authorization| Sí | Se admite en la cuenta de Azure Cosmos con las claves maestras (principal y secundaria) y tokens de recursos. Puede obtener acceso de lectura/escritura o acceso de solo lectura a los datos con las claves maestras. Los tokens de recursos permiten el acceso de tiempo limitado a los recursos, como documentos y contenedores. |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | El registro de actividad de Azure para las operaciones de nivel de cuenta, como Firewalls, redes virtuales, acceso con claves e IAM. |
| Registro y auditoría del plano de datos | Sí | Registro de supervisión de diagnósticos para las operaciones de nivel de contenedor, como la creación contenedores, el aprovisionamiento de rendimiento, la indexación de directivas y las operaciones CRUD en documentos. |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sin  | | 

### <a name="additional-security-attributes-for-cosmos-db"></a>Atributos de seguridad adicionales para Cosmos DB

| Atributo de seguridad | Sí/no | Notas|
|---|---|--|
| Uso compartido de recursos entre orígenes (CORS) | Sí | Consulte [Configuración del uso compartido de recursos entre orígenes (CORS)](/azure/cosmos-db/how-to-configure-cross-origin-resource-sharing). |


## <a name="event-hubsazureevent-hubsevent-hubs-security-attributes"></a>[Event Hubs](/azure/event-hubs/event-hubs-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) |  Sí | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sin |  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí |  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| Sin | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |
| Compatibilidad con la tunelización forzada| Sin |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Authorization|  Sí | |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos| Sí |   |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | |


## <a name="expressrouteazureexpressrouteexpressroute-security-attributes"></a>[ExpressRoute](/azure/expressroute/expressroute-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) |  N/D | ExpressRoute no almacena datos de clientes. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sin | |
| Control de claves de cifrado (CMK, BYOK, etc.)| N/D |  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Mediante [Azure Resource Manager](/azure/azure-resource-manager/index) y HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D |  |
| Compatibilidad con inserción de red virtual| N/D | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Cada cliente está contenido en su propio dominio de enrutamiento y tunelizado a su propia red virtual |
| Compatibilidad con la tunelización forzada| N/D | Mediante el Protocolo de puerta de enlace de borde (BGP). |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Supervisión, métricas y alertas de ExpressRoute ](/azure/expressroute/expressroute-monitoring-metrics-alerts).|

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Cuenta de servicio de Gateway for Microsoft (GWM) (controlador); acceso Just-in-Time (JIT) para desarrollo y operaciones. |
| Authorization|  Sí |Cuenta de servicio de Gateway for Microsoft (GWM) (controlador); acceso Just-in-Time (JIT) para desarrollo y operaciones. |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas| 
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos| Sin |   |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante el proveedor de recursos de red (NRP). |


## <a name="key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Key Vault](../../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Se cifran todos los objetos. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | Toda la comunicación se realiza a través de llamadas a API cifradas |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sí | El cliente controla todas las claves de su almacén de claves. Cuando se especifican claves con el respaldo del módulo de seguridad de hardware (HSM), un HSM de nivel 2 de FIPS protege la clave, el certificado o el secreto. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Se usa HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | Se usan puntos de conexión de servicio de red virtual (VNet). |
| Compatibilidad con la inserción de redes virtuales| Sin |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Se usan reglas de firewall de red virtual. |
| Compatibilidad con la tunelización forzada| Sin |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Se usa Log Analytics. |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Se usa la directiva de acceso de Key Vault. |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de plano de administración y de control| Sí | Se usa Log Analytics. |
| Registro y auditoría del plano de datos| Sí | Se usa Log Analytics. |

### <a name="access-controls"></a>Controles de acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Controles de acceso del plano de administración y de control | Sí | Control de acceso basado en rol (RBAC) de Azure Resource Manager |
| Controles de acceso del plano de datos (en cada nivel de servicio) | Sí | Directiva de acceso de Key Vault |

## <a name="load-balancerload-balancerload-balancer-security-attributesmd"></a>[Equilibrador de carga](../../load-balancer/load-balancer-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) | N/D | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| N/D | |
| Control de clave de cifrado (CMK, BYOK, etc.)| N/D | |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de [Azure Resource Manager](/azure/azure-resource-manager/index). |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D | |
| Compatibilidad con inserción de red virtual| N/D | . |
| Compatibilidad con el aislamiento de red y los firewalls| N/D |  |
| Compatibilidad con la tunelización forzada| N/D | |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Registros de Azure Monitor para el equilibrador de carga básica público](/azure/load-balancer/load-balancer-monitor-log). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| N/D |  |
| Authorization| N/D |  |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Consulte [Registros de Azure Monitor para el equilibrador de carga básica público](/azure/load-balancer/load-balancer-monitor-log). |
| Registro y auditoría del plano de datos | N/D |  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| N/D |  | 

## <a name="service-bus-messagingazureservice-bus-messagingservice-bus-messaging-security-attributes"></a>[Service Bus Messaging](/azure/service-bus-messaging/service-bus-messaging-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) |  Sí, para el cifrado en reposo en el servidor, de forma predeterminada. | No se admiten todavía claves administradas por el cliente ni BYOK. El cifrado del lado del cliente es responsabilidad de este. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | Compatible con el mecanismo estándar de HTTPS/TLS. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de API se realizan mediante [Azure Resource Manager](/azure/azure-resource-manager/index) y HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con los puntos de conexión de servicio| Sí (solo el nivel Premium) | Se admiten los puntos de conexión de servicio de red virtual solo para el [nivel Premium de Service Bus](/azure/service-bus-messaging/service-bus-premium-messaging). |
| Compatibilidad con la inserción de redes virtuales| Sin | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí (solo el nivel Premium) |  |
| Compatibilidad con la tunelización forzada| Sin |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Compatible mediante [Azure Monitor y las alertas](/azure/service-bus-messaging/service-bus-metrics-azure-monitor). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Administrada mediante [Managed Service Identity de Azure Active Directory](/azure/service-bus-messaging/service-bus-managed-service-identity); consulte [Autenticación y autorización de Service Bus](/azure/service-bus-messaging/service-bus-authentication-and-authorization).|
| Authorization| Sí | Admite la autorización mediante [RBAC](/azure/service-bus-messaging/service-bus-role-based-access-control) (versión preliminar) y el token de SAS; consulte [Autenticación y autorización de Service Bus](/azure/service-bus-messaging/service-bus-authentication-and-authorization). |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Están disponibles los registros de las operaciones; consulte [Registros de diagnóstico de Service Bus](/azure/service-bus-messaging/service-bus-diagnostic-logs).  |
| Registro y auditoría del plano de datos| Sin |  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Compatible con el control de versiones del proveedor de recursos mediante la [API de Azure Resource Manager](/rest/api/resources/).|


## <a name="service-bus-relayazureservice-bus-relayservice-bus-relay-security-attributes"></a>[Service Bus Relay](/azure/service-bus-relay/service-bus-relay-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) |  N/D | La retransmisión es un socket web y no conserva los datos. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | El servicio requiere TLS. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin | Usa solo los certificados TLS de Microsoft.  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | HTTPS. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sin |  |
| Compatibilidad con la tunelización forzada| N/D | La retransmisión es el túnel de TLS  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Mediante SAS. |
| Authorization|  Sí | Mediante SAS. |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Mediante [Azure Resource Manager](/azure/azure-resource-manager/index). |
| Registro y auditoría del plano de datos| Sí | Conexión correcta o con errores, y errores y registros.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante [Azure Resource Manager](/azure/azure-resource-manager/index).|

## <a name="service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Service Fabric](../../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales en el que se basa el clúster. El cifrado de discos de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales en el que se basa el clúster. El cifrado de discos de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Las llamadas de API de Service Fabric se hacen a través de Azure Resource Manager. Se requiere un token web de JSON (JWT) válido. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| Sí |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Usando grupos de seguridad de red (NSG). |
| Compatibilidad con la tunelización forzada| Sí | Las redes de Azure proporcionan la tunelización forzada. |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Con la compatibilidad de supervisión de Azure y la compatibilidad de las soluciones de terceros. |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Administración de identidad y acceso (IAM) para llamadas a través de SFRP. Las llamadas directamente al punto de conexión del clúster son compatibles con dos roles: usuario y administrador. El cliente puede asignar las API a cualquiera de estos roles. |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Registro y auditoría del plano de datos| N/D | El cliente es propietario del clúster.  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | |

## <a name="sql-databasesql-databasesql-database-security-attributesmd"></a>[SQL Database](../../sql-database/sql-database-security-attributes.md)

SQL Database incluye una [base de datos única](/azure/sql-database/sql-database-single-index) y una [instancia administrada](/azure/sql-database/sql-database-managed-instance). Las entradas siguientes se aplican a ambas ofertas, excepto si se indica lo contrario.

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Se denomina "cifrado en uso", tal y como se describe en el artículo [Always Encrypted](/azure/sql-database/sql-database-always-encrypted). El cifrado del servidor usa el [cifrado de datos transparente](/azure/sql-database/transparent-data-encryption-azure-sql).|
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | Se usa HTTPS. |
| Control de claves de cifrado, como CMK o BYOK| Sí | Se ofrece control de claves tanto administradas por el servicio como por el cliente. Este último se ofrece mediante [Azure Key Vault](/azure/key-vault/index). |
| Cifrado de nivel de columna proporcionado por Azure Data Services| Sí | Mediante [Always Encrypted](/azure/sql-database/sql-database-always-encrypted). |
| Llamadas a API cifradas| Sí | Con HTTPS/SSL. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | Se aplica solo a las [bases de datos únicas](/azure/sql-database/sql-database-single-index). |
| Compatibilidad con la inserción de Azure Virtual Network| Sí | Se aplica solo a las [instancias administradas](/azure/sql-database/sql-database-managed-instance). |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Firewall en el nivel de base de datos y de servidor. El aislamiento de red es solo para [instancias administradas](/azure/sql-database/sql-database-managed-instance). |
| Compatibilidad con la tunelización forzada| Sí | [Instancia administrada](/azure/sql-database/sql-database-managed-instance) mediante VPN de [ExpressRoute](/azure/expressroute/index). |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure, como Log Analytics o Application Insights| Sí | SecureSphere, la solución SIEM de Imperva, también se admite gracias a la integración de [Azure Event Hubs](/azure/event-hubs/index) mediante la [auditoría de SQL](/azure/sql-database/sql-database-auditing). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory (Azure AD) |
| Authorization| Sí | None |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Sí, pero solo para algunos eventos |
| Registro y auditoría del plano de datos | Sí | Mediante la [auditoría de SQL](/azure/sql-database/sql-database-auditing) |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración, como el control de versiones de configuración| Sin  | None |

### <a name="additional-security-attributes-for-sql-database"></a>Atributos de seguridad adicionales para SQL Database

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Preventivo: evaluación de vulnerabilidad | Sí | Consulte [El servicio de evaluación de vulnerabilidad de SQL le ayuda a identificar los puntos vulnerables de la base de datos](/azure/sql-database/sql-vulnerability-assessment). |
| Preventivo: detección y clasificación de datos  | Sí | Consulte [Clasificación y detección de datos de Azure SQL Database y Azure SQL Data Warehouse](/azure/sql-database/sql-database-data-discovery-and-classification). |
| Detección: detección de amenazas | Sí | Consulte [Advanced Threat Protection en Azure SQL Database](/azure/sql-database/sql-database-threat-detection-overview). |


## <a name="storageazurestoragecommonstorage-security-attributes"></a>[Storage](/azure/storage/common/storage-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual) | Sí | Compatible con los mecanismos estándar de HTTPS/TLS.  Los usuarios también pueden cifrar datos antes de que se transmitan al servicio. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sí | Consulte [Storage Service Encryption mediante claves administradas por el cliente en Azure Key Vault](/azure/storage/common/storage-service-encryption-customer-managed-keys?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí |  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| N/D |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | |
| Compatibilidad con la tunelización forzada| N/D |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Las métricas de Azure Monitor ya están disponibles ahora, los registros empiezan la versión preliminar |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory, clave compartida, el token de acceso compartido. |
| Authorization| Sí | Compatible con la autorización mediante RBAC, listas de control de acceso de POSIX y tokens de SAS |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control | Sí | Registro de actividad de Azure Resource Manager |
| Registro y auditoría del plano de datos| Sí | Los registros de diagnóstico del servicio y el registro de Azure Monitor empiezan la versión preliminar  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Compatible con el control de versiones del proveedor de recursos mediante las API de Azure Resource Manager |

## <a name="virtual-machines-and-virtual-machine-scale-sets"></a>Virtual Machines y Virtual Machine Scale Sets

[VM Linux](/azure/virtual-machines/windows/virtual-machines-windows-security-attributes) | [VM Windows](/azure/virtual-machines/windows/virtual-machines-windows-security-attributes) | [Conjuntos de escalado de máquinas virtuales](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-security-attributes)


### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | Consulte [Cifrado de una máquina virtual Linux en Azure](/azure/virtual-machines/linux/encrypt-disks) y [Cifrado de discos virtuales en una máquina virtual Windows](/azure/virtual-machines/windows/encrypt-disks). |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| Sí | Azure Virtual Machines admite [ExpressRoute](/azure/expressroute) y el cifrado de red virtual. Consulte [Cifrado en tránsito en máquinas virtuales](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sí | Las claves administradas por el cliente es un escenario de cifrado admitidos de Azure; consulte [Introducción al cifrado de Azure](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de HTTPS y SSL. |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | |
| Compatibilidad con la inserción de redes virtuales| Sí | . |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |
| Compatibilidad con la tunelización forzada| Sí | Consulte [Configuración de la tunelización forzada mediante el modelo de implementación de Azure Resource Manager](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Supervisión y actualización de una máquina virtual Linux en Azure](/azure/virtual-machines/linux/tutorial-monitoring) y [Supervisión y actualización de una máquina virtual Windows en Azure](/azure/virtual-machines/windows/tutorial-monitoring). |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí |  |
| Authorization| Sí |  |


### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos | Sin |  |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí |  | 


## <a name="vpn-gatewayazurevpn-gatewayvpn-gateway-security-attributes"></a>[VPN Gateway](/azure/vpn-gateway/vpn-gateway-security-attributes)

### <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, el cifrado del lado servidor, el cifrado del lado servidor con claves administradas por el cliente y otras características de cifrado) | N/D | Datos de clientes en tránsito de VPN Gateway, NO se almacenan los datos de los clientes |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, en el cifrado de red virtual y el cifrado de red virtual a red virtual)| Sí | Paquetes de clientes cifrados de VPN Gateway entre las instancias de Azure VPN Gateway y los clientes de VPN (P2S) o los dispositivos de VPN (S2S) locales de los clientes. Las instancias de Azure VPN Gateway también admiten el cifrado entre redes virtuales. |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sin | Las claves precompartidas especificadas por el cliente se cifran en reposo; pero aún no se integran con CMK. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Mediante [Azure Resource Manager](/azure/azure-resource-manager/index) y HTTPS  |

### <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D | |
| Compatibilidad con inserción de red virtual| N/D | . |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Las instancias de VPN Gateway son instancias de VM dedicadas para cada red virtual de cliente  |
| Compatibilidad con la tunelización forzada| Sí |  |

### <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Registros de diagnósticos de Azure Monitor/alerta](/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log) & [Métricas de Azure Monitor/alerta](/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric).  |

### <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) para administrar el servicio y configurar Azure VPN Gateway. |
| Authorization| Sí | Admitir la autorización a través de [RBAC](/azure/role-based-access-control/overview). |

### <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Registro de actividad de Azure Resource Manager. |
| Registro y auditoría del plano de datos | Sí | [Registros de diagnóstico de Azure Monitor](/azure/azure-resource-manager/resource-group-audit) para el registro y la auditoría de la conectividad de VPN. |

### <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Para las operaciones de administración, el estado de una configuración de Azure VPN Gateway se puede exportar como una plantilla de Azure Resource Manager y con control de versiones a lo largo del tiempo. | 

