---
title: Habilitación de Azure Disk Encryption para máquinas virtuales IaaS Linux
description: En este artículo se proporcionan instrucciones sobre cómo habilitar Microsoft Azure Disk Encryption para máquinas virtuales IaaS de Linux.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 2/5/2018
ms.custom: seodec18
ms.openlocfilehash: 9ce8484151d59eef50efe1ad0598f736752eb03e
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746694"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms"></a>Habilitación de Azure Disk Encryption para máquinas virtuales IaaS Linux 

Puede habilitar muchos escenarios de cifrado de disco, y los pasos pueden variar en función del escenario. En las secciones siguientes se tratan con más detalle los escenarios de máquinas virtuales IaaS de Linux. Antes de usar el cifrado de disco, es necesario satisfacer los [requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) y se debe revisar la sección de [requisitos previos adicionales para máquinas virtuales IaaS de Linux](azure-security-disk-encryption-prerequisites.md#bkmk_LinuxPrereq).

Tome una [instantánea](../virtual-machines/windows/snapshot-copy-managed-disk.md) o realice una copia de seguridad antes de cifrar los discos. Las copias de seguridad garantizan que, en caso de un error inesperado durante el cifrado, sea posible una opción de recuperación. Las máquinas virtuales con discos administrados requieren una copia de seguridad antes del cifrado. Una vez realizada la copia de seguridad, puede usar el cmdlet Set-AzureRmVMDiskEncryptionExtension para cifrar los discos administrados mediante la especificación del parámetro -skipVmBackup. Para más información sobre cómo realizar la copia de seguridad y restauración de máquinas virtuales cifradas, consulte el artículo sobre [Azure Backup](../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
> - Si ha usado anteriormente [Azure Disk Encryption con la aplicación Azure AD](azure-security-disk-encryption-prerequisites-aad.md) para cifrar esta VM, tendrá que seguir usando esta opción para cifrar la VM. No puede usar [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) en esta VM cifrada ya que no es un escenario compatible, lo que significa que el cambio desde la aplicación de AAD para esta VM cifrada aún no es compatible.
 > - Azure Disk Encryption requiere que Key Vault y las máquinas virtuales se coloquen en la misma región. Cree y use una instancia de Key Vault que se encuentre en la misma región que la máquina virtual que se va a cifrar.
> - Al cifrar los volúmenes del sistema operativo Linux, la máquina virtual no estará disponible y se deshabilitará el protocolo SSH. Para comprobar el progreso, puede usar los comandos [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) o [vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show). Este proceso puede tardar unas horas si trabaja con un volumen de sistema operativo de 30 GB; además, deberá tener en cuenta el tiempo necesario para realizar el cifrado de los volúmenes de datos. El tiempo de cifrado del volumen de datos será proporcional al tamaño y la cantidad de los volúmenes de datos, a menos que se use la opción "all" del formato de cifrado. 
> - Solo se puede deshabilitar el cifrado en máquinas virtuales Linux para volúmenes de datos. No se admite en volúmenes de datos o volúmenes del sistema operativo si el volumen del sistema operativo se ha cifrado.  


## <a name="bkmk_RunningLinux"></a> Habilitación del cifrado en una máquina virtual IaaS de Linux existente o en ejecución
En este escenario, puede habilitar el cifrado mediante la plantilla de Resource Manager, los cmdlets de PowerShell o los comandos de la CLI. Si necesita información de esquema para la extensión de máquina virtual, consulte el artículo [Extensión de Azure Disk Encryption para Linux](../virtual-machines/extensions/azure-disk-enc-linux.md).

>[!IMPORTANT]
 >Es obligatorio crear una instantánea o una copia de seguridad de una instancia de máquina virtual basada en un disco administrado fuera de Azure Disk Encryption y antes de habilitar esta característica. Puede tomar una instantánea del disco administrado desde el portal o se puede usar [Azure Backup](../backup/backup-azure-vms-encryption.md). Las copias de seguridad garantizan que es posible disponer de una opción de recuperación en el caso de que se produzca un error inesperado durante el cifrado. Una vez que se realiza una copia de seguridad, el cmdlet Set-AzureRmVMDiskEncryptionExtension puede utilizarse para cifrar los discos administrados especificando el parámetro -skipVmBackup. El comando Set-AzureRmVMDiskEncryptionExtension produce un error en las máquinas virtuales basadas en un disco administrado hasta que se realice una copia de seguridad y se especifique este parámetro. 
>
>Cifrar o deshabilitar el cifrado puede provocar el reinicio de la máquina virtual. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Habilitación del cifrado en una máquina virtual Linux existente o en ejecución mediante la CLI de Azure 
Puede habilitar el cifrado de disco en el disco duro virtual cifrado mediante la instalación y el uso de la herramienta de la línea de comandos [CLI de Azure 2.0](/cli/azure). Se puede usar en el explorador con [Azure Cloud Shell](../cloud-shell/overview.md), o lo puede instalar en el equipo local y usarlo en cualquier sesión de PowerShell. Para habilitar el cifrado en máquinas virtuales IaaS de Linux existentes o en ejecución en Azure, use los siguientes comandos:

Use el comando [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable) para habilitar el cifrado en una máquina virtual IaaS en ejecución en Azure.

-  **Cifrado de una máquina virtual en ejecución:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Cifrado de una máquina virtual en ejecución mediante KEK:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br>
La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Comprobar que los discos están cifrados:** para comprobar el estado de cifrado de una máquina virtual IaaS, use el comando [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show). 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Deshabilitar el cifrado:** para deshabilitar el cifrado, use el comando [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable). La deshabilitación del cifrado solo se permite en volúmenes de datos de máquinas virtuales Linux.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Habilitación del cifrado en una máquina virtual Linux existente o en ejecución mediante PowerShell
Use el cmdlet [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) para habilitar el cifrado en una máquina virtual IaaS en ejecución en Azure. 

-  **Cifrado de una máquina virtual en ejecución:** el siguiente script inicializa las variables y ejecuta el cmdlet Set-AzureRmVMDiskEncryptionExtension. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual y el almacén de claves. Reemplace MySecureRg, MySecureVM y MySecureVault por sus valores. Es posible que deba agregar el parámetro -VolumeType si va a cifrar discos de datos y no el disco del sistema operativo. 

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Cifrado de una máquina virtual en ejecución mediante KEK:** Es posible que deba agregar el parámetro -VolumeType si va a cifrar discos de datos y no el disco del sistema operativo. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
     ```

    >[!NOTE]
    > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Comprobar que los discos están cifrados:** para comprobar el estado de cifrado de una máquina virtual de IaaS, use el cmdlet [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus). 
    
     ```azurepowershell-interactive 
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName MySecureRg -VMName MySecureVM
     ```
    
- **Deshabilitar el cifrado de disco:** para deshabilitar el cifrado, use el cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). La deshabilitación del cifrado solo se permite en volúmenes de datos de máquinas virtuales Linux.
     
     ```azurepowershell-interactive 
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Habilitación del cifrado en una máquina virtual IaaS de Linux existente o en ejecución con una plantilla

Puede habilitar el cifrado de disco en una máquina virtual IaaS con Linux existente o en ejecución en Azure mediante la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad).

1. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).

2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, los parámetros, los términos legales y el contrato. Haga clic en **Crear** para habilitar el cifrado en la máquina virtual IaaS existente o en ejecución.

En la tabla siguiente figuran los parámetros de la plantilla de Resource Manager para máquinas virtuales existentes o en ejecución:

| Parámetro | DESCRIPCIÓN |
| --- | --- |
| vmName | Nombre de la máquina virtual para ejecutar la operación de cifrado. |
| keyVaultName | Nombre del almacén de claves en el que se que debe cargar la clave de BitLocker. Puede obtenerlo mediante el cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` o el comando de la CLI de Azure `az keyvault list --resource-group "MySecureGroup" |ConvertFrom-JSON`.|
| keyVaultResourceGroup | Nombre del grupo de recursos que contiene el almacén de claves|
|  keyEncryptionKeyURL | Dirección URL de la clave de cifrado de claves que se utiliza para cifrar la clave generada por BitLocker. Este parámetro es opcional si selecciona **nokek** en la lista desplegable de UseExistingKek. Si selecciona **kek** en la lista desplegable de UseExistingKek, debe proporcionar el valor de _keyEncryptionKeyURL_. |
| volumeType | Tipo de volumen en que se realiza la operación de cifrado. Los valores válidos son _SO_, _Datos_ y _Todo_. 
| forceUpdateTag | Cada vez que la operación tenga que ejecutarse, pase un valor único como GUID. |
| resizeOSDisk | Si se debería cambiar el tamaño de la partición del sistema operativo para ocupar el VHD del sistema operativo completo antes de dividir el volumen del sistema. |
| location | Ubicación para todos los recursos. |



## <a name="encrypt-virtual-machine-scale-sets"></a>Cifrado de conjuntos de escalado de máquinas virtuales
Los [conjuntos de escalado de máquinas virtuales de Azure](../virtual-machine-scale-sets/overview.md) permiten crear y administrar un grupo de máquinas virtuales idénticas con equilibrio de carga. El número de instancias de máquina virtual puede aumentar o disminuir automáticamente según la demanda, o de acuerdo a una programación definida. Use la CLI o Azure PowerShell para cifrar los conjuntos de escalado de máquinas virtuales.

[Aquí](https://github.com/Azure-Samples/azure-cli-samples/tree/master/disk-encryption/vmss) se puede encontrar un ejemplo de archivo por lotes para el cifrado de discos de datos de un conjunto de escalado de Linux. En este ejemplo se crea un grupo de recursos y un conjunto de escalado de Linux, se monta un disco de datos de 5 GB y se cifra el conjunto de escalado de máquinas virtuales.

### <a name="register-for-disk-encryption-preview-using-azure-cli"></a>Registro para la versión preliminar de cifrado de discos con la CLI de Azure.

El cifrado de discos de Azure para conjuntos de escalado de máquinas virtuales (versión preliminar) exige el autorregistro de la suscripción con [az feature register](/cli/azure/feature#az-feature-register). Solo tiene que realizar los siguientes pasos la primera vez que use la característica de cifrado de discos (versión preliminar):

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

La solicitud de registro puede tardar hasta diez minutos en propagarse. Puede comprobar el estado de registro con [az feature show](/cli/azure/feature#az-feature-show). Cuando `State` indique *Registered* (Registrado), vuelva a registrar el proveedor *Microsoft.Compute* con [az provider register](/cli/azure/provider#az-provider-register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Cifrado de conjuntos de escalado de máquinas virtuales con la CLI de Azure
Use [az vmss encryption enable](/cli/azure/vmss/encryption#az-vmss-encryption-enable) para habilitar el cifrado en un conjunto de escalado de máquinas virtuales Windows. Si establece en manual la directiva de actualización en el conjunto de escalado, inicie el cifrado con [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual y el almacén de claves. 

-  **Cifrado de un conjunto de escalado de máquinas virtuales en ejecución**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Cifrado de un conjunto de escalado de máquinas virtuales en ejecución con KEK para ajustar la clave**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 
     ```

    >[!NOTE]
    > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Obtener el estado de cifrado para un conjunto de escalado de máquinas virtuales:** use [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show).

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Deshabilitar el cifrado en un conjunto de escalado de máquinas virtuales**: use [az vmss encryption disable](/cli/azure/vmss/encryption#az-vmss-encryption-disable).
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

### <a name="register-for-disk-encryption-preview-using-azure-powershell"></a>Registro para la versión preliminar de cifrado de discos con Azure PowerShell

El cifrado de discos de Azure para conjuntos de escalado de máquinas virtuales (versión preliminar) permite autorregistrar la suscripción con [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). Solo tiene que realizar los siguientes pasos la primera vez que use la característica de cifrado de discos (versión preliminar):

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

La solicitud de registro puede tardar hasta 10 minutos en propagarse. Puede comprobar el estado de registro con [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Cuando `RegistrationState` indique *Registered* (Registrado), vuelva a registrar el proveedor *Microsoft.Compute* con [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Cifrado de conjuntos de escalado de máquinas virtuales con Azure PowerShell

Use el cmdlet [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension) para habilitar el cifrado en un conjunto de escalado de máquinas virtuales Windows. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual y el almacén de claves.

-  **Cifrado de un conjunto de escalado de máquinas virtuales en ejecución**:
    ```powershell
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```

-  **Cifrado de un conjunto de escalado de máquinas virtuales en ejecución mediante KEK para ajustar la clave**:
    ```powershell
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $KeyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

    >[!NOTE]
    > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Obtener el estado de cifrado para un conjunto de máquinas virtuales**: use el cmdlet [Get-AzureRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption).
    
    ```powershell
    get-AzureRmVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Deshabilitar el cifrado en un conjunto de escalado de máquinas virtuales**: use el cmdlet [Disable-AzureRmVmssDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption). 

    ```powershell
    Disable-AzureRmVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-linux-virtual-machine-scale-sets"></a>Plantillas de Azure Resource Manager para conjuntos de escalado de máquinas virtuales Linux

Para cifrar o descifrar conjuntos de escalado de máquinas virtuales Linux, use las plantillas de Azure Resource Manager y las instrucciones siguientes:

- [Habilitación del cifrado en conjuntos de escalado de máquinas virtuales Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)
- [Implementación de un conjunto de escalado de máquinas virtuales Linux con JumpBox y habilitar su cifrado en el conjunto de escalado de máquinas virtuales Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)
- [Deshabilitación del cifrado en un conjunto de escalado de máquinas virtuales Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

     1. Haga clic en **Implementar en Azure**.
     2. Rellene los campos obligatorios y acepte los términos y condiciones.
     3. Haga clic en **Comprar** para implementar la plantilla.

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

### <a name="bkmk_EFAPSH"> </a> Uso del parámetro EncryptFormatAll con la CLI de Azure
Use el comando [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable) para habilitar el cifrado en una máquina virtual IaaS en ejecución en Azure.

-  **Cifrar una máquina virtual en ejecución mediante EncryptFormatAll:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="bkmk_EFAPSH"> </a> Uso del parámetro EncryptFormatAll con un cmdlet de PowerShell
Use el cmdlet [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) con el [parámetro EncryptFormatAll](https://www.powershellgallery.com/packages/AzureRM/5.0.0). 

**Cifrar una máquina virtual en ejecución mediante EncryptFormatAll:** como ejemplo, el siguiente script inicializa las variables y ejecuta el cmdlet Set-AzureRmVMDiskEncryptionExtension con el parámetro EncryptFormatAll. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual y el almacén de claves. Reemplace MySecureRg, MySecureVM y MySecureVault por sus valores.
  
   ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MySecureVM';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
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
         Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. Configure LVM encima de estos nuevos discos. Tenga en cuenta que las unidades cifradas se desbloquean después de que la máquina virtual ha terminado de arrancar. Por lo tanto, el montaje de LVM también tendrá que retrasarse.


## <a name="bkmk_VHDpre"> </a> Máquinas virtuales IaaS creadas a partir de discos duros virtuales cifrados y claves de cifrado
En este escenario, puede habilitar el cifrado mediante los cmdlets de PowerShell o los comandos de la CLI. 

Use las instrucciones del apéndice para preparar imágenes previamente cifradas que se pueden usar en Azure. Una vez creada la imagen, puede seguir los pasos de la sección siguiente para crear una VM cifrada de Azure.

* [Preparación de un VHD con Windows precifrado](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Preparación de un VHD con Linux precifrado](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Es obligatorio crear una instantánea o una copia de seguridad de una instancia de máquina virtual basada en un disco administrado fuera de Azure Disk Encryption y antes de habilitar esta característica. Puede tomar una instantánea del disco administrado desde el portal o se puede usar [Azure Backup](../backup/backup-azure-vms-encryption.md). Las copias de seguridad garantizan que es posible disponer de una opción de recuperación en el caso de que se produzca un error inesperado durante el cifrado. Una vez que se realiza una copia de seguridad, el cmdlet Set-AzureRmVMDiskEncryptionExtension puede utilizarse para cifrar los discos administrados especificando el parámetro -skipVmBackup. El comando Set-AzureRmVMDiskEncryptionExtension produce un error en las máquinas virtuales basadas en un disco administrado hasta que se realice una copia de seguridad y se especifique este parámetro. 
>
>Cifrar o deshabilitar el cifrado puede provocar el reinicio de la máquina virtual. 



### <a name="bkmk_VHDprePSH"> </a> Uso de Azure PowerShell para cifrar las máquinas virtuales IaaS con discos duros virtuales previamente cifrados 
Puede habilitar el cifrado de disco en el disco duro virtual cifrado mediante el cmdlet [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples) de PowerShell. En el ejemplo siguiente se proporcionan algunos parámetros comunes. 

```azurepowershell-interactive
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Habilitación del cifrado en un disco de datos recién agregado

Puede agregar un nuevo disco de datos mediante [az vm disk attach](../virtual-machines/linux/add-disk.md), o [Azure Portal](../virtual-machines/linux/attach-disk-portal.md). Para poder cifrarlo, deberá montar primero el disco de datos recién asociado. Dado que la unidad de datos no se puede usar mientras el cifrado está en curso, deberá solicitar el cifrado de la unidad. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Habilitación del cifrado en un disco recién agregado con la CLI de Azure

 Si la VM se cifró previamente con "All", entonces el parámetro --volume-type debe permanecer como All. All incluye los discos de datos y del SO. Si la VM se cifró previamente con un tipo de volumen de "OS", entonces el parámetro--volume-type se debe cambiar a All, de modo que se incluyan el nuevo disco de datos y el SO. Si la VM se cifró con el tipo de volumen de "Data", entonces puede permanecer como "Data", tal y como se muestra a continuación. Para preparar el cifrado, no es suficiente con agregar y asociar un nuevo disco de datos a una VM. El disco recién conectado también se debe formatear y montar correctamente en la VM antes de habilitar el cifrado. En Linux, el disco se debe montar en /etc/fstab con un [nombre de dispositivo de bloque persistente](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

A diferencia de la sintaxis de PowerShell, la CLI no requiere que el usuario proporcione ninguna versión de secuencia única cuando se habilita el cifrado. La CLI genera y usa su propio valor de versión de secuencia único automáticamente.

-  **Cifrar los volúmenes de datos de una VM en ejecución:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Cifrar los volúmenes de datos de una VM con KEK:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Habilitación del cifrado en un disco recién agregado con Azure PowerShell
 Al usar PowerShell para cifrar un nuevo disco para Linux, se debe especificar una nueva versión de secuencia. La versión de secuencia debe ser única. El siguiente script genera un GUID para la versión de secuencia. 
 

-  **Cifrar los volúmenes de datos de una VM en ejecución:** el siguiente script inicializa las variables y ejecuta el cmdlet Set-AzureRmVMDiskEncryptionExtension. Ya se deben haber satisfecho los requisitos previos de crear el grupo de recursos, la máquina virtual y el almacén de claves. Reemplace MySecureRg, MySecureVM y MySecureVault por sus valores. Los valores aceptables para el parámetro -VolumeType son All, OS y Data. Si la VM se cifró previamente con un tipo de volumen de "OS" o "All", entonces el parámetro --VolumeType se debe cambiar a All, de modo que se incluyan el nuevo disco de datos y el SO.

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
    ```
- **Cifrar los volúmenes de datos de una VM con KEK:** Los valores aceptables para el parámetro -VolumeType son All, OS y Data. Si la VM se cifró previamente con un tipo de volumen de "OS" o "All", entonces el parámetro --VolumeType se debe cambiar a All, de modo que se incluyan el nuevo disco de datos y el SO.

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data';

     ```

    >[!NOTE]
    > La sintaxis del valor del parámetro disk-encryption-keyvault es la cadena completa del identificador: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> La sintaxis del valor del parámetro key-encryption-key es el URI completo de KEK como en: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


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
- **Deshabilitar el cifrado con una plantilla de Resource Manager:** use la plantilla [Deshabilitación del cifrado en una máquina virtual Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) para deshabilitar el cifrado.
     1. Haga clic en **Implementar en Azure**.
     2. Seleccione la suscripción, el grupo de recursos, la ubicación, la máquina virtual, los términos legales y el contrato.
     3.  Haga clic en **Comprar** para deshabilitar el cifrado de disco en una máquina virtual Windows en ejecución. 


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Habilitación de Azure Disk Encryption para Windows](azure-security-disk-encryption-windows.md)
