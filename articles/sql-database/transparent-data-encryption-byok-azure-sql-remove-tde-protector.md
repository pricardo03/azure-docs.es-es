---
title: 'PowerShell: Eliminación de un protector de TDE en Azure SQL Database | Microsoft Docs'
description: Guía de procedimientos para responder a un protector TDE potencialmente comprometido para una base de datos Azure SQL Database o Data Warehouse mediante TDE con compatibilidad con Bring Your Own Key (BYOK).
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 10/12/2018
ms.openlocfilehash: 8ffda7fd1b987e34dc0e8157b535ccef65571247
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567900"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>Eliminación de un protector de Cifrado de datos transparente (TDE) con PowerShell

## <a name="prerequisites"></a>Requisitos previos

- Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
- Es preciso tener instalada y en ejecución la versión 4.2.0 o más reciente de Azure PowerShell. 
- Esta guía de procedimientos asume que ya utiliza una clave de Azure Key Vault como protector TDE para una base de datos Azure SQL Database o Data Warehouse. Para más información, consulte [Cifrado de datos transparente con compatibilidad con BYOK](transparent-data-encryption-byok-azure-sql.md).

## <a name="overview"></a>Información general

Esta guía de procedimientos describe cómo responder a un protector TDE potencialmente comprometido para una base de datos Azure SQL Database o Data Warehouse mediante TDE con compatibilidad con Bring Your Own Key (BYOK). Para más información sobre la compatibilidad con BYOK para TDE, consulte la [página de introducción](transparent-data-encryption-byok-azure-sql.md). 

Los siguientes procedimientos deben realizarse únicamente en casos extremos o en entornos de prueba. Revise la guía de procedimientos cuidadosamente, ya que si se eliminan protectores TDE que se usan activamente en Azure Key Vault puede dar como resultado la **pérdida de datos**. 

Si alguna vez se sospecha que una clave está comprometida, de modo que un servicio o usuario ha tenido acceso no autorizado a la clave, lo mejor es eliminarla.

Tenga en cuenta que cuando se elimina el protector de TDE en Key Vault, **se bloquean todas las conexiones a las bases de datos cifradas en el servidor y estas bases de datos quedarán sin conexión y se eliminarán en 24 horas**. Las copias de seguridad antiguas cifradas con la clave en peligro ya no son accesibles.

Esta guía de procedimientos explica dos enfoques dependiendo del resultado deseado después de la respuesta al incidente:

- Para que las bases de datos de Azure SQL Database y Data Warehouse estén **accesibles**
- Para que las bases de datos de Azure SQL Database y Data Warehouse estén **inaccesibles**

## <a name="to-keep-the-encrypted-resources-accessible"></a>Para que los recursos cifrados estén accesibles

1. Cree una [nueva clave en Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey?view=azurermps-4.1.0). Asegúrese de que esta nueva clave se crea en un almacén de claves independiente desde el protector de TDE potencialmente comprometido, ya que el control de acceso se aprovisiona en un nivel de almacén. 
2. Agregue la nueva clave al servidor mediante los cmdlets [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) y [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) y actualícela como el nuevo protector de TDE del servidor.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   # Set the key as the TDE protector for all resources under the server
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Asegúrese de que el servidor y las réplicas se han actualizado al nuevo protector de TDE con el cmdlet [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector). 

   >[!NOTE]
   > El nuevo protector de TDE puede tardar unos minutos en propagarse a todas las bases de datos y a las bases de datos secundarias del servidor.

   ```powershell
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Realice una [copia de seguridad de la nueva clave](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey) en Key Vault.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```

5. Elimine la clave comprometida de Key Vault mediante el cmdlet [Remove-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/remove-azurekeyvaultkey). 

   ```powershell
   Remove-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```

6. Para restaurar una clave en Key Vault en el futuro mediante el cmdlet [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey):
   ```powershell
   Restore-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -InputFile <BackupFilePath>
   ```

## <a name="to-make-the-encrypted-resources-inaccessible"></a>Para hacer inaccesibles los recursos cifrados

1. Quite las bases de datos que se están cifrando por la clave potencialmente comprometida.

   La copia de seguridad de la base de datos y los archivos de registro se realiza automáticamente, por lo que se puede realizar una restauración a un momento dado de la base de datos en cualquier momento (siempre y cuando se proporcione la clave). Las bases de datos deben eliminarse antes de eliminar un protector TDE activo para evitar la pérdida potencial de datos de hasta 10 minutos de las transacciones más recientes. 
2. Realice una copia de seguridad del material de la clave del protector de TDE en Key Vault.
3. Eliminación de la clave potencialmente comprometida de Key Vault

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a cambiar el protector de TDE de un servidor para cumplir con los requisitos de seguridad: [Eliminación de un protector de Cifrado de datos transparente (TDE) con PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- Introducción sobre la compatibilidad de Bring Your Own Key para TDE: [Activar TDE con su propia clave de Key Vault mediante PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
