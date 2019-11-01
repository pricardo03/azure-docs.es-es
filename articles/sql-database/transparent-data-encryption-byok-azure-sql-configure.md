---
title: 'PowerShell y CLI: Habilitación de SQL TDE - con Azure Key Vault - Traiga su propia clave - Azure SQL Database | Microsoft Docs'
description: Aprenda a configurar una base de datos de Azure SQL Database y Data Warehouse para comenzar a usar Cifrado de datos transparente (TDE) para cifrado en reposo mediante PowerShell o CLI.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
ms.date: 03/12/2019
ms.openlocfilehash: beeb5fa4f979ac457db8a779dd8f8f2e94ef87f5
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73163857"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell y CLI: Habilitación de Cifrado de datos transparente con una clave administrada por el cliente de Azure Key Vault

En este artículo se explica cómo usar una clave de Azure Key Vault para Cifrado de datos transparente (TDE) en una base de datos SQL Database o en Data Warehouse. Para obtener más información sobre TDE con la integración de Azure Key Vault - compatibilidad con Traiga su propia clave (BYOK), visite [TDE with customer-managed keys in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md) (TDE con claves administradas por el cliente de Azure Key Vault). 

## <a name="prerequisites-for-powershell"></a>Requisitos previos para PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager de PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo Az y en los módulos AzureRm son esencialmente idénticos.

- Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
- [Recomendado, pero opcional] Debe tener un módulo de seguridad de hardware (HSM) o un almacén de claves locales para crear una copia local del material de claves del protector de TDE.
- Es preciso tener instalado y en ejecución Azure PowerShell. 
- Cree una instancia de Azure Key Vault y una clave para usar para TDE.
  - [Instrucciones de PowerShell desde Key Vault](../key-vault/quick-create-powershell.md)
  - [Instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
    - El almacén de claves debe tener la siguiente propiedad que se usará para TDE:
  - [eliminación temporal](../key-vault/key-vault-ovw-soft-delete.md) y protección de purgas
  - [Uso de la eliminación temporal de Key Vault con PowerShell](../key-vault/key-vault-soft-delete-powershell.md) 
- La clave debe tener los siguientes atributos que se usarán para TDE:
   - Sin fecha de expiración
   - No deshabilitado
   - Poder realizar las operaciones *get*, *wrap key*, *unwrap key*

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>Paso 1. Asignar una identidad de Azure AD al servidor 

Si ya tiene un servidor, use lo siguiente para agregar una identidad de Azure AD al servidor:

   ```powershell
   $server = Set-AzSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Si va a crear un servidor, utilice el cmdlet [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) con la etiqueta -Identity para agregar una identidad de Azure AD durante la creación del servidor:

   ```powershell
   $server = New-AzSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Paso 2. Conceder permisos de Key Vault al servidor

Utilice el cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) para conceder al servidor acceso al almacén de claves antes de usar una clave para TDE.

   ```powershell
   Set-AzKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Paso 3. Agregar la clave de Key Vault al servidor y establecer el protector de TDE


- Use el cmdlet [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey?view=azps-2.4.0) para recuperar el identificador de clave del almacén de claves
- Utilice el cmdlet [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) para agregar la clave de Key Vault al servidor.
- Utilice el cmdlet [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) para establecer la clave como protector de TDE para todos los recursos del servidor.
- Utilice el cmdlet [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) para confirmar que el protector de TDE se ha configurado según lo previsto.

> [!Note]
> La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.
> 

>[!Tip]
>KeyId de ejemplo desde Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>Paso 4 Activar TDE 

Utilice el cmdlet [Set-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) para activar el TDE.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

Ahora, la base de datos o el almacén de datos tienen TDE habilitado con una clave de cifrado en Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Paso 5. Comprobar el estado de cifrado y la actividad de cifrado

Utilice el cmdlet [Get-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) para obtener el estado de cifrado y el cmdlet [Get-AzSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) para comprobar el progreso de cifrado para un almacén de datos o una base de datos.

   ```powershell
   # Get the encryption state
   Get-AzSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Otros cmdlets útiles de PowerShell

- Utilice el cmdlet [Set-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) para desactivar TDE.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled"
   ```
 
- Utilice el cmdlet [Get-AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) para devolver la lista de claves de Key Vault agregadas al servidor.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Utilice el cmdlet [Remove-AzSqlServerKeyVaultKey](/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) para quitar una clave de Key Vault del servidor.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>solución de problemas

Compruebe lo siguiente si se produce un problema:
- Si no se encuentra el almacén de claves, asegúrese de que está en la suscripción correcta mediante el cmdlet [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription).

   ```powershell
   Get-AzSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Si la nueva clave no se puede agregar al servidor, o la nueva clave no se puede actualizar como protector de TDE, compruebe lo siguiente:
   - La clave no debe tener una fecha de expiración
   - La clave debe tener las operaciones *get*, *wrap key* y *unwrap key*.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a cambiar el protector de TDE de un servidor para cumplir con los requisitos de seguridad: [Rotación del protector de Cifrado de datos transparente (TDE) mediante PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- En caso de riesgo de seguridad, aprenda a quitar un protector de TDE potencialmente comprometido: [Eliminación de una clave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Requisitos previos para la CLI

- Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
- [Recomendado, pero opcional] Debe tener un módulo de seguridad de hardware (HSM) o un almacén de claves locales para crear una copia local del material de claves del protector de TDE.
- Versión 2.0 de la interfaz de la línea de comandos o posterior. Para instalar la última versión y conectarla a su suscripción de Azure, consulte el artículo sobre la [instalación y configuración de la interfaz de la línea de comandos 2.0 entre plataformas de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 
- Cree una instancia de Azure Key Vault y una clave para usar para TDE.
  - [Administración de Key Vault mediante CLI 2.0](../key-vault/key-vault-manage-with-cli2.md)
  - [Instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
    - El almacén de claves debe tener la siguiente propiedad que se usará para TDE:
  - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
  - [Uso de la eliminación temporal de Key Vault con la CLI](../key-vault/key-vault-soft-delete-cli.md) 
- La clave debe tener los siguientes atributos que se usarán para TDE:
   - Sin fecha de expiración
   - No deshabilitado
   - Poder realizar las operaciones *get*, *wrap key*, *unwrap key*
   
## <a name="step-1-create-a-server-with-an-azure-ad-identity"></a>Paso 1. Creación de un servidor con una identidad de Azure AD
      cli
      # create server (with identity) and database
      az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
      az sql db create --name <dbname> --server <servername> --resource-group <rgname>  
 
 
>[!Tip]
>Mantenga el valor de "principalID" de la creación del servidor; es el identificador de objeto usado para asignar permisos de almacén de claves en el siguiente paso.
>
 
## <a name="step-2-grant-key-vault-permissions-to-the-logical-sql-server"></a>Paso 2. Concesión de permisos de Key Vault al servidor SQL lógico
      cli
      # create key vault, key and grant permission
       az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
       az keyvault key create --name <keyname> --vault-name <kvname> --protection software
       az keyvault set-policy --name <kvname>  --object-id <objectid> --resource-group <rgname> --key-permissions wrapKey unwrapKey get 


>[!Tip]
>Mantenga el URI de clave o el valor de keyID de la nueva clave para el próximo paso; por ejemplo: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h.
>
 
       
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Paso 3. Agregar la clave de Key Vault al servidor y establecer el protector de TDE
  
     cli
     # add server key and update encryption protector
     az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
     az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>

        
  > [!Note]
> La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.
> 

  
## <a name="step-4-turn-on-tde"></a>Paso 4 Activar TDE 
      cli
      # enable encryption
      az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled 
      

Ahora, la base de datos o el almacén de datos tienen TDE habilitado con una clave de cifrado administrada por el cliente en Azure Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Paso 5. Comprobar el estado de cifrado y la actividad de cifrado

     cli
      # get encryption scan progress
      az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

      # get whether encryption is on or off
      az sql db tde show --database <dbname> --server <servername> --resource-group <rgname> 

## <a name="sql-cli-references"></a>Referencias de la CLI de SQL

https://docs.microsoft.com/cli/azure/sql 

https://docs.microsoft.com/cli/azure/sql/server/key 

https://docs.microsoft.com/cli/azure/sql/server/tde-key 

https://docs.microsoft.com/cli/azure/sql/db/tde 

