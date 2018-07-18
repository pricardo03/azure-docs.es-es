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
ms.openlocfilehash: 298b92205e73b3623db02fb1dd803edac8379700
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664704"
---
### <a name="sql-servers"></a>Servidores SQL Server

|  |  |
|---------|---------|
| [Auditar que no haya un administrador de Azure Active Directory](../articles/azure-policy/scripts/audit-no-aad-admin.md) | Audite cuándo no hay administrador de Azure Active Directory asignado al servidor SQL. |
| [Auditar la configuración de detección de amenazas en el nivel de servidor](../articles/azure-policy/scripts/audit-sql-ser-threat-det-setting.md) | Audita las directivas de alertas de seguridad de SQL Database si no están establecidas en un estado especificado. Se especifica un valor que indica si la detección de amenazas está habilitada o deshabilitada.  |
| [Auditar la configuración de auditoría de SQL Server](../articles/azure-policy/scripts/sql-server-audit.md) | Audita SQL Server en función de si la configuración de auditoría está habilitada. |
| [Auditar la configuración de auditoría en el nivel de SQL Server](../articles/azure-policy/scripts/audit-sql-ser-leve-audit-setting.md) | Audita la configuración de auditoría de SQL Server si esa configuración no coincide con una configuración especificada. Se especifica un valor que indica si la configuración de auditoría debe estar habilitada o deshabilitada. |
| [Requerir SQL Server 12.0](../articles/azure-policy/scripts/req-sql-12.md) | Requiere que los servidores de SQL Server usen la versión 12.0.  |