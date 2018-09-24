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
ms.openlocfilehash: 76e161be1adca4aa2ec7b682a13b0a42e4b79412
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003912"
---
### <a name="sql-servers"></a>Servidores SQL Server

|  |  |
|---------|---------|
| [Auditar que no haya un administrador de Azure Active Directory](../articles/governance/policy/samples/audit-no-aad-admin.md) | Audite cuándo no hay administrador de Azure Active Directory asignado al servidor SQL. |
| [Auditar la configuración de detección de amenazas en el nivel de servidor](../articles/governance/policy/samples/audit-sql-ser-threat-det-setting.md) | Audita las directivas de alertas de seguridad de SQL Database si no están establecidas en un estado especificado. Se especifica un valor que indica si la detección de amenazas está habilitada o deshabilitada.  |
| [Auditar la configuración de auditoría de SQL Server](../articles/governance/policy/samples/sql-server-audit.md) | Audita SQL Server en función de si la configuración de auditoría está habilitada. |
| [Auditar la configuración de auditoría en el nivel de SQL Server](../articles/governance/policy/samples/audit-sql-ser-leve-audit-setting.md) | Audita la configuración de auditoría de SQL Server si esa configuración no coincide con una configuración especificada. Se especifica un valor que indica si la configuración de auditoría debe estar habilitada o deshabilitada. |
| [Requerir SQL Server 12.0](../articles/governance/policy/samples/req-sql-12.md) | Requiere que los servidores de SQL Server usen la versión 12.0.  |