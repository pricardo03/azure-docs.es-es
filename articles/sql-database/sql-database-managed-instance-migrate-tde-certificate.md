---
title: 'Migración de certificados TDE: Instancia administrada de Azure SQL Database | Microsoft Docs'
description: Migración del certificado que protege la clave de cifrado de base de datos de una base de datos con cifrado de datos transparente a Instancia administrada de Azure SQL
keywords: tutorial de sql database, instancia administrada de sql database, migrar certificado TDE
services: sql-database
author: MladjoA
ms.reviewer: carlrab, jovanpop
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 08/09/2018
ms.author: mlandzic
manager: craigg
ms.openlocfilehash: 73990d6feeed56114bc3c66164bbb53c093bbe21
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44050618"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-managed-instance"></a>Migración de certificados de una base de datos protegida por TDE a Instancia administrada de Azure SQL

Al migrar una base de datos protegida mediante [Cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) a Instancia administrada de Azure SQL con la opción de restauración nativa, se debe migrar el certificado correspondiente de SQL Server local o IaaS antes de restaurar la base de datos. Este artículo le guía en el proceso de migración manual del certificado a Instancia administrada de Azure SQL Database:

> [!div class="checklist"]
> * Exportación del certificado a un archivo de intercambio de información personal (.pfx)
> * Extracción del certificado del archivo a la cadena de base 64
> * Carga del archivo mediante el cmdlet de PowerShell

Para ver una opción alternativa que emplea el servicio totalmente administrado para la migración completa de la base de datos protegida con TDE y el certificado correspondiente, consulte [Migración de la base de datos local a Instancia administrada mediante Azure Database Migration Service](../dms/tutorial-sql-server-to-managed-instance.md).

> [!IMPORTANT]
> Cifrado de datos transparente para Instancia administrada de Azure SQL funciona en modo de servicio administrado. El certificado migrado se usa únicamente para la restauración de la base de datos protegida por TDE. Poco después de realizar la restauración, el certificado migrado se sustituye por un certificado administrado por el sistema diferente.

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, necesitará lo siguiente:

- Tener instalada la herramienta de la línea de comandos [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) en el servidor local o en otro equipo con acceso al certificado exportado como un archivo. La herramienta Pvk2Pfx forma parte del [kit de controladores de Windows para empresa](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk), un entorno de línea de comandos autocontenido independiente.
- Tener instalado [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell), versión 5.0 o superior.
- Módulo de AzureRM PowerShell [instalado y actualizado](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
- [Módulo AzureRM.Sql](https://www.powershellgallery.com/packages/AzureRM.Sql) versión 4.10.0 o superior.
  Ejecute los siguientes comandos de PowerShell para instalar o actualizar el módulo de PowerShell:

   ```powershell
   Install-Module -Name AzureRM.Sql
   Update-Module -Name AzureRM.Sql
   ```

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>Exportación del certificado TDE a un archivo de intercambio de información personal (.pfx)

El certificado se puede exportar directamente desde la instancia de SQL Server de origen o desde el almacén de certificados si se conserva ahí.

### <a name="export-certificate-from-the-source-sql-server"></a>Exportación del certificado desde la instancia de SQL Server de origen

Siga estos pasos para exportar el certificado con SQL Server Management Studio y convertirlo a formato pfx. En los pasos se usan los nombres genéricos *TDE_Cert* y *full_path* para los nombres de certificados y archivos y las rutas de acceso. Estos se deben reemplazar por los nombres reales.

1. En SSMS, abra una nueva ventana de consulta y conéctese a la instancia de SQL Server de origen.
2. Use el siguiente script para enumerar las bases de datos protegidas por TDE y obtener el nombre del certificado que protege el cifrado de la base de datos que se va a migrar:

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![Lista de certificados TDE](./media/sql-database-managed-instance-migrate-tde-certificate/onprem-certificate-list.png)

3. Ejecute el script siguiente para exportar el certificado a un par de archivos (.cer y .pvk), y mantener así la información de clave pública y privada:

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![Copia de seguridad del certificado TDE](./media/sql-database-managed-instance-migrate-tde-certificate/backup-onprem-certificate.png)

4. Use la consola de PowerShell para copiar la información del certificado de un par de archivos recién creados en un archivo de intercambio de información personal (.pfx), mediante la herramienta Pvk2Pfx:

   ```powershell
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>Exportar el certificado desde el almacén de certificados

Si el certificado se guarda en el almacén de certificados de la máquina local de SQL Server, se puede exportar mediante los siguientes pasos:

1. Abra la consola de PowerShell y ejecute el siguiente comando para abrir el complemento Certificados de Microsoft Management Console:

   ```powershell
   certlm
   ```

2. En el complemento Certificados de MMC, expanda la ruta de acceso Personal -> Certificados para ver la lista de certificados.

3. Haga clic con el botón derecho en el certificado y haga clic en Exportar.

4. Siga el asistente para exportar el certificado y la clave privada a un formato de intercambio de información personal.

## <a name="upload-certificate-to-azure-sql-managed-instance-using-azure-powershell-cmdlet"></a>Carga del certificado en Instancia administrada de Azure SQL mediante el cmdlet de Azure PowerShell

1. Comience con los pasos de preparación de PowerShell:

   ```powershell
   # Import the module into the PowerShell session
   Import-Module AzureRM
   # Connect to Azure with an interactive dialog for sign-in
   Connect-AzureRmAccount
   # List subscriptions available and copy id of the subscription target Managed Instance belongs to
   Get-AzureRmSubscription
   # Set subscription for the session (replace Guid_Subscription_Id with actual subscription id)
   Select-AzureRmSubscription Guid_Subscription_Id
   ```

2. Una vez realizados todos los pasos de preparación, ejecute los siguientes comandos para cargar el certificado codificado en base 64 en la instancia administrada de destino:

   ```powershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "SomeStrongPassword"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzureRmSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<ResourceGroupName>" -ManagedInstanceName "<ManagedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

El certificado ya está disponible para la instancia administrada especificada y la copia de seguridad de la base de datos protegida por TDE correspondiente se puede restaurar correctamente.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a migrar el certificado que protege la clave de cifrado de base de datos con Cifrado de datos transparente, de una instancia local o IaaS de SQL Server a Instancia administrada de Azure SQL.

Para saber cómo restaurar la copia de seguridad de una base de datos en Instancia administrada de Azure SQL Database, consulte [Restauración de una copia de seguridad de datos en una instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started-restore.md).
