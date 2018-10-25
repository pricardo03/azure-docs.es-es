---
title: Cifrado de datos transparente para Azure SQL Database y Data Warehouse | Microsoft Docs
description: Información general sobre el cifrado de datos transparente para SQL Database y Data Warehouse. Este documento explica sus ventajas y las opciones de configuración, que incluye cifrado de datos transparente administrado por el servicio y Bring Your Own Key.
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
ms.date: 10/15/2018
ms.openlocfilehash: 0d5b7e484024294eb5c95b632dbef85c377b717e
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469034"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Cifrado de datos transparente para SQL Database y Data Warehouse

El cifrado de datos transparente (TDE) ayuda a proteger Azure SQL Database y Azure Data Warehouse frente a la amenaza de actividades malintencionadas. También realiza cifrado y descifrado de la base de datos en tiempo real, copias de seguridad asociadas y archivos de registro de transacciones en reposo sin necesidad de efectuar cambios en la aplicación. De forma predeterminada, TDE está habilitado para todas las instancias de Azure SQL Database recién implementadas. TDE no se puede usar para cifrar la base de datos **maestra** lógica en SQL Database.  La base de datos **maestra** contiene objetos que son necesarios para realizar las operaciones de TDE en las bases de datos de usuario.

TDE tendrá que habilitarse manualmente para bases de datos anteriores o Azure SQL Data Warehouse.  

El cifrado de datos transparente se encarga de cifrar el almacenamiento de toda una base de datos mediante una clave simétrica denominada clave de cifrado de base de datos. Esta clave de cifrado de base de datos está protegida por el protector de cifrado de datos transparente. El protector es un certificado administrado por el servicio (cifrado de datos transparentes administrado por el servicio) o una clave asimétrica almacenada en Azure Key Vault (Bring Your Own Key). El protector de cifrado de datos transparente se configura en el nivel de servidor.

Durante el inicio de la base de datos, la clave de cifrado de base de datos cifrada se descifra y, a continuación, se usa para descifrar y volver a cifrar los archivos de base de datos en el proceso del motor de base de datos de SQL Server. El cifrado de datos transparente realiza el cifrado de E/S en tiempo real y el descifrado de los datos en el nivel de página. Todas las páginas se descifran cuando se leen en la memoria y, a continuación, se cifran antes de escribirse en el disco. Para consultar una descripción general del cifrado de datos transparente, consulte [Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Las instancias de SQL Server que se ejecutan en una máquina virtual de Azure también pueden usar una clave asimétrica desde Key Vault. Los pasos de configuración son diferentes del uso de una clave asimétrica en SQL Database. Para obtener más información, consulte [Administración extensible de claves con Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Cifrado de datos transparente administrado por el servicio

En Azure, la configuración predeterminada para el cifrado de datos transparente es aquella por la que la clave de cifrado de base de datos está protegida por un certificado de servidor integrado. El certificado de servidor integrado es único para cada servidor. Si una base de datos está en una relación de replicación geográfica, la base de datos principal y secundaria con replicación geográfica están protegidas por la clave de servidor principal de la base de datos principal. Si hay dos bases de datos conectadas al mismo servidor, comparten el mismo certificado integrado. Microsoft alterna automáticamente estos certificados al menos cada 90 días.

Microsoft también mueve y administra con total fluidez las claves según sea necesario para la replicación geográfica y las restauraciones.

> [!IMPORTANT]
> Todas las bases de datos SQL recién creadas se cifran de forma predeterminada mediante el uso de cifrado de datos transparente administrado por el servicio. Las bases de datos existentes antes de mayo de 2017 y las bases de datos creadas mediante restauración, replicación geográfica o copia de base de datos no se cifran de manera predeterminada.

## <a name="bring-your-own-key"></a>Bring Your Own Key

Gracias a la compatibilidad con Bring Your Own Key, puede tomar el control de las claves de cifrado de datos transparente y supervisar quién puede acceder a ellas y cuándo. Key Vault, que es el sistema de administración de claves externas basado en la nube de Azure, es el primer servicio de administración de claves que el cifrado de datos transparente ha integrado con la compatibilidad con Bring Your Own Key. Con la compatibilidad con Bring Your Own Key, la clave de cifrado de base de datos está protegida por una clave asimétrica almacenada en Key Vault. La clave asimétrica nunca sale de Key Vault. Una vez que el servidor tenga permisos para un almacén de claves, el servidor le envía solicitudes básicas de operación de claves a través de Key Vault. Establezca la clave asimétrica en el nivel de servidor y la heredarán todas las bases de datos en ese servidor.

Con la compatibilidad con Bring Your Own Key, ahora puede controlar las tareas de administración de claves, como las rotaciones de claves y los permisos del almacén de claves. También puede eliminar claves y habilitar auditorías o generación de informes en todas las claves de cifrado. Key Vault ofrece una administración central de claves y usa módulos de seguridad de hardware extremadamente supervisados. Key Vault promueve la separación de la administración de claves y datos para ayudar a cumplir las normativas. Para obtener más información acerca de Key Vault, consulte la [página de documentación de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

Para obtener más información acerca del cifrado de datos transparente con compatibilidad con Bring Your Own Key para SQL Database y Data Warehouse, consulte [Cifrado de datos transparente con compatibilidad con Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md).

Para empezar a usar el cifrado de datos transparente con compatibilidad con Bring Your Own Key, consulte la guía de procedimientos [Turn on transparent data encryption by using your own key from Key Vault by using PowerShell](transparent-data-encryption-byok-azure-sql-configure.md) (Activación del cifrado de datos transparente con su propia clave desde Key Vault mediante PowerShell).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Traslado de una base de datos protegida por cifrado de datos transparente

No es necesario descifrar las bases de datos para las operaciones dentro de Azure. La configuración del cifrado de datos transparente en la base de datos de origen o la base de datos principal se hereda de forma transparente en el destino. Entre las operaciones incluidas están las siguientes:

- Restauración geográfica
- Restauración a un momento específico de autoservicio
- Restauración de una base de datos eliminada
- Replicación geográfica activa
- Creación de una copia de base de datos

Al exportar una base de datos protegida por el cifrado de datos transparente, no se cifra el contenido exportado de la base de datos. Este contenido exportado se almacena en archivos BACPAC no cifrados. Asegúrese de proteger adecuadamente los archivos BACPAC y habilitar el cifrado de datos transparente una vez finalizada la importación de la nueva base de datos.

Por ejemplo, si el archivo BACPAC se exporta desde una instancia de SQL Server local, el contenido importado de la nueva base de datos no se cifra automáticamente. Del mismo modo, si el archivo BACPAC se exporta a una instancia de SQL Server local, la base de datos nueva tampoco se cifra automáticamente.

La única excepción es cuando se exporta a y desde una base de datos SQL. El cifrado de datos transparente está habilitado en la nueva base de datos, pero todavía no se cifra el propio archivo BACPAC.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Administración del cifrado de datos transparente en Azure Portal

Para configurar el cifrado de datos transparente mediante Azure Portal, debe estar conectado como propietario o colaborador de Azure o administrador de seguridad SQL.

Establezca el cifrado de datos transparente en el nivel de base de datos. Para habilitar el cifrado de datos transparente en una base de datos, vaya a [Azure Portal](https://portal.azure.com) e inicie sesión con su cuenta de administrador o colaborador de Azure. Busque la configuración de cifrado de datos transparente en la base de datos de usuario. De forma predeterminada, se usa el cifrado de datos transparente administrado por el servicio. Se genera automáticamente un certificado de cifrado de datos transparente para el servidor que contiene la base de datos.

![Cifrado de datos transparente administrado por el servicio](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Establezca la clave maestra del cifrado de datos transparente, también conocida como protector de cifrado de datos transparente, en el nivel de servidor. Para usar el cifrado de datos transparente con compatibilidad con Bring Your Own Key y proteger las bases de datos con una clave de Key Vault, consulte la configuración del cifrado de datos transparente en el servidor.

![Cifrado de datos transparente con compatibilidad con Bring Your Own Key](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Administración del cifrado de datos transparente mediante PowerShell

Para configurar el cifrado de datos transparente mediante PowerShell, debe estar conectado como propietario o colaborador de Azure o administrador de seguridad SQL.

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Habilita o deshabilita el cifrado de datos transparente para una base de datos|
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Obtiene el estado del cifrado de datos transparente para una base de datos |
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption-Activity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Comprueba el progreso del cifrado de una base de datos |
| [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Agrega una clave de Key Vault a una instancia de SQL Server |
| [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Obtiene las claves de Key Vault para una instancia de SQL Server  |
| [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Establece el protector de cifrado de datos transparente para una instancia de SQL Server |
| [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |Obtiene el protector de cifrado de datos transparente |
| [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Quita una clave de Key Vault de una instancia de SQL Server |
|  | |

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Administración del cifrado de datos transparente mediante Transact-SQL

Conéctese a la base de datos mediante un inicio de sesión que sea un administrador o un miembro del rol **dbmanager** en la base de datos maestra.

| Get-Help | DESCRIPCIÓN |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) | SET ENCRYPTION ON/OFF cifra o descifra una base de datos |
| [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Devuelve información sobre el estado de cifrado de una base de datos y sus claves de cifrado de base de datos asociadas |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Devuelve información sobre el estado de cifrado de cada nodo de almacenamiento de datos y sus claves de cifrado de base de datos asociadas |
|  | |

No puede cambiar el protector de cifrado de datos transparente para una clave de Key Vault mediante Transact-SQL. Use PowerShell o Azure Portal.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Administración del cifrado de datos transparente mediante la API REST

Para configurar el cifrado de datos transparente mediante la API REST, debe estar conectado como el propietario o colaborador de Azure o administrador de seguridad SQL.

| Get-Help | DESCRIPCIÓN |
| --- | --- |
|[Create Or Update Server](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Agrega una identidad de Azure Active Directory a una instancia de SQL Server (que se usa para conceder acceso a Key Vault)|
|[Create Or Update Server Key](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Agrega una clave de Key Vault a una instancia de SQL Server|
|[Delete Server Key](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Quita una clave de Key Vault de una instancia de SQL Server|
|[Get Server Keys](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Obtiene una clave específica de Key Vault de una instancia de SQL Server|
|[List Server Keys By Server](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Obtiene las claves de Key Vault para una instancia de SQL Server |
|[Create Or Update Encryption Protector](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Establece el protector de cifrado de datos transparente para una instancia de SQL Server|
|[Get Encryption Protector](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Obtiene el protector de cifrado de datos transparente para una instancia de SQL Server|
|[List Encryption Protectors By Server](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Obtiene los protectores de cifrado de datos transparente para una instancia de SQL Server |
|[Create Or Update Transparent Data Encryption Configuration](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Habilita o deshabilita el cifrado de datos transparente para una base de datos|
|[Get Transparent Data Encryption Configuration](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Obtiene la configuración de cifrado de datos transparente para una base de datos|
|[List Transparent Data Encryption Configuration Results](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Obtiene el resultado de cifrado para una base de datos|

## <a name="next-steps"></a>Pasos siguientes

- Para consultar una descripción general del cifrado de datos transparente, consulte [Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Para obtener más información acerca del cifrado de datos transparente con compatibilidad con Bring Your Own Key para SQL Database y Data Warehouse, consulte [Cifrado de datos transparente con compatibilidad con Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md).
- Para empezar a usar el cifrado de datos transparente con compatibilidad con Bring Your Own Key, consulte la guía de procedimientos [Turn on transparent data encryption by using your own key from Key Vault by using PowerShell](transparent-data-encryption-byok-azure-sql-configure.md) (Activación del cifrado de datos transparente con su propia clave desde Key Vault mediante PowerShell).
- Para obtener más información acerca de Key Vault, consulte la [página de documentación de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
