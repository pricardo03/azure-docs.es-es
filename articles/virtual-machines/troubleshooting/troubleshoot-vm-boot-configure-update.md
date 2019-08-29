---
title: El inicio de la máquina virtual está detenido en "Preparando Windows. No apague el equipo" en Azure | Microsoft Docs
description: Introducción a los pasos de solución del problema donde el inicio de la máquina virtual se bloquea en "Preparando Windows. No apague el equipo."
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: 2f3c18ea1887ea5b05bb89f85371139ac83dfe49
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70080160"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>El inicio de la máquina virtual está detenido en "Preparando Windows. No apague el equipo"en Azure

Este artículo le ayudará a solucionar el problema cuando la máquina virtual (VM) está bloqueada en la fase "Preparando Windows. No apague el equipo" durante el inicio.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="symptoms"></a>Síntomas

Cuando usa **diagnósticos de arranque** para obtener la captura de pantalla de una máquina virtual, el sistema operativo no se inicia por completo. La máquina virtual muestra el mensaje "Preparando Windows. No apague el equipo."

![Ejemplo de mensaje para Windows Server 2012 R2](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Mensaje de ejemplo](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Causa

Normalmente, este problema se produce cuando el servidor está realizando el último reinicio después de que la configuración ha cambiado. El cambio en la configuración se puede inicializar mediante actualizaciones de Windows o cambios en los roles o las características del servidor. En el caso de Windows Update, si el tamaño de las actualizaciones es grande, el sistema operativo necesitará más tiempo para reconfigurar los cambios.

## <a name="back-up-the-os-disk"></a>Copia de seguridad del disco del sistema operativo

Antes de intentar corregir el problema, realice una copia de seguridad del disco del sistema operativo.

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>En máquinas virtuales con discos cifrados, debe desbloquear primero los discos.

Siga estos pasos para determinar si la máquina virtual es una máquina virtual cifrada.

1. En Azure Portal, abra la máquina virtual y, luego, vaya a los discos.

2. Examine la columna **Cifrado** para ver si está habilitado el cifrado.

Si el disco del sistema operativo está cifrado, desbloquee el disco cifrado. Para desbloquear el disco, siga estos pasos.

1. Cree una máquina virtual de recuperación que se encuentre en el mismo grupo de recursos, cuenta de almacenamiento y ubicación que la máquina virtual afectada.

2. En Azure Portal, elimine la máquina virtual afectada y guarde el disco.

3. Ejecute PowerShell como administrador.

4. Ejecute el siguiente cmdlet para obtener el nombre del secreto.

    ```Powershell
    Login-AzAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Cuando tenga el nombre del secreto, ejecute los siguientes comandos de PowerShell.

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Convierta el valor codificado en Base64 a bytes y escriba la salida en un archivo. 

    > [!Note]
    > El nombre de archivo de BEK debe coincidir con el GUID de BEK original si usa la opción de desbloqueo USB. Cree una carpeta en la unidad C denominada "BEK" antes de seguir estos pasos.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Después de crear el archivo BEK en su PC, cópielo en la máquina virtual de recuperación a la que ha asociado el disco del sistema operativo bloqueado. Ejecute los siguientes comandos mediante la ubicación del archivo BEK.

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Opcional**: En algunos escenarios, podría ser necesario descifrar el disco mediante el uso de este comando.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > El comando anterior supone que el disco que se va a cifrar está en la unidad F.

8. Si tiene que recopilar registros, vaya a la ruta de acceso **LETRA DE UNIDAD:\Windows\System32\winevt\Logs**.

9. Desasocie la unidad de la máquina de recuperación.

### <a name="create-a-snapshot"></a>Crear una instantánea

Para crear una instantánea, siga los pasos descritos en [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Recopilación de un volcado de memoria del sistema operativo

Use los pasos de la sección [Recopilación de un volcado de memoria del sistema operativo](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) para recopilar un volcado de memoria del sistema operativo cuando la máquina virtual se bloquea en la configuración.

## <a name="contact-microsoft-support"></a>Consultar al soporte técnico de Microsoft

Después de recopilar el archivo de volcado de memoria, póngase en contacto con el [servicio de soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para analizar la causa principal.


## <a name="rebuild-the-vm-by-using-powershell"></a>Recompilación de la máquina virtual mediante PowerShell

Después de recopilar el archivo de volcado de memoria, siga estos pasos para recompilar la máquina virtual.

**Para discos no administrados**

```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session
Get-AzSubscription –SubscriptionID “SubscriptionID” | Select-AzSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Para discos administrados**

```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session
Get-AzSubscription –SubscriptionID "SubscriptionID" | Select-AzSubscription

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

#This can be found by selecting the Managed Disks you wish you use in the Azure portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
