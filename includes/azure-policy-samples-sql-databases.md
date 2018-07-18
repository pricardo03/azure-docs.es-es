---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 50a6388a95bce14bc9af7e0c9a5387e148f6fa73
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664714"
---
### <a name="sql-databases"></a>Instancias de SQL Database

|  |  |
|---------|---------|
| [SKU permitidas de SQL DB](../articles/azure-policy/scripts/allowed-sql-db-skus.md) | Requiere que las bases de datos de SQL Database usen una SKU aprobada. Se especifica una matriz de los identificadores de las SKU permitidas o una matriz de los nombres de las SKU que se permiten. |
| [Auditar la configuración de detección de amenazas en el nivel de base de datos](../articles/azure-policy/scripts/audit-db-threat-det-setting.md) | Audita las directivas de alertas de seguridad de SQL Database si no están establecidas en un estado especificado. Se especifica un valor que indica si la detección de amenazas está habilitada o deshabilitada.  |
| [Auditar cifrado de base de datos SQL](../articles/azure-policy/scripts/sql-database-encryption-audit.md) | Audita si la base de datos SQL no tiene un cifrado de datos transparente habilitado. |
| [Auditar la configuración de auditoría en el nivel de SQL DB](../articles/azure-policy/scripts/audit-sql-db-audit-setting.md) | Audita la configuración de auditoría de SQL Database si esa configuración no coincide con una configuración especificada. Se especifica un valor que indica si la configuración de auditoría debe estar habilitada o deshabilitada.  |
| [Auditar el estado del cifrado de datos transparente](../articles/azure-policy/scripts/audit-trans-data-enc-status.md) | Audita el cifrado de datos transparente de SQL Database si no está habilitado.  |