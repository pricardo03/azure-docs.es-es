---
title: Atributos de seguridad de base de datos de SQL Azure
description: Una lista de comprobación de los atributos de seguridad para evaluar la base de datos de SQL Azure
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: df1ffa07c9b813ee3da4952bbcc394f43c69b7ac
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204234"
---
# <a name="security-attributes-for-azure-sql-database"></a>Atributos de seguridad de base de datos de SQL Azure

Este artículo documenta los atributos comunes de seguridad integrados en Azure SQL Database.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

Azure SQL Database incluye tanto [base de datos única](sql-database-single-index.yml) y [instancia administrada](sql-database-managed-instance.md). Las siguientes entradas se aplican a ambas ofertas, excepto donde se indique lo contrario.

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente, siempre cifrado, etc.)</ul>| Sí | Conoce como "cifrado en uso", como se describe en el artículo [Always Encrypted](sql-database-always-encrypted.md). Cifrado del lado del servicio usa [cifrado de datos transparente](transparent-data-encryption-azure-sql.md) (TDE).|
| Cifrado en tránsito:<ul><li>Cifrado de ExpressRoute</li><li>Cifrado en red virtual</li><li>Cifrado de red virtual a red virtual</ul>| Sí | Se usa HTTPS. |
| Control de clave de cifrado (CMK, BYOK, etc.)| Sí | Se ofrecen control de claves administradas del servicio tanto administradas por el cliente (a través de este último [Azure Key Vault](../key-vault/index.yml). |
| Cifrado de nivel de columna (Azure Data Services)| Sí | A través de [Always Encrypted](sql-database-always-encrypted.md). |
| Llamadas a API cifradas| Sí | Uso de HTTPS/SSL. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí | Se aplica a [base de datos única](sql-database-single-index.yml) solo. |
| Compatibilidad con inserción de red virtual| Sí | Se aplica a [instancia administrada](sql-database-managed-instance.md) solo. |
| Compatibilidad con aislamiento de red y firewalls| Sí | En ambas bases de datos - y nivel de servidor; de Firewall aislamiento de red [instancia administrada](sql-database-managed-instance.md) solo |
| Compatibilidad con tunelización forzada | Sí | [instancia administrada](sql-database-managed-instance.md) a través de [Azure ExpressRoute](../expressroute/index.yml) VPN |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Supervisión de soporte técnico (Log analytics, Application insights, etcetera) de Azure| Sí | La solución SIEM de terceros de Imperva (SecureSphere) también se admite, a través [Azure Event Hubs](../event-hubs/index.yml) integración a través de [auditoría SQL](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Administración del acceso: autenticación| Sí | Azure Active Directory. |
| Administración del acceso: autorización| Sí |  |


## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría de planificación de control/administración| Sí | Sí, solo algunos eventos. |
| Registro y auditoría del plano de datos | Sí | A través de [auditoría SQL](sql-database-auditing.md). |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etcetera.)| Sin   | | 

## <a name="additional-security-attributes-for-sql-database"></a>Atributos de seguridad adicionales para la base de datos de SQL

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Preventivas: evaluación de vulnerabilidad | Sí | Consulte [evaluación de vulnerabilidad de SQL service le ayuda a identificar las vulnerabilidades de la base de datos](sql-vulnerability-assessment.md). |
| Preventivas: clasificación y detección de datos  | Sí | Consulte [clasificación y detección de datos de Azure SQL Database y SQL Data Warehouse](sql-database-data-discovery-and-classification.md). |
| Detección: detección de amenazas | Sí | Consulte [protección contra amenazas para Azure SQL Database avanzada](sql-database-threat-detection-overview.md). |
