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
ms.openlocfilehash: 7c1718298c3f7c3fea28fa0b18569085f071696f
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003054"
---
# <a name="security-attributes-for-azure-service-fabric"></a>Atributos de seguridad para Azure Service Fabric

En este artículo describe los atributos de seguridad integrados en Azure Service Fabric. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | El cliente posee el clúster y el conjunto de escalado en que se crea el clúster. Cifrado de disco de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>En el cifrado de red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí |  |
| Control de claves de cifrado (CMK, BYOK, etcetera.)| Sí | El cliente posee el clúster y el conjunto de escalado en que se crea el clúster. Cifrado de disco de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Las llamadas de API de Service Fabric se hacen a través de Azure Resource Manager. Se requiere un token web de JSON (JWT) válido. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad de punto de conexión de servicio| Sí |  |
| Compatibilidad con inserción de redes virtuales| Sí |  |
| Aislamiento de red y la compatibilidad con Firewall| Sí | Usando grupos de seguridad de red (NSG). |
| Fuerza la tunelización de soporte técnico| Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | Uso de soporte técnico y soporte técnico de terceros de supervisión de Azure. |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Autorización| Sí | Administración de identidad y acceso (IAM) para llamadas a través de SFRP. Las llamadas directamente al punto de conexión del clúster son compatibles con dos roles: usuario y administrador. El cliente puede asignar las API a cualquiera de estos roles. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Auditoría y registro del plano de control y administración| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Auditoría y registro del plano de datos| N/D | El cliente es propietario del clúster.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sí | |