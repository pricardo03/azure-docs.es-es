---
title: Atributos de seguridad de Azure Service Fabric
description: Lista de comprobación de los atributos de seguridad para evaluar Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 23c7f8bdcf67d59ccdd5cd0b00bc0e0960ba1d8f
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443870"
---
# <a name="security-attributes-for-azure-service-fabric"></a>Atributos de seguridad de Azure Service Fabric

En este artículo, se documentan los atributos de seguridad integrados en Azure Service Fabric. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo (por ejemplo, cifrado en el servidor, cifrado en el servidor con claves administradas por el cliente y otras características de cifrado)| Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales en el que se basa el clúster. El cifrado de discos de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí |  |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales en el que se basa el clúster. El cifrado de discos de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Llamadas a API cifradas| Sí | Las llamadas de API de Service Fabric se hacen a través de Azure Resource Manager. Se requiere un token web de JSON (JWT) válido. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con los puntos de conexión de servicio| Sí |  |
| Compatibilidad con inserción de red virtual| Sí |  |
| Compatibilidad con aislamiento de red y firewalls| Sí | Usando grupos de seguridad de red (NSG). |
| Compatibilidad con la tunelización forzada| Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Utilizando la compatibilidad de supervisión de Azure y la compatibilidad de las soluciones de terceros. |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Administración de identidad y acceso (IAM) para llamadas a través de SFRP. Las llamadas directamente al punto de conexión del clúster son compatibles con dos roles: usuario y administrador. El cliente puede asignar las API a cualquiera de estos roles. |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Registro y auditoría del plano de datos| N/D | El cliente es propietario del clúster.  |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | |