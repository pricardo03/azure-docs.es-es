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
ms.openlocfilehash: dabadf3a4175947f09ba20b0e644f1d2b485ee38
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73591419"
---
### <a name="sql-servers"></a>Servidores SQL Server

|  |  |
|---------|---------|
| [Auditar que no haya un administrador de Azure Active Directory](../articles/governance/policy/samples/audit-no-aad-admin.md) | Audite cuándo no hay administrador de Azure Active Directory asignado al servidor SQL. |
| [Auditar la configuración de detección de amenazas en el nivel de servidor](../articles/governance/policy/samples/audit-sql-server-threat-detection-setting.md) | Audita las directivas de alertas de seguridad de SQL Database si no están establecidas en un estado especificado. Se especifica un valor que indica si la detección de amenazas está habilitada o deshabilitada.  |
| [Auditar la configuración de auditoría de SQL Server](../articles/governance/policy/samples/sql-server-audit.md) | Audita SQL Server en función de si la configuración de auditoría está habilitada. |
| [Auditar la configuración de auditoría en el nivel de SQL Server](../articles/governance/policy/samples/audit-sql-server-audit-setting.md) | Audita la configuración de auditoría de SQL Server si esa configuración no coincide con una configuración especificada. Se especifica un valor que indica si la configuración de auditoría debe estar habilitada o deshabilitada. |