---
title: Atributos comunes de seguridad para Azure Service Fabric
description: Lista de comprobación de los atributos de seguridad que se utilizan habitualmente para evaluar Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: f6db40a35c289fa0870ac4c9e5e55b773c84f4f4
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679234"
---
# <a name="common-security-attributes-for-azure-service-fabric"></a>Atributos comunes de seguridad para Azure Service Fabric

La seguridad está integrada en todos los aspectos de los servicios de Azure. Este artículo documenta los atributos comunes de seguridad integrados en Azure Service Fabric. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | El cliente posee el clúster y el conjunto de escalado en que se crea el clúster. Cifrado de disco de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí |  |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sí | El cliente posee el clúster y el conjunto de escalado en que se crea el clúster. Cifrado de disco de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Las llamadas de API de Service Fabric se hacen a través de Azure Resource Manager. Se requiere un token web de JSON (JWT) válido. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de red virtual| Sí |  |
| Compatibilidad con aislamiento de red y firewalls| Sí | Usando grupos de seguridad de red (NSG). |
| Compatibilidad con la tunelización forzada | Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Uso de soporte técnico y soporte técnico de terceros de supervisión de Azure. |

## <a name="iam-support"></a>Compatibilidad con IAM

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Administración del acceso: autenticación| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Administración del acceso: autorización| Sí | Administración de identidad y acceso (IAM) para llamadas a través de SFRP. Las llamadas directamente al punto de conexión del clúster son compatibles con dos roles: usuario y administrador. El cliente puede asignar las API a cualquiera de estos roles. |


## <a name="audit-trail"></a>Registro de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Registro y auditoría del plano de datos| N/D | El cliente es propietario del clúster.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | El control de versiones y la implementación de la configuración del servicio se hacen mediante Azure Deploy. El control de versiones del código (aplicación y tiempo de ejecución) se hace mediante Azure Build.