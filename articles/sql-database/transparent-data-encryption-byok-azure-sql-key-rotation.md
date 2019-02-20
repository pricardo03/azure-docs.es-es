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
ms.date: 12/06/2018
ms.openlocfilehash: 45cd4e884530836d515e0c6cce8a6fc9be109d88
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992014"
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

- Esta guía de procedimientos asume que ya utiliza una clave de Azure Key Vault como protector TDE para una base de datos Azure SQL Database o Data Warehouse. Consulte [Cifrado de datos transparente con la integración de Azure Key Vault, compatibilidad con BYOK](transparent-data-encryption-byok-azure-sql.md).
- Es preciso tener instalada y en ejecución la versión 3.7.0 o más reciente de Azure PowerShell. 
- [Recomendado pero opcional] Cree primero el material de clave para el protector de TDE en un módulo de seguridad de hardware (HSM) o en un almacén local de claves e importe el material de clave a Azure Key Vault. Siga las [instrucciones para usar un módulo de seguridad de hardware (HSM) y Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) para más información.

## <a name="manual-key-rotation"></a>Rotación manual de claves

La rotación manual de claves usa los cmdlets [Add-AzureeKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) y [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) para agregar una clave completamente nueva, que podría estar bajo un nuevo nombre de clave o incluso en otro almacén de claves. Usar este enfoque permite agregar la misma clave a distintos almacenes de claves para admitir escenarios de alta disponibilidad y recuperación ante desastres geográfica.

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

- En caso de riesgo de seguridad, aprenda a quitar un protector de TDE potencialmente comprometido: [Eliminación de una clave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Introducción a la integración de Azure Key Vault y compatibilidad con Bring Your Own Key para TDE: [Activar TDE con su propia clave de Key Vault mediante PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
