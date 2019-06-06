---
title: 'PowerShell: Habilitar TDE con BYOK: Instancia administrada de Azure SQL Database| Microsoft Docs'
description: Aprenda a configurar una Instancia administrada de Azure SQL Database para comenzar a usar Cifrado de datos transparente (TDE) con BYOK para cifrado en reposo mediante PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 04/19/2019
ms.openlocfilehash: c2c4bd7bffd923430d0817cb6ea975f4c1596623
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729161"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault-preview"></a>Administrar el cifrado de datos transparente en una instancia administrada mediante su propia clave desde Azure Key Vault (versión preliminar)

En este ejemplo de script de PowerShell configura el cifrado de datos transparente (TDE) en el escenario de Bring Your Own Key (versión preliminar) para Azure SQL Managed Instance, con una clave desde Azure Key Vault. Para más información sobre TDE con compatibilidad con Bring Your Own Key (BYOK), vea [BYOK de TDE con Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Requisitos previos

- Una instancia administrada existente. Consulte [instancia administrada de uso de PowerShell para crear una base de datos de SQL Azure](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Con ambos PowerShell localmente o mediante Azure Cloud Shell requiere PowerShell AZ 1.1.1-Preview o una versión posterior de la versión preliminar. Si tiene que actualizar, vea [instalar Azure PowerShell module](/powershell/azure/install-az-ps), o ejecute el siguiente script de ejemplo para instalar el módulo.

`Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force`

Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="sample-scripts"></a>Scripts de ejemplo

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../sql-database-powershell-samples.md).
