---
title: Mantenimiento de un proveedor de recursos de SQL en Azure Stack |Microsoft Docs
description: Aprenda cómo puede mantener el servicio de proveedor de recursos SQL en Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e7ddbe1235b3957a1e0cb7693ee728bfdbf9db6b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295666"
---
# <a name="maintenance-operations"></a>Operaciones de mantenimiento 
El proveedor de recursos SQL es una máquina virtual bloqueada. La actualización de la seguridad de la máquina virtual del proveedor de recursos se puede realizar mediante el punto de conexión _DBAdapterMaintenance_ de JEA (Just Enough Administration) de PowerShell. Para facilitar estas operaciones, se proporciona un script con el paquete de instalación del proveedor de recursos.

## <a name="patching-and-updating"></a>Revisiones y actualizaciones
El proveedor de recursos de SQL no se suministra como parte de Azure Stack, ya que es un componente complementario. Microsoft proporcionará las actualizaciones para el proveedor de recursos de SQL según sea necesario. La instancia del proveedor de recursos de SQL se crea en la máquina virtual de un _usuario_, en la suscripción del proveedor predeterminado. Por lo tanto, es necesario proporcionar las revisiones de Windows, las firmas de antivirus, etcétera. Los paquetes de actualización de Windows que se proporcionan como parte del ciclo de revisión y actualización se pueden usar para aplicar actualizaciones a la máquina virtual Windows. Cuando se publica un adaptador actualizado, se proporciona un script para aplicar la actualización. Este script crea una nueva máquina virtual de RP y migra cualquier estado que ya tenga.

 ## <a name="backuprestoredisaster-recovery"></a>Copia de seguridad, restauración o recuperación ante desastres
 La copia de seguridad del proveedor de recursos de SQL no se realiza como parte del proceso de copia de seguridad y recuperación ante desastres de Azure Stack, ya que es un componente complementario. Se proporcionarán scripts para facilitar:
- La copia de seguridad de la información de estado necesaria (almacenada en una cuenta de almacenamiento de Azure Stack)
- La restauración del RP en caso de que sea necesario llevar a cabo la recuperación de la pila completa.
Los servidores de base de datos deben recuperarse en primer lugar (si es necesario), hasta que se restaure el proveedor de recursos.

## <a name="updating-sql-credentials"></a>Actualización de las credenciales de SQL
Es responsable de crear y mantener las cuentas de administrador del sistema en los servidores SQL Server. El proveedor de recursos necesita una cuenta con estos privilegios para administrar las bases de datos en nombre de los usuarios, no necesita acceso a los datos de esas bases de datos. Si tiene que actualizar las contraseñas de sa en los servidores SQL Server, puede utilizar la funcionalidad de actualización de la interfaz del administrador de proveedor de recursos para cambiar la contraseña almacenada utilizada por el proveedor de recursos. Estas contraseñas se almacenan en un almacén de claves en la instancia de Azure Stack.

Para modificar la configuración, haga clic en **Examinar** &gt; **RECURSOS ADMINISTRATIVOS** &gt; **Servidores de hospedaje SQL** &gt; **Inicios de sesión SQL** y seleccione un nombre de inicio de sesión. El cambio se debe efectuar en la instancia de SQL en primer lugar (y en cualquier réplica, si es necesario). En el panel **Configuración**, haga clic en **Contraseña**.

![Actualización de la contraseña de administrador](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Cambio de secretos 
*Estas instrucciones se aplican solo a sistemas integrados de Azure Stack (versión 1804 y posteriores). No intente el cambio de secretos en versiones de Azure Stack anteriores a la 1804.*

Al usar los proveedores de recursos de SQL y MySQL con sistemas integrados de Azure Stack, puede cambiar los siguientes secretos de infraestructura (implementación):
- Certificado SSL externo [proporcionado durante la implementación](azure-stack-pki-certs.md).
- La contraseña de la cuenta de administrador local de la máquina virtual del proveedor de recursos proporcionada durante la implementación.
- Contraseña de usuario de diagnóstico del proveedor de recursos (dbadapterdiag).

### <a name="powershell-examples-for-rotating-secrets"></a>Ejemplos de PowerShell de cambio de secretos

**Cambio de todos los secretos al mismo tiempo**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**Cambio únicamente de la contraseña de usuario de diagnóstico**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd 
```

**Cambio de contraseña de la cuenta de administrador local de la máquina virtual**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**Cambio del certificado SSL**
```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd 
```

### <a name="secretrotationsqlproviderps1-parameters"></a>Parámetros de SecretRotationSQLProvider.ps1

|.|DESCRIPCIÓN|
|-----|-----|
|AzCredential|Credencial de la cuenta de administrador de servicio de Azure Stack.|
|CloudAdminCredential|Credencial de cuenta de dominio de administración en la nube de Azure Stack.|
|PrivilegedEndpoint|Punto de conexión con privilegios para acceder a Get-AzureStackStampInformation.|
|DiagnosticsUserPassword|Contraseña de usuario de diagnóstico.|
|VMLocalCredential|La cuenta de administrador local de la máquina virtual de la máquina virtual de MySQLAdapter.|
|DefaultSSLCertificatePassword|Contraseña del certificado SSL predeterminado (*.pfx).|
|DependencyFilesLocalPath|Ruta de acceso local de los archivos de dependencia.|
|     |     |

### <a name="known-issues"></a>Problemas conocidos
**Problema**: Los registros para el cambio de secretos no se recopilan automáticamente si el script personalizado de cambio de secretos produce un error cuando se ejecuta.

**Solución alternativa**: Utilice el cmdlet Get-AzsDBAdapterLogs para recopilar todos los registros del proveedor de recursos, incluido AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, en C:Logs.

## <a name="update-the-virtual-machine-operating-system"></a>Actualización del sistema operativo de la máquina virtual
Hay varias maneras de actualizar la máquina virtual Windows Server:
- Instalar el paquete más reciente del proveedor de recursos mediante una imagen de Windows Server 2016 Core actualmente revisada
- Instalar un paquete de Windows Update durante la instalación o actualización del proveedor de recursos

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Actualización de las definiciones de Windows Defender de máquina virtual
Siga estos pasos para actualizar las definiciones de Defender:

1. Descargue la actualización de las definiciones de Windows Defender desde [esta página](https://www.microsoft.com/en-us/wdsi/definitions).

    En esa página, en "Manually download and install the definitions" (Descargar e instalar manualmente las definiciones), descargue el archivo de 64 bits "Windows Defender Antivirus for Windows 10 and Windows 8.1" (Windows Defender Antivirus para Windows 10 y Windows 8.1). 
    
    Vínculo directo: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64.

2. Cree una sesión de PowerShell al punto de conexión de mantenimiento de la máquina virtual del adaptador del proveedor de recursos SQL.
3. Copie el archivo de actualización de definiciones en la máquina del adaptador de base de datos mediante la sesión del punto de conexión de mantenimiento.
4. En la sesión de PowerShell de mantenimiento, invoque el comando _Update-DBAdapterWindowsDefenderDefinitions_.
5. Después de la instalación, se recomienda quitar el archivo de actualización de definiciones usado. Se puede quitar en la sesión de mantenimiento mediante el comando _Remove-ItemOnUserDrive)_.


Este es un script de ejemplo para actualizar las definiciones de Defender (sustituya la dirección o el nombre de la máquina virtual por el valor real):

```powershell
# Set credentials for the RP VM local admin user
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession 
```


## <a name="collect-diagnostic-logs"></a>Recopilación de registros de diagnóstico
El proveedor de recursos SQL es una máquina virtual bloqueada. Si es necesario recopilar registros de la máquina virtual, se proporciona para tal fin un punto de conexión _DBAdapterDiagnostics_ de JEA (Just Enough Administration) de PowerShell. Hay dos comandos disponibles a través de este punto de conexión:

- **Get-AzsDBAdapterLog**. Prepara un paquete ZIP que contiene registros de diagnóstico del proveedor de recursos y lo coloca en la unidad de usuario de la sesión. El comando se puede invocar sin ningún parámetro y recopila las últimas cuatro horas de registros.
- **Remove-AzsDBAdapterLog**. Limpia los paquetes de registros existentes en la máquina virtual del proveedor de recursos.

Se crea una cuenta de usuario llamada **dbadapterdiag** durante la implementación o la actualización del proveedor de recursos para conectar con el punto de conexión de diagnóstico y extraer los registros del proveedor de recursos. La contraseña de esta cuenta es la misma que la proporcionada para la cuenta de administrador local durante la implementación o actualización.

Para usar estos comandos, deberá crear una sesión remota de PowerShell a la máquina virtual del proveedor de recursos e invocar el comando. También puede proporcionar los parámetros FromDate y ToDate. Si no especifica uno o ninguno de estos parámetros, el valor de FromDate será cuatro horas antes de la hora actual y el valor de ToDate será la hora actual.

Este script de ejemplo muestra el uso de estos comandos:

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample captures logs from the previous one hour
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup logs
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session
$session | Remove-PSSession
```

## <a name="next-steps"></a>Pasos siguientes
[Adición de servidores de hospedaje de SQL Server](azure-stack-sql-resource-provider-hosting-servers.md)
