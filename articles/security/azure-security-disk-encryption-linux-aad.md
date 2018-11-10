---
title: Azure Disk Encryption con máquinas virtuales IaaS de Linux de aplicación de Azure AD (versión anterior) | Microsoft Docs
description: En este artículo se proporcionan instrucciones sobre cómo habilitar Microsoft Azure Disk Encryption para máquinas virtuales IaaS de Linux.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/19/2018
ms.openlocfilehash: b0b67121e172bb29d1f95e56d3b31f509552bf2e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211237"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms-previous-release"></a>Habilitación de Azure Disk Encryption para máquinas virtuales Iaas de Linux (versión anterior)

**La nueva versión de Azure Disk Encryption elimina la necesidad de proporcionar un parámetro de aplicación de Azure AD para habilitar el cifrado de disco de máquina virtual. Con la nueva versión, ya no es necesario proporcionar credenciales de Azure AD durante el paso de habilitar el cifrado. Todas las nuevas máquinas virtuales deben estar cifradas sin los parámetros de aplicación de Azure AD con la nueva versión. Para instrucciones sobre cómo habilitar el cifrado de disco de máquina virtual con la nueva versión, consulte [Azure Disk Encryption para máquinas virtuales de Linux](azure-security-disk-encryption-linux.md). Las máquinas virtuales que ya se han cifrado con parámetros de aplicación de Azure AD se siguen admitiendo y se deben seguir manteniendo con la sintaxis de AAD.**

Puede habilitar muchos escenarios de cifrado de disco, y los pasos pueden variar en función del escenario. En las secciones siguientes se tratan con más detalle los escenarios de máquinas virtuales IaaS de Linux. Antes de usar el cifrado de disco, es necesario satisfacer los [requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites-aad.md) y se debe revisar la sección de [requisitos previos adicionales para máquinas virtuales IaaS de Linux](azure-security-disk-encryption-prerequisites-aad.md#bkmk_LinuxPrereq).

Tome una [instantánea](../virtual-machines/windows/snapshot-copy-managed-disk.md) o realice una copia de seguridad antes de cifrar los discos. Las copias de seguridad garantizan que, en caso de un error inesperado durante el cifrado, sea posible una opción de recuperación. Las máquinas virtuales con discos administrados requieren una copia de seguridad antes del cifrado. Una vez realizada la copia de seguridad, puede usar el cmdlet Set-AzureRmVMDiskEncryptionExtension para cifrar los discos administrados mediante la especificación del parámetro -skipVmBackup. Para más información sobre cómo realizar la copia de seguridad y restauración de máquinas virtuales cifradas, consulte el artículo sobre [Azure Backup](../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
 >Con el fin de garantizar que los secretos de cifrado no traspasen los límites regionales, Azure Disk Encryption necesita que Key Vault y las máquinas virtuales estén ubicadas conjuntamente en la misma región. Cree y use una instancia de Key Vault que se encuentre en la misma región que la máquina virtual que se va a cifrar.</br></br>

> Al cifrar los volúmenes del sistema operativo Linux, el proceso puede tardar unas horas. Es normal que los volúmenes del sistema operativo Linux tarden más en cifrarse que los volúmenes de datos. 

>Solo se puede deshabilitar el cifrado en máquinas virtuales Linux para volúmenes de datos. No se admite en volúmenes de datos o volúmenes del sistema operativo si el volumen del sistema operativo se ha cifrado.  


## <a name="bkmk_NewLinux"></a> Implementación de una nueva máquina virtual IaaS de Linux con cifrado de disco habilitado 

1. Use la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel) para crear una máquina virtual IaaS de Linux cifrada. La plantilla crea una máquina virtual RedHat Linux 7.2 con una matriz RAID-0 de 200 GB y cifrado de disco completo mediante discos administrados. En el artículo de [P+F](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport), observará que algunas distribuciones de Linux solo admiten el cifrado de discos de datos. Sin embargo, esta plantilla ofrece una oportunidad de familiarizarse con la implementación de plantillas y la comprobación del estado de cifrado con varios métodos. 
 
1. Haga clic en **Deploy to Azure** (Implementar en Azure) en la plantilla de Azure Resource Manager.

2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, los parámetros, los términos legales y el contrato. Haga clic en **Crear** para habilitar el cifrado en la máquina virtual IaaS existente o en ejecución.

3. Después de implementar la plantilla, compruebe el estado de cifrado de la máquina virtual con su método preferido:
     - Realice la comprobación con la CLI de Azure con el comando [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show). 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
         ```

     - Realice la comprobación con Azure PowerShell con el comando [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus). 

         ```azurepowershell-interactive
         Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
         ```

     - Seleccione la máquina virtual y haga clic en **Discos** en el encabezado **Configuración** para comprobar el estado de cifrado en el portal. En el gráfico que aparece en **Cifrado**, verá si está habilitado. 

| Parámetro | DESCRIPCIÓN |
| --- | --- |
| Identificador de cliente de AAD | Identificador de cliente de la aplicación de Azure AD que tiene permisos para escribir secretos en el almacén de claves. |
| Secreto de cliente de AAD | Secreto de cliente de la aplicación de Azure AD que tiene permisos para escribir secretos en el almacén de claves. |
| Nombre de Key Vault | Nombre del almacén de claves donde se debe colocar la clave. |
| Grupo de recursos de Key Vault | Grupo de recursos del almacén de claves. |


## <a name="bkmk_RunningLinux"></a> Habilitación del cifrado en una máquina virtual IaaS de Linux existente o en ejecución
En este escenario, puede habilitar el cifrado mediante la plantilla de Resource Manager, los cmdlets de PowerShell o los comandos de la CLI. 

>[!IMPORTANT]
 >Es obligatorio crear una instantánea o una copia de seguridad de una instancia de máquina virtual basada en un disco administrado fuera de Azure Disk Encryption y antes de habilitar esta característica. Puede tomar una instantánea del disco administrado desde el portal o se puede usar [Azure Backup](../backup/backup-azure-vms-encryption.md). Las copias de seguridad garantizan que es posible disponer de una opción de recuperación en el caso de que se produzca un error inesperado durante el cifrado. Una vez que se realiza una copia de seguridad, el cmdlet Set-AzureRmVMDiskEncryptionExtension puede utilizarse para cifrar los discos administrados especificando el parámetro -skipVmBackup. El comando Set-AzureRmVMDiskEncryptionExtension produce un error en las máquinas virtuales basadas en un disco administrado hasta que se realice una copia de seguridad y se especifique este parámetro. 
>
>Cifrar o deshabilitar el cifrado puede provocar el reinicio de la máquina virtual. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Habilitación del cifrado en una máquina virtual Linux existente o en ejecución mediante la CLI de Azure 
Puede habilitar el cifrado de disco en el disco duro virtual cifrado mediante la instalación y el uso de la herramienta de la línea de comandos [CLI de Azure 2.0](/cli/azure). Se puede usar en el explorador con [Azure Cloud Shell](../cloud-shell/overview.md), o lo puede instalar en el equipo local y usarlo en cualquier sesión de PowerShell. Para habilitar el cifrado en máquinas virtuales IaaS de Linux existentes o en ejecución en Azure, use los siguientes comandos:

Use el comando [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable) para habilitar el cifrado en una máquina virtual IaaS en ejecución en Azure.

-  **Cifrar una máquina virtual en ejecución mediante un secreto de cliente:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Cifrar una máquina virtual en ejecución con KEK para encapsular el secreto de cliente:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> </br>
La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Compruebe que los discos están cifrados:** para comprobar el estado de cifrado de una máquina virtual IaaS, use el comando [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show). 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Deshabilitar el cifrado:** para deshabilitar el cifrado, use el comando [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). La deshabilitación del cifrado solo se permite en volúmenes de datos de máquinas virtuales Linux.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Habilitación del cifrado en una máquina virtual Linux existente o en ejecución mediante PowerShell
Use el cmdlet [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) para habilitar el cifrado en una máquina virtual IaaS en ejecución en Azure. 

-  **Cifrar una máquina virtual en ejecución mediante un secreto de cliente:** el siguiente script inicializa las variables y ejecuta el cmdlet Set-AzureRmVMDiskEncryptionExtension. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual, el almacén de claves, la aplicación de AAD y el secreto de cliente. Reemplace MySecureRg, MySecureVM, MySecureVault, My-AAD-client-ID y My-AAD-client-secret por sus propios valores. Es posible que deba agregar el parámetro -VolumeType si va a cifrar discos de datos y no el disco del sistema operativo. 

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Cifrar una máquina virtual en ejecución con KEK para encapsular el secreto de cliente:** Azure Disk Encryption le permite especificar una clave existente en el almacén de claves para encapsular los secretos de cifrado de disco que se generaron al habilitar el cifrado. Cuando se especifica una clave de cifrado de claves, Azure Disk Encryption usa esa clave para encapsular los secretos de cifrado antes de escribirlos en Key Vault. Es posible que deba agregar el parámetro -VolumeType si va a cifrar discos de datos y no el disco del sistema operativo. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```

 >[!NOTE]
 > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> </br>
La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Comprobar que los discos están cifrados:** para comprobar el estado de cifrado de una máquina virtual IaaS, use el cmdlet [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus). 
    
     ```azurepowershell-interactive 
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName MySecureRg -VMName MySecureVM
     ```
    
- **Deshabilitar el cifrado de disco:** para deshabilitar el cifrado, use el cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). La deshabilitación del cifrado solo se permite en volúmenes de datos de máquinas virtuales Linux.
     
     ```azurepowershell-interactive 
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Habilitación del cifrado en una máquina virtual IaaS de Linux existente o en ejecución con una plantilla

Puede habilitar el cifrado de disco en una máquina virtual IaaS con Linux existente o en ejecución en Azure mediante la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).

2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, los parámetros, los términos legales y el contrato. Haga clic en **Crear** para habilitar el cifrado en la máquina virtual IaaS existente o en ejecución.

En la tabla siguiente figuran los parámetros de la plantilla de Resource Manager para máquinas virtuales existentes o en ejecución que usan un identificador de cliente de Azure AD:

| Parámetro | DESCRIPCIÓN |
| --- | --- |
| AADClientID | Identificador de cliente de la aplicación de Azure AD que tiene permisos para escribir secretos en el almacén de claves. |
| AADClientSecret | Secreto de cliente de la aplicación de Azure AD que tiene permisos para escribir secretos en el almacén de claves. |
| keyVaultName | Nombre del almacén de claves donde se debe cargar la clave. Puede obtenerlo mediante el comando `az keyvault show --name "MySecureVault" --query resourceGroup` de la CLI de Azure. |
|  keyEncryptionKeyURL | Dirección URL de la clave de cifrado de claves que se usa para cifrar la clave generada. Este parámetro es opcional si selecciona **nokek** en la lista desplegable de UseExistingKek. Si selecciona **kek** en la lista desplegable de UseExistingKek, debe proporcionar el valor de _keyEncryptionKeyURL_. |
| volumeType | Tipo de volumen en que se realiza la operación de cifrado. Los valores admitidos válidos son _OS_ o _All_ (consulte las distribuciones de Linux admitidas y sus versiones para los discos de sistema operativo y datos en la sección anterior de requisitos previos). |
| sequenceVersion | Versión de la secuencia de la operación de BitLocker. Incremente el número de versión cada vez que se realice una operación de cifrado de disco en la misma máquina virtual. |
| vmName | Nombre de la máquina virtual en que se va a realizar la operación de cifrado. |
| frase de contraseña | Escriba una frase de contraseña segura como clave de cifrado de datos. |



## <a name="bkmk_EFA"> </a>Uso de la característica EncryptFormatAll para discos de datos en máquinas virtuales IaaS de Linux
El parámetro **EncryptFormatAll** reduce el tiempo que se tardan en cifrar los discos de datos de Linux. Las particiones que cumplen determinados criterios se formatean (con su actual sistema de archivos). A continuación, se deben volver a montar donde estaban antes de la ejecución del comando. Si quiere excluir un disco de datos que cumple los criterios, puede desmontarlo antes de ejecutar el comando.

 Después de ejecutar este comando, se formatean las unidades que se han montado anteriormente. A continuación, se inicia la capa de cifrado sobre la unidad ahora vacía. Cuando se selecciona esta opción, también se cifra el disco de recursos efímero asociado a la máquina virtual. Si el disco efímero se restablece, la solución Azure Disk Encryption lo vuelve a formatear y cifrar para la máquina virtual en la siguiente oportunidad.

>[!WARNING]
> EncryptFormatAll no debe usarse cuando hay datos necesarios en los volúmenes de datos de una máquina virtual. Para excluir discos del cifrado, puede desmontarlos. Debe probar primero EncryptFormatAll en una máquina virtual de prueba y comprender el parámetro de característica y su implicación antes de intentar usarlo en la máquina virtual de producción. La opción EncryptFormatAll formatea el disco de datos, y todos los datos que contiene se pierden. Antes de continuar, compruebe que los discos que quiere excluir estén desmontados correctamente. </br></br>
 >Si configura este parámetro mientras actualiza la configuración de cifrado, podría provocar un reinicio antes del cifrado real. En este caso, también querrá quitar el disco que no quiere que se formatee del archivo fstab. De igual forma, debe agregar la partición que quiere cifrar formateada al archivo fstab antes de iniciar la operación de cifrado. 

### <a name="bkmk_EFACriteria"> </a> Criterios de EncryptFormatAll
El parámetro recorre todas las particiones y las cifra siempre que cumplan **todos** los siguientes criterios: 
- No es una partición raíz, de arranque o del sistema operativo
- Ya no está cifrada
- No es un volumen BEK
- No es un volumen RAID
- No es un volumen LVM
- Está montada

Cifre los discos que componen el volumen LVM o RAID en lugar de los volúmenes LVM o RAID.

### <a name="bkmk_EFATemplate"> </a> Uso del parámetro EncryptFormatAll con una plantilla
Para usar la opción EncryptFormatAll, use cualquier plantilla de Azure Resource Manager ya existente que cifre una máquina virtual Linux y cambie el campo **EncryptionOperation** del recurso AzureDiskEncryption.

1. Por ejemplo, use la [plantilla de Resource Manager para cifrar una máquina virtual IaaS de Linux en ejecución](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm). 
2. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).
3. Cambie **EncryptionOperation** de **EnableEncryption** a **EnableEncryptionFormatAl**
4. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, otros parámetros, los términos legales y el contrato. Haga clic en **Crear** para habilitar el cifrado en la máquina virtual IaaS existente o en ejecución.


### <a name="bkmk_EFAPSH"> </a> Uso del parámetro EncryptFormatAll con un cmdlet de PowerShell
Use el cmdlet [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) con el [parámetro EncryptFormatAll](https://www.powershellgallery.com/packages/AzureRM/5.0.0). 

**Cifrar una máquina virtual en ejecución mediante un secreto de cliente y EncryptFormatAll:** como ejemplo, el siguiente script inicializa las variables y ejecuta el cmdlet Set-AzureRmVMDiskEncryptionExtension con el parámetro EncryptFormatAll. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual, el almacén de claves, la aplicación de AAD y el secreto de cliente. Reemplace MySecureRg, MySecureVM, MySecureVault, My-AAD-client-ID y My-AAD-client-secret por sus propios valores.
  
   ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MySecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
   ```


### <a name="bkmk_EFALVM"> </a> Uso del parámetro EncryptFormatAll con el Administrador de volúmenes lógicos (LVM) 
Se recomienda una configuración LVM-on-crypt. En todos los ejemplos siguientes, reemplace la ruta de acceso de dispositivo y los puntos de montaje por lo que se adapte a su caso de uso. Esta configuración se puede realizar de la manera siguiente:

- Agregue los discos de datos que componen la máquina virtual.
- Formatee, monte y agregue estos discos al archivo fstab.

    1. Formatee el disco recién agregado. Aquí se usarán symlinks generados por Azure. El uso de symlinks evita los problemas relacionados con el cambio de los nombres de dispositivo. Para más información, consulte el artículo [Solución de problemas de nombres de dispositivo](../virtual-machines/linux/troubleshoot-device-names-problems.md).
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. Monte los discos.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. Agréguelos a fstab.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. Ejecute el cmdlet Set-AzureRmVMDiskEncryptionExtension PowerShell con -EncryptFormatAll para cifrar estos discos.
         ```azurepowershell-interactive
         Set-AzureRmVMDiskEncryptionExtension -ResouceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. Configure LVM encima de estos nuevos discos. Tenga en cuenta que las unidades cifradas se desbloquean después de que la máquina virtual ha terminado de arrancar. Por lo tanto, el montaje de LVM también tendrá que retrasarse.




## <a name="bkmk_VHDpre"> </a> Máquinas virtuales IaaS creadas a partir de discos duros virtuales cifrados y claves de cifrado
En este escenario, se puede habilitar el cifrado mediante la plantilla de Resource Manager, cmdlets de PowerShell o comandos de la CLI. Las siguientes secciones explican con más detalle la plantilla de Resource Manager y los comandos de la CLI. 

Use las instrucciones del apéndice para preparar imágenes previamente cifradas que se pueden usar en Azure. Una vez creada la imagen, puede seguir los pasos de la sección siguiente para crear una VM cifrada de Azure.

* [Preparación de un VHD con Windows precifrado](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Preparación de un VHD con Linux precifrado](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Es obligatorio crear una instantánea o una copia de seguridad de una instancia de máquina virtual basada en un disco administrado fuera de Azure Disk Encryption y antes de habilitar esta característica. Puede tomar una instantánea del disco administrado desde el portal o se puede usar [Azure Backup](../backup/backup-azure-vms-encryption.md). Las copias de seguridad garantizan que es posible disponer de una opción de recuperación en el caso de que se produzca un error inesperado durante el cifrado. Una vez que se realiza una copia de seguridad, el cmdlet Set-AzureRmVMDiskEncryptionExtension puede utilizarse para cifrar los discos administrados especificando el parámetro -skipVmBackup. El comando Set-AzureRmVMDiskEncryptionExtension produce un error en las máquinas virtuales basadas en un disco administrado hasta que se realice una copia de seguridad y se especifique este parámetro. 
>
>Cifrar o deshabilitar el cifrado puede provocar el reinicio de la máquina virtual. 



### <a name="bkmk_VHDprePSH"> </a> Uso de Azure PowerShell para cifrar las máquinas virtuales IaaS con discos duros virtuales previamente cifrados 
Puede habilitar el cifrado de disco en el disco duro virtual cifrado mediante el cmdlet [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples) de PowerShell. En el ejemplo siguiente se proporcionan algunos parámetros comunes. 

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResouceGroupName "MySecureRG"
```

### <a name="bkmk_VHDpreRM"> </a> Uso de la plantilla de Resource Manager para cifrar las máquinas virtuales IaaS con discos duros virtuales previamente cifrados 
Puede habilitar el cifrado de disco en el VHD cifrado mediante el uso de la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).

2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, los parámetros, los términos legales y el contrato. Haga clic en **Crear** para habilitar el cifrado en la nueva máquina virtual IaaS.

En la tabla siguiente figuran los parámetros de la plantilla de Resource Manager para el VHD cifrado:

| Parámetro | DESCRIPCIÓN |
| --- | --- |
| newStorageAccountName | Nombre de la cuenta de almacenamiento que almacena el VHD del sistema operativo cifrado. Esta cuenta de almacenamiento se debe haber creado en el mismo grupo de recursos y en la misma ubicación que la máquina virtual. |
| osVhdUri | Identificador URI del VHD de sistema operativo de la cuenta de almacenamiento. |
| osType | Tipo de producto de sistema operativo (Windows o Linux). |
| virtualNetworkName | Nombre de la red virtual a la que debería pertenecer la NIC de la máquina virtual. El nombre se debe haber creado en el mismo grupo de recursos y en la misma ubicación que la máquina virtual. |
| subnetName | El nombre de la subred en la red virtual a la que debería pertenecer la NIC de la máquina virtual. |
| vmSize | Tamaño de la máquina virtual. Actualmente, solo se admiten las series A, D y G estándar. |
| keyVaultResourceID | El valor ResourceID que identifica el recurso de almacén de claves en Azure Resource Manager. Puede obtenerlo mediante el cmdlet `(Get-AzureRmKeyVault -VaultName &lt;MyKeyVaultName&gt; -ResourceGroupName &lt;MyResourceGroupName&gt;).ResourceId` de PowerShell o con el comando `az keyvault show --name "MySecureVault" --query id` de la CLI de Azure.|
| keyVaultSecretUrl | La dirección URL de la clave de cifrado del disco que se puede configurar en el almacén de claves. |
| keyVaultKekUrl | La dirección URL de la clave de cifrado de claves para cifrar la clave de cifrado de disco generada. |
| vmName | Nombre de la máquina virtual IaaS. |

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Habilitación del cifrado en un disco de datos recién agregado
Puede agregar un nuevo disco de datos mediante [az vm disk attach](../virtual-machines/linux/add-disk.md), o [Azure Portal](../virtual-machines/linux/attach-disk-portal.md). Para poder cifrarlo, deberá montar primero el disco de datos recién asociado. Dado que la unidad de datos no se puede usar mientras el cifrado está en curso, deberá solicitar el cifrado de la unidad. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Habilitación del cifrado en un disco recién agregado con la CLI de Azure
 Si la VM se cifró previamente con "All", entonces el parámetro --volume-type debe permanecer como All. All incluye los discos de datos y del SO. Si la VM se cifró previamente con un tipo de volumen de "OS", entonces el parámetro--volume-type se debe cambiar a All, de modo que se incluyan el nuevo disco de datos y el SO. Si la VM se cifró con el tipo de volumen de "Data", entonces puede permanecer como "Data", tal y como se muestra a continuación. Para preparar el cifrado, no es suficiente con agregar y asociar un nuevo disco de datos a una VM. El disco recién conectado también se debe formatear y montar correctamente en la VM antes de habilitar el cifrado. En Linux, el disco se debe montar en /etc/fstab con un [nombre de dispositivo de bloque persistente](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

A diferencia de la sintaxis de PowerShell, la CLI no requiere que el usuario proporcione ninguna versión de secuencia única cuando se habilita el cifrado. La CLI genera y usa su propio valor de versión de secuencia único automáticamente.

-  **Cifrar una máquina virtual en ejecución mediante un secreto de cliente:** 

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Cifrar una máquina virtual en ejecución con KEK para encapsular el secreto de cliente:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Habilitación del cifrado en un disco recién agregado con Azure PowerShell
 Al usar PowerShell para cifrar un nuevo disco para Linux, se debe especificar una nueva versión de secuencia. La versión de secuencia debe ser única. El siguiente script genera un GUID para la versión de secuencia. 
 

-  **Cifrar una máquina virtual en ejecución mediante un secreto de cliente:** el siguiente script inicializa las variables y ejecuta el cmdlet Set-AzureRmVMDiskEncryptionExtension. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual, el almacén de claves, la aplicación de AAD y el secreto de cliente. Reemplace MySecureRg, MySecureVM, MySecureVault, My-AAD-client-ID y My-AAD-client-secret por sus propios valores. El parámetro - VolumeType se establece en discos de datos y no en el disco del sistema operativo. Si la VM se cifró previamente con un tipo de volumen de "OS" o "All", entonces el parámetro --VolumeType se debe cambiar a All, de modo que se incluyan el nuevo disco de datos y el SO.

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
    ```
- **Cifrar una máquina virtual en ejecución con KEK para encapsular el secreto de cliente:** Azure Disk Encryption le permite especificar una clave existente en el almacén de claves para encapsular los secretos de cifrado de disco que se generaron al habilitar el cifrado. Cuando se especifica una clave de cifrado de claves, Azure Disk Encryption usa esa clave para encapsular los secretos de cifrado antes de escribirlos en Key Vault. El parámetro - VolumeType se establece en discos de datos y no en el disco del sistema operativo. Si la VM se cifró previamente con un tipo de volumen de "OS" o "All", entonces el parámetro --VolumeType se debe cambiar a All, de modo que se incluyan el nuevo disco de datos y el SO.

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data';
     ```


>[!NOTE]
> La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> </br>
La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

## <a name="disable-encryption-for-linux-vms"></a>Deshabilitación del cifrado para máquinas virtuales Linux
Puede deshabilitar el cifrado con Azure PowerShell, la CLI de Azure o una plantilla de Resource Manager. 

>[!IMPORTANT]
>Solo se puede deshabilitar el cifrado con Azure Disk Encryption en máquinas virtuales Linux para volúmenes de datos. No se admite en volúmenes de datos o volúmenes del sistema operativo si el volumen del sistema operativo se ha cifrado.  

- **Deshabilitar el cifrado de disco con Azure PowerShell:** para deshabilitar el cifrado, use el cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Deshabilitar el cifrado con la CLI de Azure:** para deshabilitar el cifrado, use el comando [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Deshabilitar el cifrado con una plantilla de Resource Manager:** use la plantilla [Deshabilitar el cifrado en una máquina virtual Linux en ejecución](https://aka.ms/decrypt-linuxvm).
     1. Haga clic en **Implementar en Azure**.
     2. Seleccione la suscripción, el grupo de recursos, la ubicación, la máquina virtual, los términos legales y el contrato.
     3.  Haga clic en **Comprar** para deshabilitar el cifrado de disco en una máquina virtual Windows en ejecución. 


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Habilitación de Azure Disk Encryption para Windows](azure-security-disk-encryption-windows-aad.md)
