---
title: Detectar firewall integrado de Instancia administrada de Azure SQL Database| Microsoft Docs
description: Aprenda a comprobar la protección de firewall integrado en Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: c98b0fd5669140559b4840e157394c2e8c6086ae
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567442"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Comprobación del firewall integrado de Instancia administrada

Las [reglas de seguridad de entrada obligatorias](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) requieren la apertura de los puertos administrados 9000, 9003, 1438, 1440 o 1452 desde **Cualquier origen** en el grupo de seguridad de red (NSG) que protege la Instancia administrada. Aunque estos puertos están abiertos en el nivel de NSG, el firewall integrado los protege en el nivel de red.

## <a name="verify-firewall"></a>Comprobar el firewall

Para comprobar estos puertos, use cualquier herramienta de escáner de seguridad para probar estos puertos. La captura de pantalla siguiente muestra cómo usar una de estas herramientas.

![Comprobación del firewall integrado](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Instancias administradas y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL Database](sql-database-managed-instance-connectivity-architecture.md).