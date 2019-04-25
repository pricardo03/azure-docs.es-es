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
ms.date: 03/12/2019
ms.openlocfilehash: ad7e760bf84ee08e3928164432564fb23c10d211
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60330659"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>Eliminación de un protector de Cifrado de datos transparente (TDE) con PowerShell

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

- Debe tener una suscripción de Azure y ser un administrador en esa suscripción.
- Debe tener Azure PowerShell instalado y ejecutándose. 
- Esta guía de procedimientos asume que ya utiliza una clave de Azure Key Vault como protector TDE para una base de datos Azure SQL Database o Data Warehouse. Para más información, consulte [Cifrado de datos transparente con compatibilidad con BYOK](transparent-data-encryption-byok-azure-sql.md).

## <a name="overview"></a>Información general

En esta guía de procedimientos se describe cómo responder a un protector TDE potencialmente comprometido para una instancia de Azure SQL Database o Data Warehouse mediante TDE con claves administradas por el usuario en Azure Key Vault y compatibilidad con Bring Your Own Key (BYOK). Para más información sobre la compatibilidad con BYOK para TDE, consulte la [página de introducción](transparent-data-encryption-byok-azure-sql.md). 

Los siguientes procedimientos deben realizarse únicamente en casos extremos o en entornos de prueba. Revise la guía de procedimientos cuidadosamente, ya que si se eliminan protectores TDE que se usan activamente en Azure Key Vault puede dar como resultado la **pérdida de datos**. 

Si alguna vez se sospecha que una clave está comprometida, de modo que un servicio o usuario ha tenido acceso no autorizado a la clave, lo mejor es eliminarla.

Tenga en cuenta que cuando se elimina el protector de TDE en Key Vault, **se bloquean todas las conexiones a las bases de datos cifradas en el servidor y estas bases de datos quedarán sin conexión y se eliminarán en 24 horas**. Las copias de seguridad antiguas cifradas con la clave en peligro ya no son accesibles.

Los siguientes pasos describen cómo comprobar las huellas digitales de Protector de TDE en archivos por registro Virtual (VLF) de una base de datos de uso. La huella digital del protector de TDE actual de la base de datos y el identificador de la base de datos puede encontrarse ejecutando: Seleccione [database_id],       [encryption_state], [encryptor_type], /*clave asimétrica significa que Azure Key VAULT, certificado significa claves administradas del servicio*/ [encryptor_thumbprint,] desde [sys]. [ dm_database_encryption_keys] 
 
La consulta siguiente devuelve el VLF y el sistema de cifrado huellas digitales respectivas en uso. Cada huella digital diferente hace referencia a otra clave en Azure Key Vault (AKV): Seleccionar * desde sys.dm_db_log_info (database_id) 

El comando de PowerShell Get-AzureRmSqlServerKeyVaultKey proporciona la huella digital del Protector de TDE usado en la consulta, para que pueda ver qué claves se deben mantener y qué claves se deben eliminar en Azure Key VAULT. Solo las claves que ya no usa la base de datos se pueden eliminar con seguridad de Azure Key Vault.

Esta guía de procedimientos explica dos enfoques dependiendo del resultado deseado después de la respuesta al incidente:

- Para que las bases de datos de Azure SQL Database y Data Warehouse estén **accesibles**
- Para que las bases de datos de Azure SQL Database y Data Warehouse estén **inaccesibles**

## <a name="to-keep-the-encrypted-resources-accessible"></a>Para que los recursos cifrados estén accesibles

1. Cree una [nueva clave en Key Vault](/powershell/module/az.keyvault/add-azkeyvaultkey). Asegúrese de que esta nueva clave se crea en un almacén de claves independiente desde el protector de TDE potencialmente comprometido, ya que el control de acceso se aprovisiona en un nivel de almacén.
2. Agregue la nueva clave en el servidor mediante el [agregar AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) y [conjunto AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets y actualícelo como nuevo protector de TDE del servidor.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   # Set the key as the TDE protector for all resources under the server
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Asegúrese de que el servidor y las réplicas se hayan actualizado para el nuevo protector de TDE mediante el [Get AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) cmdlet. 

   >[!NOTE]
   > El nuevo protector de TDE puede tardar unos minutos en propagarse a todas las bases de datos y a las bases de datos secundarias del servidor.

   ```powershell
   Get-AzSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Realice una [copia de seguridad de la nueva clave](/powershell/module/az.keyvault/backup-azkeyvaultkey) en Key Vault.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```
 
5. Eliminar la clave en peligro de Key Vault con la [Remove-AzKeyVaultKey](/powershell/module/az.keyvault/remove-azkeyvaultkey) cmdlet. 

   ```powershell
   Remove-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```
 
6. Para restaurar una clave en Key Vault en el futuro mediante el [restauración AzKeyVaultKey](/powershell/module/az.keyvault/restore-azkeyvaultkey) cmdlet:
   ```powershell
   Restore-AzKeyVaultKey `
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
