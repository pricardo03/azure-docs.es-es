---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/29/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 314f29a1135e355597e890b72ef3c0b7372194e6
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227252"
---
### <a name="sql-databases"></a>Instancias de SQL Database

|  |  |
|---------|---------|
| [SKU permitidas de SQL DB](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Requiere que las bases de datos de SQL Database usen una SKU aprobada. Se especifica una matriz de los identificadores de las SKU permitidas o una matriz de los nombres de las SKU que se permiten. |
| [Auditar la configuración de detección de amenazas en el nivel de base de datos](../articles/governance/policy/samples/audit-db-threat-det-setting.md) | Audita las directivas de alertas de seguridad de SQL Database si no están establecidas en un estado especificado. Se especifica un valor que indica si la detección de amenazas está habilitada o deshabilitada.  |
| [Auditar cifrado de base de datos SQL](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Audita si la base de datos SQL no tiene un cifrado de datos transparente habilitado. |
| [Auditar la configuración de auditoría en el nivel de SQL DB](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Audita la configuración de auditoría de SQL Database si esa configuración no coincide con una configuración especificada. Se especifica un valor que indica si la configuración de auditoría debe estar habilitada o deshabilitada.  |