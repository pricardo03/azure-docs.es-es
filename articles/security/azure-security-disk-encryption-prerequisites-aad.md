---
title: Requisitos previos de Azure Disk Encryption con una aplicación de Azure AD (versión anterior)
description: En este artículo se proporcionan los requisitos previos para usar Microsoft Azure Disk Encryption para máquinas virtuales IaaS.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 2cc5d953ec412c1c747989d58303beae05f2039c
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286421"
---
# <a name="azure-disk-encryption-prerequisites-previous-release"></a>Requisitos previos Azure Disk Encryption (versión anterior)

**La nueva versión de Azure Disk Encryption elimina la necesidad de proporcionar un parámetro de aplicación de Azure AD para habilitar el cifrado de disco de máquina virtual. Con la nueva versión, ya no es necesario proporcionar credenciales de Azure AD durante el paso de habilitar el cifrado. Todas las nuevas máquinas virtuales deben estar cifradas sin los parámetros de aplicación de Azure AD con la nueva versión. Para instrucciones sobre cómo habilitar el cifrado de disco de máquina virtual con la nueva versión, consulte [Requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md). Las máquinas virtuales que ya se han cifrado con parámetros de aplicación de Azure AD se siguen admitiendo y se deben seguir manteniendo con la sintaxis de AAD.**

 Este artículo, Requisitos previos de Azure Disk Encryption, explica los elementos que deben estar implementados antes de usar Azure Disk Encryption. Junto con los requisitos previos generales, Azure Disk Encryption se integra con [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) y usa una aplicación de Azure AD para proporcionar autenticación con el fin de administrar las claves de cifrado en el almacén de claves. También es posible que desee usar [Azure PowerShell](/powershell/azure/overview) o la [CLI de Azure](/cli/azure/) para instalar o configurar Key Vault y la aplicación de Azure AD.

Antes de habilitar Azure Disk Encryption en máquinas virtuales IaaS de Azure para los escenarios admitidos que se trataron en el artículo de [información general sobre Azure Disk Encryption](azure-security-disk-encryption-overview.md), asegúrese de cumplir los requisitos previos. 

> [!WARNING]
> - Algunas de las recomendaciones pueden provocar un aumento del uso de datos, de la red o de recursos de proceso, lo que incrementará los costes de las licencias o suscripciones. Para crear recursos en Azure en las regiones admitidas, debe tener una suscripción válida de Azure activa.
> - Si ha usado anteriormente [Azure Disk Encryption con la aplicación Azure AD](azure-security-disk-encryption-prerequisites-aad.md) para cifrar esta VM, tendrá que seguir usando esta opción para cifrar la VM. No puede usar [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) en esta VM cifrada ya que no es un escenario compatible, lo que significa que el cambio desde la aplicación de AAD para esta VM cifrada aún no es compatible. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_OSs"></a> Sistemas operativos compatibles
Azure Disk Encryption es compatible con los siguientes sistemas operativos:

- Versiones de Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.
  - En el caso de Windows Server 2008 R2, debe tener .NET Framework 4.5 instalado para poder habilitar el cifrado en Azure. Instálelo desde Windows Update con la actualización opcional Microsoft .NET Framework 4.5.2 para sistemas basados en x64 con Windows Server 2008 R2 ([KB2901983](https://support.microsoft.com/kb/2901983)).    
- Versiones de cliente Windows: Cliente Windows 8 y Windows 10.
- Azure Disk Encryption solo se admite en distribuciones y versiones específicas del servidor Linux basadas en la Galería de Azure. Para la lista de versiones admitidas actualmente, consulte [Preguntas más frecuentes de Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
- Azure Disk Encryption requiere que el almacén de claves y las máquinas virtuales residan en la misma región y suscripción de Azure. Si se configuran los recursos en regiones distintas, se producirá un error al habilitar la característica Azure Disk Encryption.

## <a name="bkmk_LinuxPrereq"></a> Requisitos previos adicionales para VM IaaS Linux 

- Azure Disk Encryption para Linux requiere 7 GB de RAM en la máquina virtual para habilitar el cifrado de disco del sistema operativo en las [imágenes compatibles](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Una vez completado el proceso de cifrado de disco del sistema operativo, la máquina virtual se puede configurar para que se ejecute con menos memoria.
- Antes de habilitar el cifrado, los discos de datos que se van a cifrar deben aparecer correctamente en /etc/fstab. Use un nombre de dispositivo de bloqueo persistente para esta entrada, ya que los nombres de dispositivo con el formato "/ dev/sdX" no son confiables para asociarlos al mismo disco en los distintos reinicios, especialmente después de que se aplica el cifrado. Para obtener más información sobre este comportamiento, consulte: [Solución de problemas: se cambian los nombres de dispositivo de máquinas virtuales Linux](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- Asegúrese de que el valor /etc/fstab está configurado correctamente para el montaje. Para configurar estos valores, ejecute el comando mount -a o reinicie la máquina virtual y desencadene el nuevo montaje de ese modo. Cuando haya finalizado, revise la salida del comando lsblk para comprobar que la unidad deseada sigue montada. 
  - Si el archivo/etc/fstab no monta la unidad correctamente antes de habilitar el cifrado, Azure Disk Encryption no podrá montarla correctamente.
  - El proceso de Azure Disk Encryption sacará la información de montaje de/etc/fstab y la colocará en su propio archivo de configuración como parte del proceso de cifrado. No se alarme si ve que falta la entrada de/etc/fstab después de que se completa el cifrado de la unidad de datos.
  -  Después del reinicio, el proceso de Azure Disk Encryption tardará un tiempo en montar los discos recién cifrados. No estarán disponibles inmediatamente tras un reinicio. El proceso necesita tiempo para iniciar, desbloquear y montar las unidades cifradas antes de que estén disponibles para que otros procesos puedan acceder a ellas. Este proceso puede tardar más de un minuto después del reinicio, dependiendo de las características del sistema.

Puede encontrar un ejemplo de los comandos que se pueden usar para montar los discos de datos y crear las entradas /etc/fstab necesarias en las [líneas 197-205 de este archivo de script](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L197-L205). 


## <a name="bkmk_GPO"></a> Redes y directiva de grupo

**Para habilitar la característica Azure Disk Encryption con la sintaxis de parámetro de ADD anterior, las máquinas virtuales IaaS deben cumplir los siguientes requisitos de configuración de puntos de conexión de red:** 
  - Para que un token se conecte al almacén de claves, la máquina virtual IaaS debe poder conectarse a un punto de conexión de Azure Active Directory, \[login.microsoftonline.com\].
  - Para escribir las claves de cifrado en el almacén de claves, la máquina virtual IaaS debe poder conectarse al punto de conexión del almacén de claves.
  - La máquina virtual IaaS debe poder conectarse al punto de conexión de Azure Storage que hospeda el repositorio de extensiones de Azure y la cuenta de Azure Storage que hospeda los archivos VHD.
  -  Si su directiva de seguridad limita el acceso desde máquinas virtuales de Azure a Internet, puede resolver el URI anterior y configurar una regla concreta para permitir la conectividad de salida para las direcciones IP. Para más información, consulte [Azure Key Vault detrás de un firewall](../key-vault/key-vault-access-behind-firewall.md).
  - En Windows, si se deshabilitó explícitamente TLS 1.0 y la versión de .NET no se ha actualizado a la 4.6 o posterior, el siguiente cambio en el registro habilitará ADE para seleccionar la versión más reciente de TLS:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

 


**Directiva de grupo:**
 - La solución Azure Disk Encryption usa el protector de claves externas de BitLocker para máquinas virtuales IaaS con Windows. Para las máquinas virtuales unidas en un dominio, no cree ninguna directiva de grupo que exija protectores de TPM. Para obtener información acerca de la directiva de grupo para "Permitir BitLocker sin un TPM compatible", consulte la [Referencia de la directiva de grupo de BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  La directiva de BitLocker en máquinas virtuales de unión a un dominio con directivas de grupo personalizadas debe incluir la siguiente configuración: [Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings) (Configuración de almacenamiento de usuario de información de recuperación de BitLocker -> Permitir clave de recuperación de 256 bits). Azure Disk Encryption presentará un error cuando la configuración de la directiva de grupo personalizada para BitLocker sea incompatible. En máquinas que no tengan la configuración de directiva correcta, puede que sea necesario aplicar la nueva directiva, forzar la nueva directiva a actualizarse (gpupdate.exe /force) y luego reiniciar.  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) ofrece un conjunto de cmdlets que usan el modelo [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para administrar los recursos de Azure. Se puede usar en el explorador con [Azure Cloud Shell](../cloud-shell/overview.md), o lo puede instalar en el equipo local con las instrucciones que aparecen a continuación para usarlo en cualquier sesión de PowerShell. Si ya tiene instalado localmente, asegúrese de que usar la versión más reciente de Azure PowerShell para configurar Azure Disk Encryption.

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Instale Azure PowerShell para su uso en el equipo local (opcional):  
1. [Instale y configure Azure PowerShell](/powershell/azure/install-az-ps). 

2. Instale el [módulo PowerShell de Azure Active Directory](/powershell/azure/active-directory/install-adv2#installing-the-azure-ad-module). 

     ```powershell
     Install-Module AzureAD
     ```

3. Compruebe las versiones instaladas de los módulos.
      ```powershell
      Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
      Get-Module AzureAD -ListAvailable | Select-Object -Property Name,Version,Path
      ```
4. Inicie sesión en Azure mediante el [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet.
     
     ```powershell
     Connect-AzAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzAccount –Environment (Get-AzEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzSubscription and 
     specify it with Set-AzContext.  #>
     Get-AzSubscription
     Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

5.  Conéctese a Azure AD [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```

6. Revise [Introducción a Azure PowerShell](/powershell/azure/get-started-azureps) y [AzureAD](/powershell/module/azuread), si es necesario.

## <a name="bkmk_CLI"></a> CLI de Azure

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


## <a name="prerequisite-workflow-for-key-vault-and-the-azure-ad-app"></a>Flujo de trabajo de requisitos previos de Key Vault y la aplicación de Azure AD

Si ya está familiarizado con los requisitos previos de Key Vault y Azure AD para Azure Disk Encryption, puede usar el [script de PowerShell de requisitos previos de Azure Disk Encryption](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Para más información sobre el uso del script de requisitos previos, vea la [guía de inicio rápido de cifrado de una máquina virtual](quick-encrypt-vm-powershell.md) y el [apéndice de Azure Disk Encryption](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Cree un almacén de claves. 
2. Configure una aplicación de Azure AD y una entidad de servicio.
3. Establezca la directiva de acceso del almacén de claves para la aplicación de Azure AD.
4. Configure las directivas de acceso avanzado del almacén de claves.
 
## <a name="bkmk_KeyVault"></a> Crear un almacén de claves 
Azure Disk Encryption se integra con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para controlar y administrar los secretos y las claves de cifrado de los discos en la suscripción de Key Vault. Puede crear un almacén de claves o usar uno existente para Azure Disk Encryption. Para más información sobre los almacenes de claves, consulte [Introducción a Azure Key Vault](../key-vault/key-vault-get-started.md) y [Protección de un almacén de claves](../key-vault/key-vault-secure-your-key-vault.md). Puede usar una plantilla de Resource Manager, Azure PowerShell o la CLI de Azure para crear un almacén de claves. 


>[!WARNING]
>Con el fin de garantizar que los secretos de cifrado no traspasen los límites regionales, Azure Disk Encryption necesita Key Vault y las máquinas virtuales para ubicarse conjuntamente en la misma región. Cree y use una instancia de Key Vault que se encuentre en la misma región que la máquina virtual que se va a cifrar. 


### <a name="bkmk_KVPSH"></a> Crear un almacén de claves con PowerShell

Puede crear un almacén de claves con Azure PowerShell mediante el [New AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault) cmdlet. Para otros cmdlets de Key Vault, consulte [Az.KeyVault](/powershell/module/az.keyvault/). 

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Crear un nuevo grupo de recursos, si es necesario, con [New AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup).  Para enumerar las ubicaciones de centro de datos, use [Get AzLocation](/powershell/module/az.resources/get-azlocation). 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

3. Cree un nuevo almacén de claves con [New AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault)
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Tenga en cuenta el **nombre del almacén**, el **nombre del grupo de recursos**, el **id. de recurso**, el **URI de almacén** y el **id. de objeto** que se devuelven para su uso posterior al cifrar los discos. 


### <a name="bkmk_KVCLI"></a> Crear un almacén de claves con la CLI de Azure
Puede administrar el almacén de claves con la CLI de Azure mediante el comando [az keyvault](/cli/azure/keyvault#commands). Para crear un almacén de claves, use [crear az keyvault](/cli/azure/keyvault#az-keyvault-create).

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Cree un nuevo grupo de recursos, si es necesario, con [crear grupo az](/cli/azure/group#az-group-create). Para enumerar las ubicaciones, use [az account list-locations](/cli/azure/account#az-account-list). 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. Cree un nuevo almacén de claves con [crear az keyvault](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. Tenga en cuenta el **nombre del almacén** (nombre), el **nombre del grupo de recursos**, el **id. de recurso** (identificador), el **URI de almacén** y el **id. de objeto** que se devuelven para su uso posterior. 

### <a name="bkmk_KVRM"></a> Crear un almacén de claves con una plantilla de Resource Manager

Puede crear un almacén de claves con la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).
2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, el nombre del almacén de claves, el id. de objeto, los términos legales y el contrato, y luego haga clic en **Comprar**. 


## <a name="bkmk_ADapp"></a> Configurar una aplicación de Azure AD y una entidad de servicio 
Si es preciso habilitar el cifrado en una máquina virtual en ejecución en Azure, Azure Disk Encryption genera y escribe las claves de cifrado en su almacén de claves. La administración de claves de cifrado en el almacén de claves necesita la autenticación de Azure AD. Para ello, cree una aplicación de Azure AD. Para realizar la autenticación, se pueden usar la autenticación basada en secreto de cliente o la [autenticación de Azure AD basada en certificado del cliente](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).


### <a name="bkmk_ADappPSH"></a> Configurar una aplicación de Azure AD y una entidad de servicio con Azure PowerShell 
Para ejecutar los siguientes comandos, obtenga y use el [módulo de PowerShell de Azure AD](/powershell/azure/active-directory/install-adv2). 

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Use la [New AzADApplication](/powershell/module/az.resources/new-azadapplication) cmdlet de PowerShell para crear una aplicación de Azure AD. MyApplicationHomePage y MyApplicationUri pueden tener los valores que desee.

     ```azurepowershell
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. $azureAdApplication.ApplicationId es el identificador de cliente de Azure AD y $aadClientSecret es el secreto de cliente que usará posteriormente para habilitar Azure Disk Encryption. Proteja el secreto del cliente de Azure AD apropiadamente. Ejecutar `$azureAdApplication.ApplicationId` mostrará el valor de ApplicationID.


### <a name="bkmk_ADappCLI"></a> Configurar una aplicación de Azure AD y una entidad de servicio con la CLI de Azure

Puede administrar las entidades de servicio con la CLI de Azure mediante el comando [az ad sp](/cli/azure/ad/sp). Para obtener más información, consulte [crear una entidad de servicio Azure](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Cree una nueva entidad de servicio.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  El valor de appId devuelto es el identificador de cliente de Azure AD que se usa en otros comandos. También es el SPN que se va a usar para az keyvault set-policy. La contraseña es el secreto de cliente que se debe usar posteriormente para habilitar Azure Disk Encryption. Proteja el secreto del cliente de Azure AD apropiadamente.
 
### <a name="bkmk_ADappRM"></a> Configurar una aplicación de Azure AD y una entidad de servicioa través de Azure Portal
Siga los pasos que aparecen en el artículo [Uso del portal para crear una aplicación de Azure Active Directory y una entidad de servicio con acceso a los recursos](../active-directory/develop/howto-create-service-principal-portal.md) para crear una aplicación de Azure AD. Cada paso que se enumera a continuación lo llevará directamente a la sección del artículo que hay que completar. 

1. [Comprobar los permisos requeridos](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)
2. [Crear una aplicación de Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - Puede utilizar cualquier nombre y dirección URL de inicio de sesión que desee al crear la aplicación.
3. [Obtener el identificador de aplicación y la clave de autenticación](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key) 
     - La clave de autenticación es el secreto de cliente y se utiliza como el AadClientSecret para Set-AzVMDiskEncryptionExtension. 
        - La aplicación usa la clave de autenticación como una credencial para iniciar sesión en Azure AD. En Azure Portal, este secreto se denomina "claves", pero no tiene relación con los almacenes de claves. Proteja adecuadamente este secreto. 
     - El identificador de aplicación se usará más adelante como el AadClientId para Set-AzVMDiskEncryptionExtension y como ServicePrincipalName para Set-AzKeyVaultAccessPolicy. 

## <a name="bkmk_KVAP"></a> Establecer la directiva de acceso del almacén de claves para la aplicación de Azure AD
Para escribir los secretos de cifrado en un almacén de claves especificado, Azure Disk Encryption necesita el identificador de cliente y el secreto de cliente de la aplicación de Azure Active Directory que tenga permisos para escribir secretos en Key Vault. 

> [!NOTE]
> Azure Disk Encryption requiere que configure las siguientes directivas de acceso a la aplicación de cliente de Azure AD: los permisos _WrapKey_ y _Set_.

### <a name="bkmk_KVAPPSH"></a> Establecer la directiva de acceso del almacén de claves para la aplicación de Azure AD con Azure PowerShell
La aplicación de Azure AD necesita derechos de acceso a las claves o secretos del almacén. Use la [conjunto AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet para conceder permisos a la aplicación, con el identificador de cliente (que se generó cuando se registró la aplicación) como el _– ServicePrincipalName_ valor del parámetro. Para obtener más información, consulte la entrada de blog [Azure Key Vault - Step by Step](https://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) (Azure Key Vault - Paso a paso). 

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Establezca la directiva de acceso del almacén de claves para la aplicación de AD con PowerShell.

     ```azurepowershell
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $KVRGname = 'MyKeyVaultResourceGroup'
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
     ```

### <a name="bkmk_KVAPCLI"></a> Establecer la directiva de acceso del almacén de claves para la aplicación de Azure AD con la CLI de Azure
Use [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) para establecer la directiva de acceso. Para más información, consulte [Administración de Key Vault mediante CLI 2.0](../key-vault/key-vault-manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret).

1. Si es necesario, [conéctese a su suscripción de Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Conceda a la entidad de servicio que creó mediante la CLI de Azure accesp para obtener los secretos y encapsular las claves con el comando siguiente:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="bkmk_KVAPRM"></a> Establecer la directiva de acceso del almacén de claves para la aplicación de Azure AD con el portal

1. Abra el grupo de recursos con el almacén de claves.
2. Seleccione el almacén de claves, vaya a **Directivas de acceso** y haga clic en **Agregar nuevo**.
3. En **Select principal** (Seleccionar la entidad de seguridad), busque la aplicación de Azure AD que creó y selecciónela. 
4. Para **Permisos de claves**, active **Encapsular clave** en **Operaciones criptográficas**.
5. Para **Permisos de secretos**, active **Establecer** en **Operaciones de administración de secretos**.
6. Haga clic en **Aceptar** para guardar la directiva de acceso. 

![Operaciones criptográficas de Azure Key Vault: Encapsular clave](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Permisos de secretos de Azure Key Vault: Establecer](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

## <a name="bkmk_KVper"></a> Configurar las directivas de acceso avanzado del almacén de claves
La plataforma Azure necesita acceso a las claves de cifrado o secretos del almacén de claves para ponerlos a disposición de la máquina virtual para el proceso de arranque y descifrado de los volúmenes. Habilite el cifrado de disco en el almacén de claves o se producirá un error en las implementaciones.  

### <a name="bkmk_KVperPSH"></a> Establecer las directivas de acceso avanzado del almacén de claves con Azure PowerShell
 Use el cmdlet de PowerShell del almacén de claves [conjunto AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) para habilitar el cifrado de disco para el almacén de claves.

  - **Habilitar Key Vault para el cifrado de disco:** se requiere EnabledForDiskEncryption para el cifrado de Azure Disk.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Habilitar Key Vault para la implementación, si es necesario:** permite que el proveedor de recursos Microsoft.Compute recupere los secretos de este almacén de claves cuando se hace referencia al almacén de claves en la creación de recursos, por ejemplo, cuando se crea una máquina virtual.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Habilitar Key Vault para la implementación de plantillas, si es necesario:** permite que Azure Resource Manager obtenga los secretos de este almacén de claves cuando se hace referencia al almacén de claves en una implementación de plantilla.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a> Establecer directivas de acceso avanzado del almacén de claves mediante la CLI de Azure
Use [az keyvault update](/cli/azure/keyvault#az-keyvault-update) para habilitar el cifrado de disco para el almacén de claves. 

 - **Habilitar Key Vault para el cifrado de disco:** Es necesario Enabled-for-disk-encryption. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Habilitar Key Vault para la implementación, si es necesario:** permite que Virtual Machines recupere certificados almacenados como secretos del almacén.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Habilitar Key Vault para la implementación de plantillas, si es necesario:** permite que Resource Manager recupere secretos del almacén.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Establecer directivas de acceso avanzado del almacén de claves a través de Azure Portal

1. Seleccione el almacén de claves, vaya a **Directivas de acceso** y **Haga clic para mostrar las directivas de acceso avanzado**.
2. Active la casilla etiquetada **Habilitar el acceso a Azure Disk Encryption para el cifrado de volúmenes**.
3. Seleccione **Habilitar el acceso a Azure Virtual Machines para la implementación** o **Habilitar el acceso a Azure Resource Manager para la implementación de plantillas**, si es necesario. 
4. Haga clic en **Save**(Guardar).

![Directivas de acceso avanzado de Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Configurar una clave de cifrado de claves (opcional)
Si desea usar una clave de cifrado de claves (KEK) para una brindar una capa adicional de seguridad para las claves de cifrado, agregue una KEK a su almacén de claves. Use la [agregar AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet para crear una clave de cifrado de claves en el almacén de claves. También puede importar una KEK en el HSM de administración de claves local. Para más información, consulte la [documentación de Key Vault](../key-vault/key-vault-hsm-protected-keys.md). Cuando se especifica una clave de cifrado de claves, Azure Disk Encryption usa esa clave para encapsular los secretos de cifrado antes de escribirlos en Key Vault. 

* Al generar las claves, use un tipo de clave RSA. Azure Disk Encryption no admite aún mediante claves de curva elíptica.

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
     # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
     # Use Get-AzLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzResourceGroup
     
     $Loc = 'MyLocation';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $KeyVaultName = 'MySecureVault'; 
     New-AzResourceGroup –Name  $KVRGname –Location $Loc;
     New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname;
     $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname).VaultUri;
     
 # Step 2: Create the AD application and service principal.
     # Fill in 'MyAADClientSecret', "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
     # MyApplicationHomePage and the MyApplicationUri can be any values you wish.
     
     $aadClientSecret =  'MyAADClientSecret';
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force;
     $azureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId;
     $aadClientID = $azureAdApplication.ApplicationId;
     
 #Step 3: Enable the vault for disk encryption and set the access policy for the Azure AD application.
     
     Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName  $KVRGname;
     
 #Step 4: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 5: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
     
     $VMName = 'MySecureVM';
      $VMRGName = 'MyVirtualMachineResourceGroup';
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```

## <a name="bkmk_Cert"></a> Autenticación basada en certificados (opcional)
Si desea usar la autenticación de certificado, puede cargar uno en el almacén de claves e implementarlo en el cliente. Antes de usar el script de PowerShell, debe estar familiarizado con los requisitos previos de Azure Disk Encryption para entender los pasos en el script. Es posible que el script de ejemplo requiera cambios para adaptarse al entorno en cuestión.

     
 ```powershell

 # Fill in "MyKeyVaultResourceGroup", "MySecureVault", and 'MyLocation' ('My location' only if needed)

   $KVRGname = 'MyKeyVaultResourceGroup'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption. No need to set 'My location' in this case.

   $Loc = 'MyLocation'
   New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption

   #Setting some variables with the key vault information 
   $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId

   # Create the Azure AD application and associate the certificate with it. 
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
   
   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert".  

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for -EnabledForDeployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDeployment

   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values.

   $VMName = 'MySecureVM'
   $VMRGName = 'MyVirtualMachineResourceGroup'
   $CertUrl = (Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGName).ResourceId
   $VM = Get-AzVM -ResourceGroupName $VMRGName -Name $VMName 
   $VM = Add-AzVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzVM -VM $VM -ResourceGroupName $VMRGName 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
 ```

## <a name="bkmk_CertKEK"></a> Autenticación basada en certificados y una KEK (opcional)

Si desea usar la autenticación de certificado y encapsular la clave de cifrado con una KEK, puede usar el siguiente script como ejemplo. Antes de usar el script de PowerShell, debe estar familiarizado con todos los requisitos previos de Azure Disk Encryption anteriores para conocer los pasos en el script. Es posible que el script de ejemplo requiera cambios para adaptarse al entorno en cuestión.

> [!IMPORTANT]
> Actualmente no se admite la autenticación basada en certificado de Azure AD en máquinas virtuales con Linux.



     
 ```powershell
# Fill in 'MyKeyVaultResourceGroup', 'MySecureVault', and 'MyLocation' (if needed)

   $KVRGname = 'MyKeyVaultResourceGroup'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption.

   $Loc = 'MyLocation'
   New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption

   # Create the Azure AD application and associate the certificate with it.  
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   ## Give access for setting secrets and wraping keys
   Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname

   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert". 

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for deployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDeployment

   #Setting some variables with the key vault information and generating a KEK 
   # FIll in 'KEKName'
   
   $KEKName ='KEKName'
   $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId
   $KEK = Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $KEKName -Destination "Software"
   $KeyEncryptionKeyUrl = $KEK.Key.kid



   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values.

   $VMName = 'MySecureVM';
   $VMRGName = 'MyVirtualMachineResourceGroup';
   $CertUrl = (Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGName).ResourceId
   $VM = Get-AzVM -ResourceGroupName $VMRGName -Name $VMName 
   $VM = Add-AzVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzVM -VM $VM -ResourceGroupName $VMRGName

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

 
## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Habilitación de Azure Disk Encryption para Windows](azure-security-disk-encryption-windows-aad.md)

> [!div class="nextstepaction"]
> [Habilitación de Azure Disk Encryption para Linux](azure-security-disk-encryption-linux-aad.md)
