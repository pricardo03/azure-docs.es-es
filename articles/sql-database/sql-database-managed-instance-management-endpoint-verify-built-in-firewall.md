---
title: Detectar firewall integrado de Instancia administrada de Azure SQL Database| Microsoft Docs
description: Aprenda a comprobar la protección de firewall integrado en Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 12/04/2018
ms.date: 12/31/2018
ms.openlocfilehash: 774455a2901782ef52b213c6a13c17636e28b1a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699651"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Comprobación del firewall integrado de Instancia administrada

Las [reglas de seguridad de entrada obligatorias](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) requieren la apertura de los puertos administrados 9000, 9003, 1438, 1440 o 1452 desde **Cualquier origen** en el grupo de seguridad de red (NSG) que protege la Instancia administrada. Aunque estos puertos están abiertos en el nivel de NSG, el firewall integrado los protege en el nivel de red.

## <a name="verify-firewall"></a>Comprobar el firewall

Para comprobar estos puertos, use cualquier herramienta de escáner de seguridad para probar estos puertos. La captura de pantalla siguiente muestra cómo usar una de estas herramientas.

![Comprobación del firewall integrado](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Instancias administradas y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL Database](sql-database-managed-instance-connectivity-architecture.md).