---
title: 'PowerShell y CLI: Habilitación de TDE de SQL con la propia clave, en Azure SQL Database | Microsoft Docs'
description: Aprenda a configurar una base de datos de Azure SQL Database y Data Warehouse para comenzar a usar Cifrado de datos transparente (TDE) para cifrado en reposo mediante PowerShell o CLI.
services: sql-database
keywords: ''
documentationcenter: ''
author: aliceku
manager: craigg
editor: ''
ms.prod: ''
ms.reviewer: carlrab
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.tgt_pltfrm: ''
ms.devlang: azurecli, powershell
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 0dd0d322bacd807dc0cc59aeeb854c427a745bca
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043565"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-using-your-own-key-from-azure-key-vault"></a>PowerShell y CLI: Habilitación de Cifrado de datos transparente con la propia clave desde Azure Key Vault

En este artículo se explica cómo usar una clave de Azure Key Vault para Cifrado de datos transparente (TDE) en una base de datos SQL Database o en Data Warehouse. Para más información sobre TDE con compatibilidad con Bring Your Own Key (BYOK), visite [BYOK de TDE con Azure SQL](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>Requisitos previos para PowerShell

- Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
- [Recomendado, pero opcional] Debe tener un módulo de seguridad de hardware (HSM) o un almacén de claves locales para crear una copia local del material de claves del protector de TDE.
- Es preciso tener instalada y en ejecución la versión 4.2.0 o más reciente de Azure PowerShell. 
- Cree una instancia de Azure Key Vault y una clave para usar para TDE.
   - [Instrucciones de PowerShell desde Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started)
   - [Instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started#a-idhsmaif-you-want-to-use-a-hardware-security-module-hsm)
 - El almacén de claves debe tener la siguiente propiedad que se usará para TDE:
   - [soft-delete](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
   - [Uso de la eliminación temporal de Key Vault con PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) 
- La clave debe tener los siguientes atributos que se usarán para TDE:
   - Sin fecha de expiración
   - No deshabilitado
   - Poder realizar las operaciones *get*, *wrap key*, *unwrap key*

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>Paso 1. Asignar una identidad de Azure AD al servidor 

Si ya tiene un servidor, use lo siguiente para agregar una identidad de Azure AD al servidor:

   ```powershell
   $server = Set-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Si va a crear un servidor, utilice el cmdlet [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) con la etiqueta -Identity para agregar una identidad de Azure AD durante la creación del servidor:

   ```powershell
   $server = New-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Paso 2. Conceder permisos de Key Vault al servidor

Utilice el cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) para conceder al servidor acceso al almacén de claves antes de usar una clave para TDE.

   ```powershell
   Set-AzureRmKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Paso 3. Agregar la clave de Key Vault al servidor y establecer el protector de TDE

- Utilice el cmdlet [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) para agregar la clave del almacén de claves al servidor.
- Utilice el cmdlet [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) para establecer la clave como protector de TDE para todos los recursos del servidor.
- Utilice el cmdlet [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) para confirmar que el protector de TDE se ha configurado según lo previsto.

> [!Note]
> La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.
> 

>[!Tip]
>KeyId de ejemplo desde Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>Paso 4 Activar TDE 

Utilice el cmdlet [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) para activar el TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

Ahora, la base de datos o el almacén de datos tienen TDE habilitado con una clave de cifrado en Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Paso 5. Comprobar el estado de cifrado y la actividad de cifrado

Utilice el cmdlet [Get-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) para obtener el estado de cifrado y el cmdlet [Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) para comprobar el progreso de cifrado para un almacén de datos o una base de datos.

   ```powershell
   # Get the encryption state
   Get-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Otros cmdlets útiles de PowerShell

- Utilice el cmdlet [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) para desactivar TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Utilice el cmdlet [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) para devolver la lista de claves de Key Vault agregadas al servidor.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Utilice el cmdlet [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) para quitar una clave de Key Vault del servidor.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>solución de problemas

Compruebe lo siguiente si se produce un problema:
- Si no se encuentra el almacén de claves, asegúrese de que está en la suscripción correcta mediante el cmdlet [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

   ```powershell
   Get-AzureRmSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Si la nueva clave no se puede agregar al servidor, o la nueva clave no se puede actualizar como protector de TDE, compruebe lo siguiente:
   - La clave no debe tener una fecha de expiración
   - La clave debe tener las operaciones *get*, *wrap key* y *unwrap key*.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a rotar el protector de TDE de un servidor para cumplir con los requisitos de seguridad: [Rotación del protector de Cifrado de datos transparente con PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- En caso de riesgo de seguridad, aprenda a quitar un protector de TDE potencialmente comprometido: [Eliminación de una clave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Requisitos previos para la CLI

- Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
- [Recomendado, pero opcional] Debe tener un módulo de seguridad de hardware (HSM) o un almacén de claves locales para crear una copia local del material de claves del protector de TDE.
- Versión 2.0 de la interfaz de la línea de comandos o posterior. Para instalar la última versión y conectarla a su suscripción de Azure, consulte el artículo sobre la [instalación y configuración de la interfaz de la línea de comandos 2.0 entre plataformas de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
- Cree una instancia de Azure Key Vault y una clave para usar para TDE.
   - [Administración de Key Vault mediante CLI 2.0](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2)
   - [Instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started#a-idhsmaif-you-want-to-use-a-hardware-security-module-hsm)
 - El almacén de claves debe tener la siguiente propiedad que se usará para TDE:
   - [soft-delete](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
   - [Uso de la eliminación temporal de Key Vault con la CLI](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli) 
- La clave debe tener los siguientes atributos que se usarán para TDE:
   - Sin fecha de expiración
   - No deshabilitado
   - Poder realizar las operaciones *get*, *wrap key*, *unwrap key*
   
## <a name="step-1-create-a-server-and-assign-an-azure-ad-identity-to-your-server"></a>Paso 1. Crear un servidor y asignar una identidad de Azure AD al servidor
      cli
      # create server (with identity) and database
      az sql server create -n "ServerName" -g "ResourceGroupName" -l "westus" -u "cloudsa" -p "YourFavoritePassWord99@34" -I 
      az sql db create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 
      

 
## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Paso 2. Conceder permisos de Key Vault al servidor
      cli
      # create key vault, key and grant permission
      az keyvault create -n "VaultName" -g "ResourceGroupName" 
      az keyvault key create -n myKey -p software --vault-name "VaultName" 
      az keyvault set-policy -n "VaultName" --object-id "ServerIdentityObjectId" -g "ResourceGroupName" --key-permissions wrapKey unwrapKey get list 
      

 
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Paso 3. Agregar la clave de Key Vault al servidor y establecer el protector de TDE
  
     cli
     # add server key and update encryption protector
      az sql server key create -g "ResourceGroupName" -s "ServerName" -t "AzureKeyVault" -u "FullVersionedKeyUri 
      az sql server tde-key update -g "ResourceGroupName" -s "ServerName" -t AzureKeyVault -u "FullVersionedKeyUri" 
      
  
  > [!Note]
> La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.
> 

>[!Tip]
>KeyId de ejemplo desde Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
  
## <a name="step-4-turn-on-tde"></a>Paso 4 Activar TDE 
      cli
      # enable encryption
      az sql db tde create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" --status Enabled 
      

Ahora, la base de datos o el almacén de datos tienen TDE habilitado con una clave de cifrado en Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Paso 5. Comprobar el estado de cifrado y la actividad de cifrado

     cli
      # get encryption scan progress
      az sql db tde show-activity -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

      # get whether encryption is on or off
      az sql db tde show-configuration -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

## <a name="sql-cli-references"></a>Referencias de la CLI de SQL

https://docs.microsoft.com/cli/azure/sql?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/tde-key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/db/tde?view=azure-cli-latest 

