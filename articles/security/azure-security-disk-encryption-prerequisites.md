---
title: 'Requisitos previos: Azure Disk Encryption para VM de IaaS | Microsoft Docs'
description: En este artículo se proporcionan los requisitos previos para usar Microsoft Azure Disk Encryption para máquinas virtuales IaaS.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 12/13/2018
ms.custom: seodec18
ms.openlocfilehash: 116f1f0a93c09ed751f0720ae74a2c24df7541eb
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342517"
---
# <a name="azure-disk-encryption-prerequisites"></a>Requisitos previos de Azure Disk Encryption

 Este artículo, Requisitos previos de Azure Disk Encryption, explica los elementos que deben estar implementados antes de usar Azure Disk Encryption. Azure Disk Encryption se integra con [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) para ayudar a administrar las claves de cifrado. Puede usar [Azure PowerShell](/powershell/azure/overview), la [CLI de Azure](/cli/azure/) o [Azure portal](https://portal.azure.com) para configurar Azure Disk Encryption.

Antes de habilitar Azure Disk Encryption en máquinas virtuales IaaS de Azure para los escenarios admitidos que se trataron en el artículo de [información general sobre Azure Disk Encryption](azure-security-disk-encryption-overview.md), asegúrese de cumplir los requisitos previos. 

> [!NOTE]
> Algunas de las recomendaciones pueden provocar un aumento del uso de datos, de la red o de recursos de proceso, lo que incrementará los costes de las licencias o suscripciones. Para crear recursos en Azure en las regiones admitidas, debe tener una suscripción válida de Azure activa.


## <a name="bkmk_OSs"></a> Sistemas operativos compatibles
Azure Disk Encryption es compatible con los siguientes sistemas operativos:

- Versiones de Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.
    - En el caso de Windows Server 2008 R2, debe tener .NET Framework 4.5 instalado para poder habilitar el cifrado en Azure. Instálelo desde Windows Update con la actualización opcional Microsoft .NET Framework 4.5.2 para sistemas basados en x64 con Windows Server 2008 R2 ([KB2901983](https://support.microsoft.com/kb/2901983)).    
- Versiones de cliente Windows: Cliente Windows 8 y Windows 10.
- Azure Disk Encryption solo se admite en distribuciones y versiones específicas del servidor Linux basadas en la Galería de Azure. Para la lista de versiones admitidas actualmente, consulte [Preguntas más frecuentes de Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
- Azure Disk Encryption requiere que el almacén de claves y las máquinas virtuales residan en la misma región y suscripción de Azure. Si se configuran los recursos en regiones distintas, se producirá un error al habilitar la característica Azure Disk Encryption.

## <a name="bkmk_LinuxPrereq"></a> Requisitos previos adicionales para máquinas virtuales Iaas Linux 

- Azure Disk Encryption para Linux requiere 7 GB de RAM en la máquina virtual para habilitar el cifrado de disco del sistema operativo en las [imágenes compatibles](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Una vez completado el proceso de cifrado de disco del sistema operativo, la máquina virtual se puede configurar para que se ejecute con menos memoria.
- Antes de habilitar el cifrado, los discos de datos que se van a cifrar deben aparecer correctamente en /etc/fstab. Use un nombre de dispositivo de bloqueo persistente para esta entrada, ya que los nombres de dispositivo con el formato "/ dev/sdX" no son confiables para asociarlos al mismo disco en los distintos reinicios, especialmente después de que se aplica el cifrado. Para obtener más información sobre este comportamiento, consulte: [Solución de problemas: se cambian los nombres de dispositivo de máquinas virtuales Linux](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- Asegúrese de que el valor /etc/fstab está configurado correctamente para el montaje. Para configurar estos valores, ejecute el comando mount -a o reinicie la máquina virtual y desencadene el nuevo montaje de ese modo. Cuando haya finalizado, revise la salida del comando lsblk para comprobar que la unidad sigue montada. 
    - Si el archivo/etc/fstab no monta la unidad correctamente antes de habilitar el cifrado, Azure Disk Encryption no podrá montarla correctamente.
    - El proceso de Azure Disk Encryption sacará la información de montaje de/etc/fstab y la colocará en su propio archivo de configuración como parte del proceso de cifrado. No se alarme si ve que falta la entrada de/etc/fstab después de que se completa el cifrado de la unidad de datos.
    -  Después del reinicio, el proceso de Azure Disk Encryption tardará un tiempo en montar los discos recién cifrados. No estarán disponibles inmediatamente después de un reinicio. El proceso necesita tiempo para iniciar, desbloquear y montar las unidades cifradas antes de que estén disponibles para que otros procesos tengan acceso a ellas. Este proceso puede tardar más de un minuto después del reinicio, dependiendo de las características del sistema.

Puede encontrar un ejemplo de los comandos que se pueden usar para montar los discos de datos y crear las entradas /etc/fstab necesarias en las [líneas 244-248 de este archivo de script](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248). 


## <a name="bkmk_GPO"></a> Redes y directiva de grupo

**Para habilitar la característica Azure Disk Encryption, las máquinas virtuales IaaS deben cumplir los siguientes requisitos de configuración de puntos de conexión de red:**
  - Para que un token se conecte al almacén de claves, la máquina virtual IaaS debe poder conectarse a un punto de conexión de Azure Active Directory, \[login.microsoftonline.com\].
  - Para escribir las claves de cifrado en el almacén de claves, la máquina virtual IaaS debe poder conectarse al punto de conexión del almacén de claves.
  - La máquina virtual IaaS debe poder conectarse al punto de conexión de Azure Storage que hospeda el repositorio de extensiones de Azure y la cuenta de Azure Storage que hospeda los archivos VHD.
  -  Si su directiva de seguridad limita el acceso desde máquinas virtuales de Azure a Internet, puede resolver el URI anterior y configurar una regla concreta para permitir la conectividad de salida para las direcciones IP. Para más información, consulte [Azure Key Vault detrás de un firewall](../key-vault/key-vault-access-behind-firewall.md).    


**Directiva de grupo:**
 - La solución Azure Disk Encryption usa el protector de claves externas de BitLocker para máquinas virtuales IaaS con Windows. Para las máquinas virtuales unidas en un dominio, no cree ninguna directiva de grupo que exija protectores de TPM. Para obtener información acerca de la directiva de grupo para "Permitir BitLocker sin un TPM compatible", consulte la [Referencia de la directiva de grupo de BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  La directiva de BitLocker en máquinas virtuales de unión a un dominio con directivas de grupo personalizadas debe incluir la siguiente configuración: [Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings) (Configuración de almacenamiento de usuario de información de recuperación de BitLocker -> Permitir clave de recuperación de 256 bits). Azure Disk Encryption presentará un error cuando la configuración de la directiva de grupo personalizada para Bitlocker es incompatible. En máquinas que no tengan la configuración de directiva correcta, puede que sea necesario aplicar la nueva directiva, forzar la nueva directiva a actualizarse (gpupdate.exe /force) y luego reiniciar.  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) ofrece un conjunto de cmdlets que usan el modelo [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para administrar los recursos de Azure. Se puede usar en el explorador con [Azure Cloud Shell](../cloud-shell/overview.md), o lo puede instalar en el equipo local con las instrucciones que aparecen a continuación para usarlo en cualquier sesión de PowerShell. Si ya lo tiene instalado localmente, asegúrese de que usa la versión más reciente de la versión del SDK de Azure PowerShell para configurar Azure Disk Encryption. Descargue la versión más reciente de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Instale Azure PowerShell para su uso en el equipo local (opcional): 
1. Siga las instrucciones de los vínculos correspondientes a su sistema operativo y continúe por el resto de los pasos siguientes.      
    - [Instale y configure Azure PowerShell para Windows](/powershell/azure/install-azurerm-ps). 
        - Instale PowerShellGet, Azure PowerShell y cargue el módulo AzureRM. 
    - [Instale y configure Azure Powershell en macOS y Linux](/powershell/azure/install-azurermps-maclinux).
        -  Instale PowerShell Core y Azure PowerShell para .NET Core y cargue el módulo Az.

2. Compruebe las versiones instaladas del módulo AzureRM. Si es necesario, [actualice el módulo Azure PowerShell](/powershell/azure/install-azurerm-ps#update-the-azure-powershell-module).
    -  La versión del módulo AzureRM debe ser 6.0.0 o posterior.
    - Se recomienda utilizar la versión más reciente del módulo AzureRM.

     ```powershell
     Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
     ```

3. Inicie sesión en Azure con el cmdlet [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount).
     
     ```azurepowershell-interactive
     Connect-AzureRmAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzureRmSubscription and 
     specify it with Set-AzureRmContext.  #>
     Get-AzureRmSubscription
     Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

4.  Si es necesario, consulte [Introducción a Azure PowerShell](/powershell/azure/get-started-azureps).

## <a name="bkmk_CLI"></a> Instalación de la CLI de Azure para su uso en la máquina local (opcional)

La [CLI de Azure 2.0](/cli/azure) es una herramienta de línea de comandos para administrar recursos de Azure. La CLI está diseñada para flexibilizar los datos de consulta, admitir operaciones de larga duración (como los procesos sin bloqueo) y facilitar la realización de scripts. Se puede usar en el explorador con [Azure Cloud Shell](../cloud-shell/overview.md), o lo puede instalar en el equipo local y usarlo en cualquier sesión de PowerShell.

1. [Instale la CLI de Azure](/cli/azure/install-azure-cli) para su uso en el equipo local (opcional):

2. Compruebe la versión instalada.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Inicie sesión en Azure mediante [az login](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Si es necesario, revise [Introducción a la CLI de Azure 2.0](/cli/azure/get-started-with-azure-cli). 


## <a name="prerequisite-workflow-for-key-vault"></a>Flujo de trabajo de requisitos previos de Key Vault
Si ya está familiarizado con los requisitos previos de Key Vault y Azure AD para Azure Disk Encryption, puede usar el [script de PowerShell de requisitos previos de Azure Disk Encryption](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Para más información sobre el uso del script de requisitos previos, vea la [guía de inicio rápido de cifrado de una máquina virtual](quick-encrypt-vm-powershell.md) y el [apéndice de Azure Disk Encryption](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Si es necesario, cree un grupo de recursos.
2. Cree un almacén de claves. 
3. Configure las directivas de acceso avanzado del almacén de claves.

>[!WARNING]
>Antes de eliminar un almacén de claves, asegúrese de que no cifró ninguna de las VM existentes con él. Para proteger un almacén de la eliminación accidental, [habilite la eliminación temporal](../key-vault/key-vault-soft-delete-powershell.md#enabling-soft-delete) y un [bloqueo de recurso](../azure-resource-manager/resource-group-lock-resources.md) en el almacén. 
 
## <a name="bkmk_KeyVault"></a> Crear un almacén de claves 
Azure Disk Encryption se integra con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para controlar y administrar los secretos y las claves de cifrado de los discos en la suscripción de Key Vault. Puede crear un almacén de claves o usar uno existente para Azure Disk Encryption. Para más información sobre los almacenes de claves, consulte [Introducción a Azure Key Vault](../key-vault/key-vault-get-started.md) y [Protección de un almacén de claves](../key-vault/key-vault-secure-your-key-vault.md). Puede usar una plantilla de Resource Manager, Azure PowerShell o la CLI de Azure para crear un almacén de claves. 


>[!WARNING]
>Con el fin de garantizar que los secretos de cifrado no traspasen los límites regionales, Azure Disk Encryption necesita Key Vault y las máquinas virtuales para ubicarse conjuntamente en la misma región. Cree y use una instancia de Key Vault que se encuentre en la misma región que la máquina virtual que se va a cifrar. 


### <a name="bkmk_KVPSH"></a> Crear un almacén de claves con PowerShell

Puede crear un almacén de claves con Azure PowerShell mediante el cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault). Para otros cmdlets de Key Vault, consulte [AzureRM.KeyVault](/powershell/module/azurerm.keyvault/). 

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Cree un nuevo grupo de recursos, si es necesario, con [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup).  Para enumerar las ubicaciones de los centro de datos, use [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation). 
     
     ```azurepowershell-interactive
     # Get-AzureRmLocation 
     New-AzureRmResourceGroup –Name 'MySecureRG' –Location 'East US'
     ```

3. Cree un nuevo almacén de claves con [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault).
    
      ```azurepowershell-interactive
     New-AzureRmKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -Location 'East US'
     ```

4. Tenga en cuenta el **nombre del almacén**, el **nombre del grupo de recursos**, el **id. de recurso**, el **URI de almacén** y el **id. de objeto** que se devuelven para su uso posterior al cifrar los discos. 


### <a name="bkmk_KVCLI"></a> Crear un almacén de claves con la CLI de Azure
Puede administrar el almacén de claves con la CLI de Azure mediante el comando [az keyvault](/cli/azure/keyvault#commands). Para crear un almacén de claves, use [crear az keyvault](/cli/azure/keyvault#az-keyvault-create).

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Cree un nuevo grupo de recursos, si es necesario, con [crear grupo az](/cli/azure/group#az-group-create). Para enumerar las ubicaciones, use [az account list-locations](/cli/azure/account#az-account-list). 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MySecureRG" -l "East US"
     ```

3. Cree un nuevo almacén de claves con [crear az keyvault](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MySecureRG" --location "East US"
     ```

4. Tenga en cuenta el **nombre del almacén** (nombre), el **nombre del grupo de recursos**, el **id. de recurso** (identificador), el **URI de almacén** y el **id. de objeto** que se devuelven para su uso posterior. 

### <a name="bkmk_KVRM"></a> Crear un almacén de claves con una plantilla de Resource Manager

Puede crear un almacén de claves con la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).
2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, el nombre del almacén de claves, el id. de objeto, los términos legales y el contrato, y luego haga clic en **Comprar**. 


## <a name="bkmk_KVper"></a> Configurar las directivas de acceso avanzado del almacén de claves
La plataforma Azure necesita acceso a las claves de cifrado o secretos del almacén de claves para ponerlos a disposición de la máquina virtual para el proceso de arranque y descifrado de los volúmenes. Habilite el cifrado de disco en el almacén de claves o se producirá un error en las implementaciones.  

### <a name="bkmk_KVperPSH"></a> Establecer las directivas de acceso avanzado del almacén de claves con Azure PowerShell
 Use el cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) de PowerShell del almacén de claves para habilitar el cifrado de disco para el almacén de claves.

  - **Habilitar Key Vault para el cifrado de disco:** se requiere EnabledForDiskEncryption para el cifrado de Azure Disk.
      
     ```azurepowershell-interactive 
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDiskEncryption
     ```

  - **Habilitar Key Vault para la implementación, si es necesario:** permite que el proveedor de recursos Microsoft.Compute recupere los secretos de este almacén de claves cuando se hace referencia al almacén de claves en la creación de recursos, por ejemplo, cuando se crea una máquina virtual.

     ```azurepowershell-interactive
      Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDeployment
     ```

  - **Habilitar Key Vault para la implementación de plantillas, si es necesario:** permite que Azure Resource Manager obtenga los secretos de este almacén de claves cuando se hace referencia al almacén de claves en una implementación de plantilla.

     ```azurepowershell-interactive             
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a> Establecer directivas de acceso avanzado del almacén de claves mediante la CLI de Azure
Use [az keyvault update](/cli/azure/keyvault#az-keyvault-update) para habilitar el cifrado de disco para el almacén de claves. 

 - **Habilitar Key Vault para el cifrado de disco:** Es necesario Enabled-for-disk-encryption. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-disk-encryption "true"
     ```  

 - **Habilitar Key Vault para la implementación, si es necesario:** permite que el proveedor de recursos Microsoft.Compute recupere los secretos de este almacén de claves cuando se hace referencia al almacén de claves en la creación de recursos, por ejemplo, cuando se crea una máquina virtual.

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-deployment "true"
     ``` 

 - **Habilitar Key Vault para la implementación de plantillas, si es necesario:** permite que Resource Manager recupere secretos del almacén.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Establecer directivas de acceso avanzado del almacén de claves a través de Azure Portal

1. Seleccione el almacén de claves, vaya a **Directivas de acceso** y **Haga clic para mostrar las directivas de acceso avanzado**.
2. Active la casilla etiquetada **Habilitar el acceso a Azure Disk Encryption para el cifrado de volúmenes**.
3. Seleccione **Habilitar el acceso a Azure Virtual Machines para la implementación** o **Habilitar el acceso a Azure Resource Manager para la implementación de plantillas**, si es necesario. 
4. Haga clic en **Save**(Guardar).

![Directivas de acceso avanzado de Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Configurar una clave de cifrado de claves (opcional)
Si desea usar una clave de cifrado de claves (KEK) para una brindar una capa adicional de seguridad para las claves de cifrado, agregue una KEK a su almacén de claves. Use el cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) para crear una clave de cifrado de claves en el almacén de claves. También puede importar una KEK en el HSM de administración de claves local. Para más información, consulte la [documentación de Key Vault](../key-vault/key-vault-hsm-protected-keys.md). Cuando se especifica una clave de cifrado de claves, Azure Disk Encryption usa esa clave para encapsular los secretos de cifrado antes de escribirlos en Key Vault. 

* El secreto del almacén de claves y las direcciones URL de KEK deben tener versiones. Azure exige esta restricción del control de versiones. Para ver direcciones URL de KEK y de secretos válidas, vea los ejemplos siguientes:

  * Ejemplo de una dirección URL secreta válida: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Ejemplo de una dirección URL de KEK válida: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption no admite la especificación de números de puerto como parte de los secretos del almacén de claves y las direcciones URL de KEK. Para ver ejemplos de direcciones URL de almacén de claves admitidas y no admitidas, consulte los siguientes:

  * Dirección URL de almacén de claves inaceptable *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Dirección URL de almacén de claves aceptable: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a> Configurar una clave de cifrado de claves con Azure PowerShell 
Antes de usar el script de PowerShell, debe estar familiarizado con los requisitos previos de Azure Disk Encryption para entender los pasos en el script. Es posible que el script de ejemplo requiera cambios para adaptarse al entorno en cuestión. Este script crea todos los requisitos previos de Azure Disk Encryption y cifra una máquina virtual IaaS existente, encapsulando la clave de cifrado de disco mediante el uso de una clave de cifrado de claves. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MySecureRG', and 'MySecureVault' with your values.
     # Use Get-AzureRmLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzureRmResourceGroup
     
     $Loc = 'MyLocation';
     $rgname = 'MySecureRG';
     $KeyVaultName = 'MySecureVault'; 
     New-AzureRmResourceGroup –Name $rgname –Location $Loc;
     New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc;
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $KeyVaultResourceId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).VaultUri;
     
 #Step 2: Enable the vault for disk encryption.
     Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption;
      
 #Step 3: Create a new key in the key vault with the Add-AzureKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 4: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' with your value. 
     
     $VMName = 'MySecureVM';
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```


 
## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Habilitación de Azure Disk Encryption para Windows](azure-security-disk-encryption-windows.md)

> [!div class="nextstepaction"]
> [Habilitación de Azure Disk Encryption para Linux](azure-security-disk-encryption-linux.md)

