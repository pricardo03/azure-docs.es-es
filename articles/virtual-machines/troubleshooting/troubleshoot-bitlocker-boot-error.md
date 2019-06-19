---
title: Solución de problemas de errores de inicio de BitLocker en una máquina virtual de Azure | Microsoft Docs
description: Obtenga información sobre cómo solucionar problemas de errores de inicio de BitLocker en una máquina virtual de Azure
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/25/2019
ms.author: genli
ms.openlocfilehash: 116748d7887ebf2ad821e3159c7c1bdcc2428121
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "64684756"
---
# <a name="bitlocker-boot-errors-on-an-azure-vm"></a>Errores de inicio de BitLocker en una máquina virtual de Azure

 En este artículo se describen los errores de BitLocker que puede experimentar cuando inicie una máquina virtual (VM) de Windows en Microsoft Azure.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Síntoma

 Una máquina virtual de Windows no se inicia. Al revisar las capturas de pantalla en la ventana [Diagnósticos de arranque](../windows/boot-diagnostics.md), ve uno de los mensajes de error siguientes:

- Inserte la unidad USB que tiene la clave de BitLocker

- ¡Estás bloqueado! Escriba la clave de recuperación para seguir trabajando (distribución del teclado: EE. UU.). Se ha escrito la información de inicio de sesión incorrecta varias veces, de modo que su PC se ha bloqueado para proteger su privacidad. Para recuperar la clave de recuperación, ve a https://windows.microsoft.com/recoverykeyfaq desde otro PC o dispositivo móvil. En caso de que lo necesites, el Id. de clave es XXXXXXX. O bien, puedes restablecer el PC.

- Escriba la contraseña para desbloquear esta unidad [ ]. Presione la tecla Insertar para ver la contraseña mientras escribe.
- Escribe la clave de recuperación. Cargar la clave de recuperación desde un dispositivo USB.

## <a name="cause"></a>Causa

Este problema puede producirse si la máquina virtual no puede encontrar el archivo de clave de recuperación de BitLocker (BEK) para descifrar el disco cifrado.

## <a name="solution"></a>Solución

Para resolver este problema, detenga y desasocie la máquina virtual y, a continuación, reiníciela. Esta operación hace que la máquina virtual recupere el archivo BEK desde Azure Key Vault y lo coloque en el disco cifrado. 

Si este método no resuelve el problema, siga estos pasos para restaurar el archivo BEK manualmente:

1. Tome una instantánea del disco del sistema de la máquina virtual afectada como copia de seguridad. Para más información, vea [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).
2. [Conecte el disco del sistema a una máquina virtual de recuperación](troubleshoot-recovery-disks-portal-windows.md) cifrada por BitLocker. Esto es necesario para ejecutar el comando [manage-bde](https://docs.microsoft.com/windows-server/administration/windows-commands/manage-bde) que solo está disponible en la máquina virtual cifrada con BitLocker.

    Cuando conecte un disco administrado, puede recibir un mensaje de error que indique que "contiene configuración cifrada y, por lo tanto, no puede usarse como disco de datos". En esta situación, ejecute el siguiente script para probar asociar el disco de nuevo:

    ```Powershell
    $rgName = "myResourceGroup"
    $osDiskName = "ProblemOsDisk"

    New-AzDiskUpdateConfig -EncryptionSettingsEnabled $false |Update-AzDisk -diskName $osDiskName -ResourceGroupName $rgName

    $recoveryVMName = "myRecoveryVM" 
    $recoveryVMRG = "RecoveryVMRG" 
    $OSDisk = Get-AzDisk -ResourceGroupName $rgName -DiskName $osDiskName;

    $vm = get-AzVM -ResourceGroupName $recoveryVMRG -Name $recoveryVMName 

    Add-AzVMDataDisk -VM $vm -Name $osDiskName -ManagedDiskId $osDisk.Id -Caching None -Lun 3 -CreateOption Attach 

    Update-AzVM -VM $vm -ResourceGroupName $recoveryVMRG
    ```
     No se puede asociar un disco administrado a una máquina virtual que se restauró a partir de una imagen de blob.

3. Después de asociar el disco, realice una conexión remota del Escritorio a la máquina virtual de recuperación para que pueda ejecutar algunos scripts de Azure PowerShell. Asegúrese de tener instalada la [versión más reciente de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) en la máquina virtual de recuperación.

4. Abra una sesión de Azure PowerShell con privilegios elevados (ejecución como administrador). Ejecute los siguientes comandos para iniciar sesión en la suscripción de Azure:

    ```Powershell
    Add-AzAccount -SubscriptionID [SubscriptionID]
    ```

5. Ejecute el script siguiente para comprobar el nombre del archivo BEK:

    ```powershell
    $vmName = "myVM"
    $vault = "myKeyVault"
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.ContentType -match 'BEK')} `
            | Sort-Object -Property Created `
            | ft  Created, `
                @{Label="Content Type";Expression={$_.ContentType}}, `
                @{Label ="Volume"; Expression = {$_.Tags.VolumeLetter}}, `
                @{Label ="DiskEncryptionKeyFileName"; Expression = {$_.Tags.DiskEncryptionKeyFileName}}
    ```

    El siguiente es un ejemplo de la salida. Busque el nombre de archivo BEK para el disco asociado. En este caso, se supone que la letra de unidad del disco asociado es F y el archivo BEK es EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK.

    ```
    Created             Content Type Volume DiskEncryptionKeyFileName               
    -------             ------------ ------ -------------------------               
    4/5/2018 7:14:59 PM Wrapped BEK  C:\    B4B3E070-836C-4AF5-AC5B-66F6FDE6A971.BEK
    4/7/2018 7:21:16 PM Wrapped BEK  F:\    EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    4/7/2018 7:26:23 PM Wrapped BEK  G:\    70148178-6FAE-41EC-A05B-3431E6252539.BEK
    4/7/2018 7:26:26 PM Wrapped BEK  H:\    5745719F-4886-4940-9B51-C98AFABE5305.BEK
    ```

    Si ve dos volúmenes duplicados, el volumen que tiene la marca de tiempo más reciente es el archivo BEK actual que la máquina virtual de recuperación usa.

    Si el valor de **Content Type** es **Wrapped BEK**, vaya a [Key Encryption Key (KEK) scenarios](#key-encryption-key-scenario) [Escenarios de Key Encryption Key (KEK)].

    Ahora que tiene el nombre del archivo BEK para la unidad, tendrá que crear el archivo nombre-archivo-secreto.BEK para desbloquear la unidad. 

6.  Descargue el archivo BEK en el disco de recuperación. En el ejemplo siguiente se guarda el archivo BEK en la carpeta C:\BEK. Asegúrese de que la ruta de acceso `C:\BEK\` existe antes de ejecutar los scripts.

    ```powershell
    $vault = "myKeyVault"
    $bek = " EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK"
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $bek
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = "C:\BEK\DiskEncryptionKeyFileName.BEK"
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7.  Para desbloquear el disco asociado mediante el archivo BEK, ejecute el siguiente comando:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    En este ejemplo, el disco del sistema operativo asociado es la unidad F. Asegúrese de que usar la letra de unidad correcta. 

    - Si el disco se ha desbloqueado correctamente mediante el uso de la clave BEK, podemos considerar que el problema de BItLocker está resuelto. 

    - Si la clave BEK no desbloquea el disco, puede usar la suspensión de la protección para desactivar temporalmente BitLocker, mediante la ejecución del comando siguiente
    
        ```powershell
        manage-bde -protectors -disable F: -rc 0
        ```      
    - Si va a recompilar la máquina virtual usando el disco dytem, debe descifrar totalmente la unidad. Para ello, ejecute el siguiente comando:

        ```powershell
        manage-bde -off F:
        ```
8.  Desasocie el disco de la máquina virtual de recuperación y, a continuación, vuelva a asociarlo a la máquina virtual afectada como disco del sistema. Para más información, consulte [Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación](troubleshoot-recovery-disks-windows.md).

### <a name="key-encryption-key-scenario"></a>Escenario de clave de cifrado de claves

Para un escenario de clave de cifrado de claves, siga estos pasos:

1. Asegúrese de que la cuenta de usuario que inició sesión requiera el permiso "unwrapped" en las directivas de acceso de Key Vault en **USUARIO|Permisos de clave|Operaciones criptográficas|Desencapsular clave**.
2. Guarde los siguientes scripts en un archivo .PS1:

    ```powershell
    #Set the Parameters for the script
    param (
            [Parameter(Mandatory=$true)]
            [string] 
            $keyVaultName,
            [Parameter(Mandatory=$true)]
            [string] 
            $kekName,
            [Parameter(Mandatory=$true)]
            [string]
            $secretName,
            [Parameter(Mandatory=$true)]
            [string]
            $bekFilePath,
            [Parameter(Mandatory=$true)]
            [string] 
            $adTenant
            )
    # Load ADAL Assemblies. The following script assumes that the Azure PowerShell version you installed is 1.0.0. 
    $adal = "${env:ProgramFiles}\WindowsPowerShell\Modules\Az.Accounts\1.0.0\PreloadAssemblies\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles}\WindowsPowerShell\Modules\Az.Accounts\1.0.0\PreloadAssemblies\Microsoft.IdentityModel.Clients.ActiveDirectory.Platform.dll"
    [System.Reflection.Assembly]::LoadFrom($adal)
    [System.Reflection.Assembly]::LoadFrom($adalforms)

    # Set well-known client ID for AzurePowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2" 
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURI = "https://vault.azure.net"
    # Set Authority to Azure AD Tenant
    $authority = "https://login.windows.net/$adtenant"
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $authResult = $authContext.AcquireTokenAsync($resourceAppIdURI, $clientId, $redirectUri, $platformParameters).result
    # Generate auth header 
    $authHeader = $authResult.CreateAuthorizationHeader()
    # Set HTTP request headers to include Authorization header
    $headers = @{'x-ms-version'='2014-08-01';"Authorization" = $authHeader}

    ########################################################################################################################
    # 1. Retrieve wrapped BEK
    # 2. Make KeyVault REST API call to unwrap the BEK
    # 3. Convert the Base64Url string returned by KeyVault unwrap to Base64 string 
    # 4. Convert Base64 string to bytes and write to the BEK file
    ########################################################################################################################

    #Get wrapped BEK and place it in JSON object to send to KeyVault REST API
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $secretName
    $wrappedBekSecretBase64 = $keyVaultSecret.SecretValueText
    $jsonObject = @"
    {
    "alg": "RSA-OAEP",
    "value" : "$wrappedBekSecretBase64"
    }
    "@

    #Get KEK Url
    $kekUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name $kekName).Key.Kid;
    $unwrapKeyRequestUrl = $kekUrl+ "/unwrapkey?api-version=2015-06-01";

    #Call KeyVault REST API to Unwrap 
    $result = Invoke-RestMethod -Method POST -Uri $unwrapKeyRequestUrl -Headers $headers -Body $jsonObject -ContentType "application/json" -Debug

    #Convert Base64Url string returned by KeyVault unwrap to Base64 string
    $base64UrlBek = $result.value;
    $base64Bek = $base64UrlBek.Replace('-', '+');
    $base64Bek = $base64Bek.Replace('_', '/');
    if($base64Bek.Length %4 -eq 2)
    {
        $base64Bek+= '==';
    }
    elseif($base64Bek.Length %4 -eq 3)
    {
        $base64Bek+= '=';
    }

    #Convert base64 string to bytes and write to BEK file
    $bekFileBytes = [System.Convert]::FromBase64String($base64Bek);
    [System.IO.File]::WriteAllBytes($bekFilePath,$bekFileBytes)
    ```
3. Establezca los parámetros. El script procesará el secreto KEK para crear la clave BEK y, a continuación, lo guardará en una carpeta local en la máquina virtual de recuperación.

4. Verá la siguiente salida cuando se inicie el script:

        GAC    Version        Location                                                                              
        ---    -------        --------                                                                              
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\Az.Accounts\...
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\Az.Accounts\...

    Una vez que finalice el script, verá la salida siguiente:

        VERBOSE: POST https://myvault.vault.azure.net/keys/rondomkey/<KEY-ID>/unwrapkey?api-
        version=2015-06-01 with -1-byte payload
        VERBOSE: received 360-byte response of content type application/json; charset=utf-8


5. Para desbloquear el disco asociado mediante el archivo BEK, ejecute el siguiente comando:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    En este ejemplo, el disco del sistema operativo asociado es la unidad F. Asegúrese de que usar la letra de unidad correcta. 

    - Si el disco se ha desbloqueado correctamente mediante el uso de la clave BEK, podemos considerar que el problema de BItLocker está resuelto. 

    - Si la clave BEK no desbloquea el disco, puede usar la suspensión de la protección para desactivar temporalmente BitLocker, mediante la ejecución del comando siguiente
    
        ```powershell
        manage-bde -protectors -disable F: -rc 0
        ```      
    - Si va a recompilar la máquina virtual usando el disco dytem, debe descifrar totalmente la unidad. Para ello, ejecute el siguiente comando:

        ```powershell
        manage-bde -off F:
        ```

6. Desasocie el disco de la máquina virtual de recuperación y, a continuación, vuelva a asociarlo a la máquina virtual afectada como disco del sistema. Para más información, consulte [Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación](troubleshoot-recovery-disks-windows.md).
