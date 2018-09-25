---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 70128735aef64d273c63236a13b0ae28edb6077d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003931"
---
### <a name="sql-databases"></a>Instancias de SQL Database

|  |  |
|---------|---------|
| [SKU permitidas de SQL DB](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Requiere que las bases de datos de SQL Database usen una SKU aprobada. Se especifica una matriz de los identificadores de las SKU permitidas o una matriz de los nombres de las SKU que se permiten. |
| [Auditar la configuración de detección de amenazas en el nivel de base de datos](../articles/governance/policy/samples/audit-db-threat-det-setting.md) | Audita las directivas de alertas de seguridad de SQL Database si no están establecidas en un estado especificado. Se especifica un valor que indica si la detección de amenazas está habilitada o deshabilitada.  |
| [Auditar cifrado de base de datos SQL](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Audita si la base de datos SQL no tiene un cifrado de datos transparente habilitado. |
| [Auditar la configuración de auditoría en el nivel de SQL DB](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Audita la configuración de auditoría de SQL Database si esa configuración no coincide con una configuración especificada. Se especifica un valor que indica si la configuración de auditoría debe estar habilitada o deshabilitada.  |
| [Auditar el estado del cifrado de datos transparente](../articles/governance/policy/samples/audit-trans-data-enc-status.md) | Audita el cifrado de datos transparente de SQL Database si no está habilitado.  |