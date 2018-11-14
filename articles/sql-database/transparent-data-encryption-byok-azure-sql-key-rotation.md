---
title: 'PowerShell: Rotación del protector de TDE (Azure SQL Database)| Microsoft Docs' description: Obtenga información sobre cómo rotar el Cifrado de datos transparente (TDE) para un servidor SQL de Azure.
services: sql-database ms.service: sql-database ms.subservice: security ms.custom: ms.devlang: ms.topic: conceptual author: aliceku ms.author: aliceku ms.reviewer: vanto manager: jhubbard ms.date: 08/07/2017
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

- Esta guía de procedimientos asume que ya utiliza una clave de Azure Key Vault como protector TDE para una base de datos Azure SQL Database o Data Warehouse. Consulte [Cifrado de datos transparente con compatibilidad con BYOK](transparent-data-encryption-byok-azure-sql.md).
- Es preciso tener instalada y en ejecución la versión 3.7.0 o más reciente de Azure PowerShell. 
- [Recomendado pero opcional] Cree primero el material de clave para el protector de TDE en un módulo de seguridad de hardware (HSM) o en un almacén local de claves e importe el material de clave a Azure Key Vault. Siga las [instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) para más información.

## <a name="option-1-auto-rotation"></a>Opción 1: Rotación automática

Genere una nueva versión de la clave del protector de TDE existente en Key Vault, bajo el mismo nombre de clave y almacén de claves. El servicio SQL de Azure empieza a usar esta nueva versión en 24 horas. 

Para crear una nueva versión del protector de TDE mediante el cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey):

   ```powershell
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>
   ```

## <a name="option-2-manual-rotation"></a>Opción 2: Rotación manual

La opción utiliza los cmdlets [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) y [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) para agregar una clave completamente nueva, que podría estar bajo un nuevo nombre de clave o incluso en otro almacén de claves. 

>[!NOTE]
>La longitud combinada para el nombre del almacén de claves y el nombre de la clave no puede superar los 94 caracteres.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Otros cmdlets útiles de PowerShell

- Para cambiar el protector de TDE del modo administrado por Microsoft a BYOK, use el cmdlet [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Para cambiar el protector de TDE del modo BYOK al administrado por Microsoft, use el cmdlet [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Pasos siguientes

- En caso de riesgo de seguridad, aprenda a quitar un protector de TDE potencialmente comprometido: [Eliminación de una clave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

- Comience a trabajar con la compatibilidad de Bring Your Own Key para TDE: [Activación del TDE con su propia clave desde Key Vault mediante PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
