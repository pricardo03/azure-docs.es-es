---
title: El inicio de la máquina virtual está detenido en "Preparando Windows. No apague el equipo." en Azure | Microsoft Docs
description: Introducción a los pasos de solución del problema donde el inicio de la máquina virtual se atasca en "Preparando Windows. No apague el equipo".
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: e0ad7a8b083a72a9d3c630fa53601aa8fb6ad601
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48862753"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>El inicio de la máquina virtual está detenido en "Preparando Windows. No apague el equipo." en Azure

Este artículo le ayudará a solucionar el problema cuando la máquina virtual (VM) está atascada en la fase "Preparando Windows. No apague el equipo." durante el inicio.

## <a name="symptoms"></a>Síntomas

Cuando usa **diagnósticos de arranque** para obtener la captura de pantalla de una máquina virtual, se encuentra con que el sistema operativo no se inicia por completo. Además, la máquina virtual muestra un mensaje **"Preparando Windows. No apague el equipo".** de la directiva).

![Mensaje de ejemplo](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Mensaje de ejemplo](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Causa

Normalmente, este problema se produce cuando el servidor está realizando el último reinicio después de que la configuración ha cambiado. El cambio en la configuración se puede inicializar mediante actualizaciones de Windows o cambios en los roles o las características del servidor. En el caso de Windows Update, si el tamaño de las actualizaciones es grande, el sistema operativo necesitará más tiempo para reconfigurar los cambios.

## <a name="back-up-the-os-disk"></a>Copia de seguridad del disco del sistema operativo

Antes de intentar corregir el problema, realice una copia de seguridad del disco del sistema operativo:

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>En máquinas virtuales con discos cifrados, debe desbloquear primero los discos.

Valide si la máquina virtual es una máquina virtual cifrada. Para ello, siga estos pasos.

1. En el portal, abra la máquina virtual y, luego, vaya a los discos.

2. Verá una columna llamada "Cifrado", que le indicará si el cifrado está habilitado.

Si el disco del sistema operativo está cifrado, desbloquee el disco cifrado. Para ello, siga estos pasos.

1. Cree una máquina virtual de recuperación que se encuentre en el mismo grupo de recursos, cuenta de almacenamiento y ubicación que la máquina virtual afectada.

2. En Azure Portal, elimine la máquina virtual afectada y guarde el disco.

3. Ejecute PowerShell como administrador.

4. Ejecute el siguiente cmdlet para obtener el nombre del secreto.

    ```Powershell
    Login-AzureRmAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Cuando tenga el nombre del secreto, ejecute los siguientes comandos de PowerShell:

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Convierta el valor codificado en Base64 a bytes y escriba la salida en un archivo. 

    > [!Note]
    > El nombre de archivo de BEK debe coincidir con el GUID de BEK original si usa la opción de desbloqueo USB. Además, deberá crear una carpeta en la unidad C: denominada "BEK" para que los siguientes pasos funcionen.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Después de crear el archivo BEK en su PC, cópielo en la máquina virtual de recuperación a la que ha asociado el disco del sistema operativo bloqueado. Ejecute lo siguiente usando la ubicación del archivo BEK:

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Opcional**: en algunos escenarios puede ser también necesario descifrar el disco con este comando.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Eso teniendo en cuenta que el disco para cifrar esté en la letra F:.

8. Si tiene que recopilar registros, puede ir a ruta de acceso **LETRA DE UNIDAD:\Windows\System32\winevt\Logs**.

9. Desasocie la unidad de la máquina de recuperación.

### <a name="create-a-snapshot"></a>Crear una instantánea

Para crear una instantánea, siga los pasos descritos en [Instantánea de un disco](..\windows\snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Recopilación de un volcado de memoria del sistema operativo

Use los pasos de la sección [Recopilación de un volcado de memoria del sistema operativo](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) para recopilar un volcado de memoria del sistema operativo cuando la máquina virtual se atasca en la configuración.

## <a name="contact-microsoft-support"></a>Consultar al soporte técnico de Microsoft

Después de recopilar el archivo de volcado de memoria, póngase en contacto con el [servicio de soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para analizar la causa principal.


## <a name="rebuild-the-vm-using-powershell"></a>Recompilación de la máquina virtual mediante PowerShell

Después de recopilar el archivo de volcado de memoria, siga estos pasos para recompilar la máquina virtual.

**Para discos no administrados**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID “SubscriptionID” | Select-AzureRmSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzureRmVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzureRmNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzureRmVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzureRmVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Para discos administrados**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID "SubscriptionID" | Select-AzureRmSubscription

#Fill in all variables
$subid = "SubscriptionID"
$rgName = "ResourceGroupName";
$loc = "Location";
$vmSize = "VmSize";
$vmName = "VmName";
$nic1Name = "FirstNetworkInterfaceName";
#$nic2Name = "SecondNetworkInterfaceName";
$avName = "AvailabilitySetName";
$osDiskName = "OsDiskName";
$DataDiskName = "DataDiskName"

#This can be found by selecting the Managed Disks you wish you use in the Azure Portal if the format below doesn't match
$osDiskResouceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availabilty Set
#$avSet = Get-AzureRmAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResouceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResouceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additnal Data Disk
#Add-AzureRmVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzureRmVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
