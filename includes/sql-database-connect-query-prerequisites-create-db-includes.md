---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 01/28/2019
ms.author: genemi
ms.openlocfilehash: edeaa932abe4d1882f957d0ed26aaddd97dabe3f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198068"
---
<!-- sql-database-connect-query-prerequisites-create-db-includes.md -->

- Una instancia de Azure SQL Database colocada en un [servidor lógico](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) o en una [Instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index). Puede utilizar una de estas técnicas para crear una base de datos:

| Servidor lógico | Instancia administrada |
| --- | --- |
| [Portal](../articles/sql-database/sql-database-get-started-portal.md) | [Portal](../articles/sql-database/sql-database-managed-instance-get-started.md) |
| [CLI](../articles/sql-database/sql-database-get-started-cli.md) | |
| [PowerShell](../articles/sql-database/sql-database-get-started-powershell.md) | |

- **Solo servidor lógico**: una regla de firewall de nivel de servidor configurada que le permite conectarse a su servidor lógico. Para más información, consulte [Creación de una regla de firewall de nivel de servidor](../articles/sql-database/sql-database-get-started-portal-firewall.md).
- **Solo Instancia administrada**: conexión configurada desde el equipo que accede a Instancia administrada. Puede usar las siguientes opciones:
  - Una [máquina virtual de Azure](../articles/sql-database/sql-database-managed-instance-configure-vm.md) en la misma red virtual de Azure que Instancia administrada que pueda acceder a la instancia.
  - Una [conexión de punto a sitio](../articles/sql-database/sql-database-managed-instance-configure-p2s.md) en el equipo que le permitirá a unir el equipo a la red virtual en la que se coloca Instancia administrada y usar Instancia administrada como cualquier otro servidor con SQL Server de la red.
