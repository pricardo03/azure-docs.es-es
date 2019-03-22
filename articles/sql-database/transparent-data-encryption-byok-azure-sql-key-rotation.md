---
title: 'PowerShell: Rotación del protector de TDE en Azure SQL Database | Microsoft Docs'
description: Aprenda a rotar el protector de Cifrado de datos transparente (TDE) para un servidor SQL de Azure.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: jhubbard
ms.date: 03/12/2019
ms.openlocfilehash: 760b292e75b4cc64b85eaf51ffad0521b721dabf
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57895658"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>Rotación del protector de Cifrado de datos transparente (TDE) mediante PowerShell

En este artículo se describe la rotación de claves para un servidor SQL de Azure mediante un protector de TDE de Azure Key Vault. Rotar el protector de TDE de un servidor SQL de Azure significa cambiar a una nueva clave asimétrica que protege las bases de datos en el servidor. La rotación de claves es una operación en línea y solo debería tardar unos segundos en completarse, ya que únicamente descifra y vuelve a cifrar la clave de cifrado de los datos de la base de datos, no en toda la base de datos.

En esta guía se describen dos opciones para rotar el protector de TDE en el servidor.

> [!NOTE]
> Debe reanudarse un almacén de datos SQL en pausa antes de las rotaciones de claves.
>

> [!IMPORTANT]
> **No elimine** las versiones anteriores de la clave después de una sustitución.  Cuando las claves se sustituyen, algunos datos siguen cifrados con las claves anteriores como, por ejemplo, las copias de seguridad de base de datos más antiguas. 
>

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

- Esta guía de procedimientos asume que ya utiliza una clave de Azure Key Vault como protector TDE para una base de datos Azure SQL Database o Data Warehouse. Consulte [Cifrado de datos transparente con compatibilidad con BYOK](transparent-data-encryption-byok-azure-sql.md).
- Debe tener Azure PowerShell instalado y ejecutándose.
- [Recomendado pero opcional] Cree primero el material de clave para el protector de TDE en un módulo de seguridad de hardware (HSM) o en un almacén local de claves e importe el material de clave a Azure Key Vault. Siga las [instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) para más información.

## <a name="manual-key-rotation"></a>Rotación manual de claves

Rotación de claves manual se usa el [agregar AzKeyVaultKey](/powershell/module/az.keyvault/Add-AzKeyVaultKey), [agregar AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey), y [conjunto AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets para agregar un clave totalmente nueva, que podría tener un nombre nuevo o incluso otro almacén de claves. Usar este enfoque permite agregar la misma clave a distintos almacenes de claves para admitir escenarios de alta disponibilidad y recuperación ante desastres geográfica.

>[!NOTE]
>La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.

   ```powershell
   # Add a new key to Key Vault
   Add-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
  
   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

## <a name="option-2-manual-rotation"></a>Opción 2: Rotación manual

La opción usa la [agregar AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey), [agregar AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey), y [conjunto AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets para agregar un completamente nueva clave, que podría tener un nombre nuevo o incluso otro almacén de claves. 

>[!NOTE]
>La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Otros cmdlets útiles de PowerShell

- Para cambiar el protector del TDE desde administrados por Microsoft al modo BYOK, use el [conjunto AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Para cambiar el protector del TDE del modo BYOK a administrada por Microsoft, use el [conjunto AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Pasos siguientes

- En caso de riesgo de seguridad, aprenda a quitar un protector de TDE potencialmente comprometido: [Eliminación de una clave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Introducción a la integración de Azure Key Vault y compatibilidad con Bring Your Own Key para TDE: [Activar TDE con su propia clave de Key Vault mediante PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
