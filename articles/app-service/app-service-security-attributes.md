---
title: Atributos de seguridad de Azure App Service
description: Lista de comprobación de los atributos de seguridad para evaluar Azure App Service
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: 299262610c027529749840720f46d6dc97a07b21
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442272"
---
# <a name="security-attributes-for-azure-app-service"></a>Atributos de seguridad de Azure App Service

En este artículo, se explican los atributos de seguridad integrados en Azure App Service.

[!INCLUDE [Security attributes header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado) | Sí | El contenido de los archivos de sitio web se almacena en Azure Storage, que cifra automáticamente el contenido en reposo. Consulte [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md).<br><br>Los secretos suministrados por el cliente se cifran en reposo. Los secretos se cifran en reposo mientras se almacenan en las bases de datos de configuración de App Service.<br><br>Los sitios web pueden utilizar los discos conectados localmente como almacenamiento temporal (D:\local y %TMP%). Los discos conectados localmente no se cifran en reposo. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Los clientes pueden configurar los sitios web para que exijan y usen HTTPS para el tráfico entrante. Consulte la entrada de blog [How to make an Azure App Service HTTPS only](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (Cómo hacer que Azure App Service use solo HTTPS). |
| Control de claves de cifrado (CMK, BYOK, etc.)| Sí | Los clientes pueden elegir almacenar secretos de aplicación en Key Vault y recuperarlos en tiempo de ejecución. Consulte [Uso de referencias de Key Vault para App Service y Azure Functions (versión preliminar)](app-service-key-vault-references.md).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Las llamadas de administración para configurar App Service se realizan mediante llamadas de [Azure Resource Manager](../azure-resource-manager/index.yml) con HTTPS. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con el punto de conexión de servicio| Sí | Disponible actualmente en versión preliminar para App Service. Consulte [Restricciones de acceso de Azure App Service](app-service-ip-restrictions.md). |
| Compatibilidad con la inserción de redes virtuales| Sí | App Service Environment son implementaciones privadas de App Service dedicadas a un solo cliente insertado en una red virtual del cliente. Consulte [Introducción a App Service Environment](environment/intro.md). |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | En la variación pública de varios inquilinos de App Service, los clientes pueden configurar las ACL (restricciones de IP) para bloquear el tráfico entrante permitido.  Consulte [Restricciones de acceso de Azure App Service](app-service-ip-restrictions.md).  App Service Environment se implementa directamente en las redes virtuales y, por tanto, se puede proteger con NSG. |
| Compatibilidad con la tunelización forzada| Sí | App Service Environment se puede implementar en la red virtual de un cliente que tenga configurada la tunelización forzada. Los clientes tienen que seguir las instrucciones de [Configuración de App Service Environment con tunelización forzada](environment/forced-tunnel-support.md). |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | App Service se integra con Application Insights para los lenguajes compatibles con Application Insights (edición completa de .NET Framework, .NET Core, Java y Node.JS).  Consulte [Supervisar el rendimiento de Azure App Service](../azure-monitor/app/azure-web-apps.md). App Service también envía las métricas de aplicación a Azure Monitor. Consulte [Supervisión de aplicaciones en Azure App Service](web-sites-monitor.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Los clientes pueden crear aplicaciones en App Service que se integren automáticamente con [Azure Active Directory (Azure AD)](../active-directory/index.yml), así como otros proveedores de identidades compatibles con OAuth; consulte [Autenticación y autorización en Azure App Service](overview-authentication-authorization.md). Para el acceso de administración a los recursos de App Service, todo el acceso se controla mediante una combinación de los roles RBAC de Azure Resource Manager y de las entidades de seguridad autenticadas de Azure AD. |
| Authorization| Sí | Para el acceso de administración a los recursos de App Service, todo el acceso se controla mediante una combinación de los roles RBAC de Azure Resource Manager y de las entidades de seguridad autenticadas de Azure AD.  |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Todas las operaciones de administración realizadas en objetos de App Service se producen a través de [Azure Resource Manager](../azure-resource-manager/index.yml). Los registros históricos de estas operaciones están disponibles tanto en el portal como a través de la CLI; consulte [Operaciones del proveedor de recursos de Azure Resource Manager](../role-based-access-control/resource-provider-operations.md#microsoftweb) y [az monitor activity-log](/cli/azure/monitor/activity-log). |
| Registro y auditoría del plano de datos | Sin | El plano de datos de App Service es un recurso compartido de archivos remoto que contiene el contenido del sitio web implementado de un cliente.  No hay ninguna auditoría del recurso compartido de archivos remoto. |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Para las operaciones de administración, el estado de una configuración de App Service se puede exportar como plantilla de Azure Resource Manager y con control de versiones a lo largo del tiempo. Para las operaciones en tiempo de ejecución, los clientes pueden mantener versiones diferentes en vivo de una aplicación con la característica de espacios de implementación de App Service. | 

