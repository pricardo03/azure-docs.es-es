---
title: Ventaja híbrida de Azure
description: Use licencias de SQL Server existentes para obtener descuentos en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
ms.openlocfilehash: 4aab699ff0141b648066fae0de445c4467be7509
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048389"
---
# <a name="azure-hybrid-benefit"></a>Ventaja híbrida de Azure

En el nivel de proceso aprovisionado del modelo de compra basado en núcleo virtual, puede intercambiar sus licencias existentes por tarifas de descuento en SQL Database mediante la [Ventaja híbrida de Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Esta ventaja de Azure le permite ahorrar hasta un 30 % en Azure SQL Database al utilizar las licencias de SQL Server locales con Software Assurance. 

> [!NOTE]
> Cambiar a Ventaja híbrida de Azure no requiere ningún tiempo de inactividad.

![Precios](./media/sql-database-service-tiers/pricing.png)

## <a name="choose-a-license-model"></a>Elija un modelo de licencia

Con la Ventaja híbrida de Azure, puede elegir pagar solo por la infraestructura subyacente de Azure mediante el uso de la licencia existente de SQL Server para el motor de base de datos SQL (precios de proceso básicos), o pagar tanto por la infraestructura subyacente como por la licencia de SQL Server (precio con licencia incluida).

Puede elegir o cambiar el modelo de licencia mediante Azure Portal o con una de las siguientes API:

- Para establecer o actualizar el tipo de licencia mediante PowerShell:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)

- Para establecer o actualizar el tipo de licencia mediante la CLI de Azure:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Para establecer o actualizar el tipo de licencia mediante la API REST:

  - [Databases - Create Or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Managed Instances - Create Or Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="next-steps"></a>Pasos siguientes

- Para elegir entre las opciones de implementación de SQL Database, consulte [Elegir la opción de implementación adecuada en Azure SQL](sql-database-paas-vs-sql-server-iaas.md).
- Para obtener una comparación de las características de SQL Database, consulte [Características de Azure SQL Database](sql-database-features.md).
