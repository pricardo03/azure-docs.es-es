---
title: Atributos de seguridad de Azure SQL Database
description: Lista de comprobación de los atributos de seguridad para evaluar Azure SQL Database
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 1318b3e433224b009b76458b12e82c9bcf94bb7a
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444390"
---
# <a name="security-attributes-for-azure-sql-database"></a>Atributos de seguridad de Azure SQL Database

En este artículo, se explican los atributos de seguridad que están integrados en Azure SQL Database.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

SQL Database incluye una [base de datos única](sql-database-single-index.yml) y una [instancia administrada](sql-database-managed-instance.md). Las entradas siguientes se aplican a ambas ofertas, excepto si se indica lo contrario.

## <a name="preventative"></a>Prevención

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado en reposo:<ul><li>Cifrado del servidor</li><li>Cifrado del servidor con claves administradas por el cliente</li><li>Otras características de cifrado (por ejemplo, del cliente o Always Encrypted)</ul>| Sí | Se denomina "cifrado en uso", tal y como se describe en el artículo [Always Encrypted](sql-database-always-encrypted.md). El cifrado del servidor usa el [cifrado de datos transparente](transparent-data-encryption-azure-sql.md).|
| Cifrado en tránsito:<ul><li>Cifrado de Azure ExpressRoute</li><li>Cifrado en una red virtual</li><li>Cifrado entre redes virtuales</ul>| Sí | Se usa HTTPS. |
| Control de claves de cifrado, como CMK o BYOK| Sí | Se ofrece control de claves tanto administradas por el servicio como por el cliente. Este último se ofrece mediante [Azure Key Vault](../key-vault/index.yml). |
| Cifrado de nivel de columna proporcionado por Azure Data Services| Sí | Mediante [Always Encrypted](sql-database-always-encrypted.md). |
| Llamadas a API cifradas| Sí | Con HTTPS/SSL. |

## <a name="network-segmentation"></a>Segmentación de red

| Atributo de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con los puntos de conexión de servicio| Sí | Se aplica solo a las [bases de datos únicas](sql-database-single-index.yml). |
| Compatibilidad con la inserción de Azure Virtual Network| Sí | Se aplica solo a las [instancias administradas](sql-database-managed-instance.md). |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Firewall en el nivel de base de datos y de servidor. El aislamiento de red es solo para [instancias administradas](sql-database-managed-instance.md). |
| Compatibilidad con la tunelización forzada| Sí | [Instancia administrada](sql-database-managed-instance.md) mediante VPN de [ExpressRoute](../expressroute/index.yml). |

## <a name="detection"></a>Detección

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure, como Log Analytics o Application Insights| Sí | SecureSphere, la solución SIEM de Imperva, también se admite gracias a la integración de [Azure Event Hubs](../event-hubs/index.yml) mediante la [auditoría de SQL](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | Azure Active Directory (Azure AD) |
| Authorization| Sí | None |

## <a name="audit-trail"></a>Pista de auditoría

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Registro y auditoría del plano de administración y de control| Sí | Sí, pero solo para algunos eventos |
| Registro y auditoría del plano de datos | Sí | Mediante la [auditoría de SQL](sql-database-auditing.md) |

## <a name="configuration-management"></a>Administración de configuración

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración, como el control de versiones de configuración| Sin  | None |

## <a name="additional-security-attributes-for-sql-database"></a>Atributos de seguridad adicionales para SQL Database

| Atributo de seguridad | Sí/No | Notas|
|---|---|--|
| Preventivo: evaluación de vulnerabilidad | Sí | Consulte [El servicio de evaluación de vulnerabilidad de SQL le ayuda a identificar los puntos vulnerables de la base de datos](sql-vulnerability-assessment.md). |
| Preventivo: detección y clasificación de datos  | Sí | Consulte [Clasificación y detección de datos de Azure SQL Database y Azure SQL Data Warehouse](sql-database-data-discovery-and-classification.md). |
| Detección: detección de amenazas | Sí | Consulte [Advanced Threat Protection en Azure SQL Database](sql-database-threat-detection-overview.md). |
