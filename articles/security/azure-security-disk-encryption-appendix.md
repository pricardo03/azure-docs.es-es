---
title: Azure Disk Encryption para máquinas virtuales IaaS con Linux y Windows| Microsoft Docs
description: Este artículo es el apéndice de Microsoft Azure Disk Encryption para máquinas virtuales IaaS de Windows y Linux.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 11/06/2018
ms.openlocfilehash: 5d72d883b8d28e1ca71ea9c69488da5e1659e407
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219688"
---
# <a name="appendix-for-azure-disk-encryption"></a>Apéndice de Azure Disk Encryption 
Este artículo es un apéndice de [Azure Disk Encryption para máquinas virtuales IaaS](azure-security-disk-encryption-overview.md). Asegúrese de leer los artículos sobre Azure Disk Encryption para máquinas virtuales IaaS para entender el contexto. En este artículo se describe cómo preparar discos duros virtuales previamente cifrados y otras tareas.

## <a name="connect-to-your-subscription"></a>su suscripción
Antes de empezar, revise el artículo de [Requisitos previos](azure-security-disk-encryption-prerequisites.md). Cuando se hayan cumplido todos los requisitos previos, conéctese a su suscripción mediante la ejecución de los siguientes cmdlets:

### <a name="bkmk_ConnectPSH"></a> Conexión a la suscripción con PowerShell

1. Inicie una sesión de Azure PowerShell e inicie sesión en su cuenta de Azure con el siguiente comando:

     ```powershell
     Connect-AzureRmAccount 
     ```
2. Si tiene varias suscripciones y desea especificar que se use una en concreto, escriba lo siguiente para ver las suscripciones de su cuenta:
     
     ```powershell
     Get-AzureRmSubscription
     ```
3. Para especificar la suscripción que desea usar, escriba:
 
     ```powershell
      Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>
     ```
4. Para comprobar que la suscripción configurada es correcta, escriba:
     
     ```powershell
     Get-AzureRmSubscription
     ```
5. Si es necesario, conéctese a Azure AD con [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```
6. Para confirmar que están instalados los cmdlets de Azure Disk Encryption, escriba:
     
     ```powershell
     Get-command *diskencryption*
     ```
                       
7. Revise [Introducción a Azure PowerShell](/powershell/azure/get-started-azureps) y [AzureAD](/powershell/module/azuread), si es necesario.

### <a name="bkmk_ConnectCLI"></a> Conexión a la suscripción con la CLI de Azure

1. Inicie sesión en Azure con [az login](/cli/azure/authenticate-azure-cli#interactive-log-in). 
     
     ```azurecli
     az login
     ```

2. Si quiere seleccionar un inquilino con el que iniciar sesión, use:
    
     ```azurecli
     az login --tenant <tenant>
     ```

3. Si tiene varias suscripciones y quiere especificar una en concreto, use [az account list](/cli/azure/account#az-account-list) para obtener la lista de suscripciones y [az account set](/cli/azure/account#az-account-set) para especificar cuál.
     
     ```azurecli
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Compruebe la versión instalada.
     
     ```azurecli
        az --version
     ``` 

5. Revise [Introducción a la CLI de Azure 2.0](/cli/azure/get-started-with-azure-cli), si es necesario. 

## <a name="sample-powershell-scripts-for-azure-disk-encryption"></a>Scripts de ejemplo de PowerShell para Azure Disk Encryption 

- **Enumerar todas las máquinas virtuales cifradas en la suscripción**

     ```azurepowershell-interactive
     $osVolEncrypted = {(Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).OsVolumeEncrypted}
     $dataVolEncrypted= {(Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).DataVolumesEncrypted}
     Get-AzureRmVm | Format-Table @{Label="MachineName"; Expression={$_.Name}}, @{Label="OsVolumeEncrypted"; Expression=$osVolEncrypted}, @{Label="DataVolumesEncrypted"; Expression=$dataVolEncrypted}
     ```

- **Enumerar todos los secretos de cifrado de disco usados para cifrar las máquinas virtuales en un almacén de claves** 

     ```azurepowershell-interactive
     Get-AzureKeyVaultSecret -VaultName $KeyVaultName | where {$_.Tags.ContainsKey('DiskEncryptionKeyFileName')} | format-table @{Label="MachineName"; Expression={$_.Tags['MachineName']}}, @{Label="VolumeLetter"; Expression={$_.Tags['VolumeLetter']}}, @{Label="EncryptionKeyURL"; Expression={$_.Id}}
     ```

### <a name="bkmk_prereq-script"></a> Uso del script de PowerShell de requisitos previos de Azure Disk Encryption
Si ya está familiarizado con los requisitos previos para Azure Disk Encryption, puede usar el [script de PowerShell de requisitos previos de Azure Disk Encryption](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Para un ejemplo de uso de este script de PowerShell, consulte la [guía de inicio rápido para el cifrado de máquinas virtuales](quick-encrypt-vm-powershell.md). Puede quitar los comentarios de una sección del script, a partir de la línea 211, para cifrar todos los discos de las máquinas virtuales de un grupo de recursos existente. 

En la siguiente tabla se muestran los parámetros que se pueden usar en el script de PowerShell: 


|Parámetro|DESCRIPCIÓN|Obligatorio|
|------|------|------|
|$resourceGroupName| Nombre del grupo de recursos al que pertenece la instancia de KeyVault.  Si no existe, se creará un grupo de recursos con este nombre.| True|
|$keyVaultName|Nombre de la instancia de KeyVault en donde se colocarán las claves de cifrado. Si no existe, se creará un almacén con este nombre.| True|
|$location|Ubicación de la instancia de KeyVault. Asegúrese de que la instancia de KeyVault y las máquinas virtuales que se van a cifrar están en la misma ubicación. Obtenga una lista de ubicaciones con `Get-AzureRMLocation`.|True|
|$subscriptionId|Identificador de la suscripción de Azure que se va a utilizar.  El identificador de la suscripción se puede obtener con `Get-AzureRMSubscription`.|True|
|$aadAppName|Nombre de la aplicación de Azure AD que se va a utilizar para escribir secretos en KeyVault. Si no existe, se creará una aplicación con este nombre. Si esta aplicación ya existe, pasará el parámetro aadClientSecret al script.|False|
|$aadClientSecret|Secreto de cliente de la aplicación de Azure AD que se creó anteriormente.|False|
|$keyEncryptionKeyName|Nombre de la clave de cifrado de clave opcional en KeyVault. Si no existe, se creará una clave con este nombre.|False|


## <a name="resource-manager-templates"></a>Plantillas de Resource Manager

<!--   - [Create a key vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) -->

### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Cifrado o descifrado de máquinas virtuales sin aplicación de Azure AD


- [Habilitar el cifrado de disco en máquinas virtuales IaaS de Windows existentes o en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)
- [Deshabilitar el cifrado de disco en máquinas virtuales IaaS de Windows existentes o en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad)
- [Habilitar el cifrado de disco en una máquina virtual IaaS de Linux existente o en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)  
 -  [Deshabilitar el cifrado en una máquina virtual Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) 
    - La deshabilitación del cifrado solo se permite en volúmenes de datos de máquinas virtuales Linux.  

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>Cifrado o descifrado de máquinas virtuales con aplicación de Azure AD (versión anterior) 
 
- [Habilitar el cifrado de disco en máquinas virtuales IaaS de Windows existentes o en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)

- [Habilitar el cifrado de disco en una máquina virtual IaaS de Linux existente o en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)    

- [Deshabilitar el cifrado de disco en máquinas virtuales IaaS de Windows en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) 

-  [Deshabilitar el cifrado en una máquina virtual Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm) 
    - La deshabilitación del cifrado solo se permite en volúmenes de datos de máquinas virtuales Linux. 

- [Habilitar el cifrado de discos en nuevas máquinas virtuales IaaS de Windows desde Marketplace](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)
    - Esta plantilla crea una nueva máquina virtual con Windows cifrada que usa una imagen de la galería de Windows Server 2012.

- [Crear una máquina virtual de disco administrado IaaS de Windows a partir de una imagen de la galería](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)
    - Esta plantilla crea una máquina virtual Windows cifrada con discos administrados mediante la imagen de la galería de Windows Server 2012.

- [Implementación de RHEL 7.2 con cifrado de disco completo con discos administrados](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)
    - Con esta plantilla se crea una máquina virtual RHEL 7.2 completamente cifrada en Azure mediante discos administrados. Incluye una unidad de sistema operativo cifrada de 30 GB y una matriz cifrada de 200 GB (RAID-0) montada en /mnt/raidencrypted. Consulte el artículo de [P+F](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) para ver las distribuciones de servidor de Linux admitidas. 

- [Implementación de RHEL 7.2 con cifrado de disco completo con discos no administrados](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel-unmanaged)
    - Con esta plantilla se crea una máquina virtual RHEL 7.2 totalmente cifrada en Azure con una unidad de sistema operativo cifrada de 30 GB y una matriz cifrada de 200 GB (RAID-0) montada en /mnt/raidencrypted. Consulte el artículo de [P+F](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) para ver las distribuciones de servidor de Linux admitidas. 

- [Habilitar el cifrado de disco en un disco duro virtual de Windows o Linux previamente cifrado](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm)

- [Crear un disco administrado cifrado a partir de un blob de almacenamiento o disco duro virtual previamente cifrado](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Crea un disco administrado cifrado a partir de un disco duro virtual previamente cifrado y su correspondiente configuración de cifrado.

- [Habilitar el cifrado de disco en una máquina virtual Windows en ejecución mediante huella digital del certificado de cliente de Azure AD](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-aad-client-cert)
    
- [Habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Windows en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

 - [Implementar un conjunto de escalado de máquinas virtuales Linux con JumpBox y habilitar su cifrado](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

 - [Implementar un conjunto de escalado de máquinas virtuales Windows con JumpBox y habilitar su cifrado](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Deshabilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Deshabilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Windows en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

## <a name="bkmk_preWin"></a> Preparación de un disco duro virtual previamente cifrado
Las secciones siguientes son necesarias para preparar un VHD con Windows precifrado para implementarlo como VHD cifrado en IaaS de Azure. Utilice la información para preparar y arrancar una máquina virtual (VHD) con Windows nueva en Azure Site Recovery o Azure. Para más información sobre cómo preparar y cargar un disco duro virtual, consulte [Carga de un VHD generalizado y su uso para crear máquinas virtuales nuevas en Azure](../virtual-machines/windows/upload-generalized-managed.md).

### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Actualización de una directiva de grupo para permitir un módulo no TPM para la protección del sistema operativo
Configure la opción de la directiva de grupos de BitLocker denominada **Cifrado de unidad BitLocker**, que se encuentra en **Directiva de equipo local** > **Configuración del equipo** > **Plantillas administrativas** > **Componentes de Windows**. Cambie esta opción a: **Unidades del sistema operativo** > **Requerir autenticación adicional al iniciar** > **Permitir BitLocker sin un TPM compatible**, como se muestra en la ilustración siguiente:

![Microsoft Antimalware en Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

### <a name="install-bitlocker-feature-components"></a>Instalación de componentes de características de BitLocker
Para Windows Server 2012 y versiones posteriores, utilice el siguiente comando:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Para Windows Server 2008 R2, utilice el siguiente comando:

    ServerManagerCmd -install BitLockers
### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Preparación del volumen del sistema operativo para BitLocker mediante `bdehdcfg`
Si es necesario, ejecute el comando [bdehdcfg](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-basic-deployment#using-bitlocker-to-encrypt-volumescommand) para comprimir la partición del sistema operativo y preparar la máquina para BitLocker:

    bdehdcfg -target c: shrink -quiet 


### <a name="protect-the-os-volume-by-using-bitlocker"></a>Protección del volumen del sistema operativo mediante BitLocker
Use el comando [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) para habilitar el cifrado en el volumen de arranque mediante un protector de clave externo. También puede colocar la clave externa (archivo .bek) en el disco externo o el volumen. El cifrado se habilita en el volumen de sistema o de arranque la próxima vez que se reinicie el equipo.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> Prepare la máquina virtual con un VHD de datos o recursos separado para obtener la clave externa mediante BitLocker.

## <a name="bkmk_LinuxRunning"></a> Cifrado de la unidad del sistema operativo en una máquina virtual Linux en ejecución

### <a name="prerequisites-for-os-disk-encryption"></a>Requisitos previos para el cifrado de disco del sistema operativo

* La máquina virtual debe usar una distribución compatible con el cifrado de disco de SO como se muestra en el artículo de [preguntas más frecuentes sobre Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). 
* La máquina virtual debe crearse a partir de la imagen de Marketplace en Azure Resource Manager.
* La máquina virtual de Azure con al menos 4 GB de RAM (el tamaño recomendado es 7 GB).
* (Para RHEL y CentOS) Deshabilite SELinux. Para deshabilitar SELinux, consulte "4.4.2. Disabling SELinux" (Deshabilitar SELinux) en el documento [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) (Guía del administrador y del usuario de SELinux) en la máquina virtual.
* Después de deshabilitar SELinux, reinicie la máquina virtual al menos una vez.

### <a name="steps"></a>Pasos
1. Cree una máquina virtual mediante una de las distribuciones especificadas anteriormente.

 Para CentOS 7.2, se admite el cifrado de disco del sistema operativo a través de una imagen especial. Para usar esta imagen, especifique "7.2n" como SKU al crear la máquina virtual:

 ```powershell
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Configure la máquina virtual según sus necesidades. Si va a cifrar todas las unidades (sistema operativo y datos), las unidades de datos tienen que especificarse y montarse desde /etc/fstab.

 > [!NOTE]
 > Use UUID=... para especificar unidades de datos en /etc/fstab en lugar de especificar el nombre del dispositivo de bloque (por ejemplo, /dev/sdb1). Durante el cifrado, se cambia el orden de las unidades en la máquina virtual. Si la máquina virtual depende de un orden específico de dispositivos de bloque, se producirá un error de montaje después del cifrado.

3. Cierre las sesiones de SSH.

4. Para cifrar el sistema operativo, especifique volumeType como **All** u **OS** al habilitar el cifrado.

 > [!NOTE]
 > Todos los procesos de espacio de usuario que no se ejecutan como servicios `systemd` deben terminarse con `SIGKILL`. Reinicie la máquina virtual. Al habilitar el cifrado del disco de sistema operativo en una máquina virtual en ejecución, tenga en cuenta el tiempo de inactividad de la máquina virtual.

5. Supervise periódicamente el progreso del cifrado con las instrucciones de la [sección siguiente](#monitoring-os-encryption-progress).

6. Cuando Get-AzureRmVmDiskEncryptionStatus muestre "VMRestartPending", inicie sesión en la máquina virtual para reiniciarla o hágalo mediante el portal, PowerShell o la CLI.
    ```powershell
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Antes de reiniciar, se recomienda que guarde los [diagnósticos de arranque](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) de la máquina virtual.

## <a name="monitoring-os-encryption-progress"></a>Supervisión del progreso de cifrado del sistema operativo
Hay tres maneras de supervisar el progreso de cifrado del sistema operativo:

* Use el cmdlet `Get-AzureRmVmDiskEncryptionStatus` e inspeccione el campo ProgressMessage:
    ```powershell
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Una vez que la máquina virtual llega al mensaje de inicio del cifrado de disco del sistema operativo, el proceso tarda aproximadamente entre 40 y 50 minutos en una máquina virtual de almacenamiento Premium.

 Debido al [problema 388](https://github.com/Azure/WALinuxAgent/issues/388) de WALinuxAgent, `OsVolumeEncrypted` y `DataVolumesEncrypted` se muestran como `Unknown` en algunas distribuciones. En WALinuxAgent versión 2.1.5 y versiones posteriores, este problema se ha corregido automáticamente. En caso de que se vea `Unknown` en la salida, puede comprobar el estado de cifrado del disco mediante el Explorador de recursos de Azure.

 Vaya al [Explorador de recursos de Azure](https://resources.azure.com/) y expanda esta jerarquía en el panel de selección a la izquierda:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 En el objeto InstanceView, desplácese hacia abajo para ver el estado de cifrado de las unidades de disco.

 ![Vista de instancia de máquina virtual](./media/azure-security-disk-encryption/vm-instanceview.png)

* Fíjese en los [diagnósticos de arranque](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). Los mensajes de la extensión ADE deben llevar el prefijo `[AzureDiskEncryption]`.

* Inicie sesión en la máquina virtual a través de SSH y obtenga el registro de la extensión de:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Se recomienda que no inicie sesión en la máquina virtual con el cifrado del sistema operativo en curso. Copie los registros solo cuando los otros dos métodos no den resultado.

## <a name="bkmk_preLinux"></a> Preparación de un disco duro virtual Linux previamente cifrado
La preparación de disco duros virtuales previamente cifrados puede variar dependiendo de la distribución. Se pueden encontrar ejemplos sobre cómo preparar [Ubuntu 16](#bkmk_Ubuntu), [openSUSE 13.2](#bkmk_openSUSE) y [CentOS 7](#bkmk_CentOS). 

### <a name="bkmk_Ubuntu"></a> Ubuntu 16
Configure el cifrado durante la instalación de la distribución; para ello, siga estos pasos:

1. Seleccione **Configure encrypted volumes** (Configurar volúmenes cifrados) al realizar la partición de los discos.

 ![Instalación de Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Cree una unidad de arranque independiente, que no debe estar cifrada. Cifre la unidad raíz.

 ![Instalación de Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Proporcione una frase de contraseña. Se trata de la frase de contraseña que se carga en el almacén de claves.

 ![Instalación de Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Finalice la partición.

 ![Instalación de Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Cuando arranque la máquina virtual y se le pida una frase de contraseña, utilice la frase de contraseña que especificó en el paso 3.

 ![Instalación de Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Prepare la máquina virtual para su carga en Azure con [estas instrucciones](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). No ejecute todavía el último paso (desaprovisionamiento de la máquina virtual).

Configure el cifrado para que funcione con Azure de esta manera:

1. Cree un archivo en /usr/local/sbin/azure_crypt_key.sh con el contenido del script siguiente. Preste atención a KeyFileName, porque es el nombre que Azure ha asignado al archivo de frase de contraseña.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. Cambie la configuración de cifrado en */etc/crypttab*. Debería ser parecido a este:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Si está editando *azure_crypt_key.sh* en Windows y lo ha copiado en Linux, ejecute `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Agregue permisos de ejecución al script:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Edite */etc/initramfs-tools/modules* anexando líneas:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Ejecute `update-initramfs -u -k all` para actualizar initramfs a fin de que `keyscript` surta efecto.

7. Ahora puede desaprovisionar la máquina virtual.

 ![Instalación de Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Continúe con el paso siguiente y cargue el disco duro virtual en Azure.

### <a name="bkmk_openSUSE"></a>  openSUSE 13.2
Para configurar el cifrado durante la instalación de distribución, siga estos pasos:
1. Al crear particiones de los discos, seleccione **Encrypt Volume Group** (Cifrar el grupo de volúmenes) y, a continuación, escriba una contraseña. Esta es la contraseña que se cargará en el almacén de claves.

 ![Instalación de openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Arranque la máquina virtual con la contraseña.

 ![Instalación de openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Prepare la máquina virtual para cargarla en Azure con las instrucciones de [Preparación de una máquina virtual SLES u openSUSE para Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). No ejecute todavía el último paso (desaprovisionamiento de la máquina virtual).

Para configurar el cifrado de manera que funcione con Azure, siga estos pasos:
1. Edite /etc/dracut.conf y agregue la línea siguiente:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Agregue comentarios a estas líneas al final del archivo /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. Agregue la siguiente línea al principio del archivo /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:
 ```
    DRACUT_SYSTEMD=0
 ```
Y cambie todas las apariciones de:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
a:
```
    if [ 1 ]; then
```
4. Edite /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh y anéxelo a "# Open LUKS device":

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Ejecute `/usr/sbin/dracut -f -v` para actualizar el initrd.

6. Ahora puede desaprovisionar la máquina virtual y cargar el disco duro virtual en Azure.

### <a name="bkmk_CentOS"></a> CentOS 7
Para configurar el cifrado durante la instalación de distribución, siga estos pasos:
1. Seleccione **Encrypt my data** (Cifrar mis datos) al crear particiones en discos.

 ![Instalación de centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Asegúrese de que **Encrypt** (Cifrar) está seleccionado para la partición raíz.

 ![Instalación de centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Proporcione una frase de contraseña. Se trata de la frase de contraseña que va a cargar en el almacén de claves.

 ![Instalación de centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Cuando arranque la máquina virtual y se le pida una frase de contraseña, utilice la frase de contraseña que especificó en el paso 3.

 ![Instalación de centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Prepare la máquina virtual para cargarla en Azure con las instrucciones de "CentOS 7.0+" en [Preparación de una máquina virtual basada en CentOS para Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). No ejecute todavía el último paso (desaprovisionamiento de la máquina virtual).

6. Ahora puede desaprovisionar la máquina virtual y cargar el disco duro virtual en Azure.

Para configurar el cifrado de manera que funcione con Azure, siga estos pasos:

1. Edite /etc/dracut.conf y agregue la línea siguiente:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Agregue comentarios a estas líneas al final del archivo /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. Agregue la siguiente línea al principio del archivo /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:
```
    DRACUT_SYSTEMD=0
```
Y cambie todas las apariciones de:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
to
```
    if [ 1 ]; then
```
4. Edite /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh y anexe lo siguiente después de "# Open LUKS device":
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Ejecute "/usr/sbin/dracut -f -v" para actualizar initrd.

![Instalación de centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

## <a name="bkmk_UploadVHD"></a> Carga de un disco duro virtual cifrado en una cuenta de almacenamiento de Azure
Cuando el cifrado de BitLocker o el cifrado DM-Crypt están habilitados, el VHD cifrado local cifrado tiene que cargarse en la cuenta de almacenamiento.
```powershell
    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```
## <a name="bkmk_UploadSecret"></a> Carga del secreto de la máquina virtual previamente cifrada en el almacén de claves
Al cifrar mediante una aplicación de Azure AD (versión anterior), el secreto de cifrado del disco que obtuvo con anterioridad se debe cargar como secreto en el almacén de claves. El almacén de claves debe tener habilitados el cifrado de discos y los permisos para el cliente de Azure AD.

```powershell 
 $AadClientId = "My-AAD-Client-Id"
 $AadClientSecret = "My-AAD-Client-Secret"

 $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

 Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
 Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption
``` 

### <a name="bkmk_SecretnoKEK"></a> Secreto del cifrado de disco no cifrado con KEK
Para configurar el secreto en el almacén de claves, use [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Si tiene una máquina virtual con Windows, el archivo bek se codifica como cadena base64 y, a continuación, se carga en el almacén de claves mediante el cmdlet `Set-AzureKeyVaultSecret`. En Linux, la frase de contraseña se codifica como cadena base64 y, a continuación, se carga en el almacén de claves. Además, asegúrese de que se establecen las siguientes etiquetas al crear el secreto en el almacén de claves.

#### <a name="windows-bek-file"></a>Archivo BEK de Windows
```powershell
# Change the VM Name, key vault name, and specify the path to the BEK file.
$VMName ="MySecureVM"
$BEKFilepath = "C:\test\BEK\E60CF855-1B47-4AE5-A70C-4FE6E8386AAA.BEK"
$VeyVaultName ="MySecureVault"

# Get the name of the BEK file from the BEK file path. This will be a tag for the key vault secret.
$BEKFileName =  Split-Path $BEKFilepath -Leaf

# These tags will be added to the key vault secret so you can easily see which BEK file belongs to which VM.
$tags = @{“MachineName” = “$VMName”;"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "$BEKFileName"}

# Convert the BEK file to a Base64 string.
$FileContentEncoded = [System.convert]::ToBase64String((Get-Content -Path $BEKFilepath -Encoding Byte))

# Create a new secret in the vault from the converted BEK file. 
# The file is converted to a secure string before import into the key vault

$SecretName = [guid]::NewGuid().ToString()
$SecureSecretValue = ConvertTo-SecureString $FileContentEncoded -AsPlainText -Force
$Secret = Set-AzureKeyVaultSecret -VaultName $VeyVaultName -Name $SecretName -SecretValue $SecureSecretValue -tags $tags

# Show the secret's URL and store it as a variable. This is used as -DiskEncryptionKeyUrl in Set-AzureRmVMOSDisk when you attach your OS disk. 
$SecretUrl=$secret.Id
$SecretUrl
```

#### <a name="linux"></a>Linux
```powershell

 # This is the passphrase that was provided for encryption during the distribution installation
 $passphrase = "contoso-password"

 $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
 $secretName = [guid]::NewGuid().ToString()
 $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
 $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

 $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
 $secretUrl = $secret.Id
```


Use `$secretUrl` en el paso siguiente para [conectar el disco del sistema operativo sin usar KEK](#bkmk_URLnoKEK).

### <a name="bkmk_SecretKEK"></a> Secreto del cifrado de disco cifrado con KEK
Antes de cargar el secreto en el almacén de claves, puede cifrarlo si lo desea mediante una clave de cifrado de claves. Utilice la [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) de encapsulamiento para cifrar por primera vez el secreto mediante la clave de cifrado de claves. El resultado de esta operación de encapsulamiento es una cadena codificada en URL como base64 que luego se carga como secreto con el cmdlet [`Set-AzureKeyVaultSecret`](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).

```powershell
    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id
```

Use `$KeyEncryptionKey` y `$secretUrl` en el paso siguiente para [conectar el disco del sistema operativo usando KEK](#BKMK_URLKEK).

##  <a name="bkmk_SecretURL"></a> Especificación de una dirección URL de secreto al asociar un disco del sistema operativo

###  <a name="bkmk_URLnoKEK"></a> Sin KEK
Mientras esté adjuntando el disco del sistema operativo, tiene que pasar `$secretUrl`. La dirección URL se generó en la sección "Secreto de cifrado de disco no cifrado con una KEK".
```powershell
    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl
```
### <a name="bkmk_URLKEK"></a> Con KEK
Cuando conecte un disco del sistema operativo, pase `$KeyEncryptionKey` y `$secretUrl`. La dirección URL se generó en la sección "Secreto del cifrado de disco cifrado con KEK".
```powershell
    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id
```
