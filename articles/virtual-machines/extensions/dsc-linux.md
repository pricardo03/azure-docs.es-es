---
title: Extensión de Azure DSC para Linux
description: Instala los paquetes OMI y DSC para permitir que una máquina virtual Linux de Azure se configure con Desired State Configuration.
services: virtual-machines-linux
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: robreed
ms.openlocfilehash: 01c3e9c82959629dc3e43fb6b0833d4dbb05e0aa
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56823015"
---
# <a name="dsc-extension-for-linux-microsoftostcextensionsdscforlinux"></a>Extensión DSC para Linux (Microsoft.OSTCExtensions.DSCForLinux)

Desired State Configuration (DSC) es una plataforma de administración que permite administrar la infraestructura de desarrollo y de TI con configuración como código.

Microsoft publica y admite la extensión DSCForLinux. La extensión instala los agentes OMI y DSC en las máquinas virtuales de Azure. La extensión DSC también puede realizar las siguientes acciones:


- Registrar la VM de Linux en la cuenta de Azure Automation para extraer las configuraciones del servicio de Azure Automation (Register ExtensionAction)
- Insertar las configuraciones de MOF en la VM de Linux (Push ExtensionAction)
- Aplicar la configuración de Meta MOF en la VM de Linux para configurar el servidor de extracción y extraer la configuración de nodo (Pull ExtensionAction)
- Instalación de módulos personalizados de DSC en la VM de Linux (Install ExtensionAction)
- Eliminación de módulos personalizados de DSC en la VM de Linux (Remove ExtensionAction)

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Requisitos previos

### <a name="operating-system"></a>Sistema operativo

La extensión DSC de Linux es compatible con todas las [distribuciones de Linux aprobadas en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros), excepto:

| Distribución | Versión |
|---|---|
| Debian | Todas las versiones |
| Ubuntu| 18,04 |
 
### <a name="internet-connectivity"></a>Conectividad de Internet

La extensión DSCForlinux requiere que la máquina virtual de destino esté conectada a Internet. Por ejemplo, la extensión Register requiere conectividad con el servicio Automation. En otras acciones, como Pull e Install, se requiere conectividad con Azure Storage o GitHub. Depende de la configuración proporcionada por el cliente.

## <a name="extension-schema"></a>Esquema de extensión

### <a name="11-public-configuration"></a>1.1 Configuración pública

Estos son todos los parámetros de configuración pública admitidos:

* `FileUri`: (opcional, cadena) URI del archivo MOF, del archivo Meta MOF o del archivo ZIP de recursos personalizados.
* `ResourceName`: (opcional, cadena) nombre del módulo de recursos personalizados.
* `ExtensionAction`: (opcional, cadena) especifica lo que hace una extensión. Valores válidos: Register, Push, Pull, Install y Remove. Si no se especifica, se considera como acción de inserción de forma predeterminada.
* `NodeConfigurationName`: (opcional, cadena) nombre de una configuración de nodo que se aplicará.
* `RefreshFrequencyMins`: (opcional, entero) especifica la frecuencia (en minutos) en que DSC intenta obtener la configuración del servidor de extracción. 
       Si la configuración del servidor de extracción es diferente de la configuración actual en el nodo de destino, se copia en el almacén pendiente y se aplica.
* `ConfigurationMode`: (opcional, cadena) especifica cómo DSC debe aplicar la configuración. Los valores válidos son: ApplyOnly, ApplyAndMonitor y ApplyAndAutoCorrect.
* `ConfigurationModeFrequencyMins`: (opcional, entero) especifica la frecuencia (en minutos) en que DSC garantiza que la configuración esté en el estado deseado.

> [!NOTE]
> Si usa una versión anterior a la 2.3, el parámetro de modo es igual que ExtensionAction. El modo parece ser un término sobrecargado. Por lo tanto, para evitar confusiones, ExtensionAction se utiliza a partir de la versión 2.3. Para ofrecer compatibilidad con versiones anteriores, la extensión admite tanto el modo como ExtensionAction. 
>

### <a name="12-protected-configuration"></a>1.2. Configuración protegida

Estos son todos los parámetros de configuración protegidos que se admiten:

* `StorageAccountName`: (opcional, cadena) nombre de la cuenta de almacenamiento que contiene el archivo.
* `StorageAccountKey`: (opcional, cadena) clave de la cuenta de almacenamiento que contiene el archivo.
* `RegistrationUrl`: (opcional, cadena) dirección URL de la cuenta de Azure Automation.
* `RegistrationKey`: (opcional, cadena) clave de acceso de la cuenta de Azure Automation.


## <a name="scenarios"></a>Escenarios

### <a name="register-to-azure-automation-account"></a>Registro de la cuenta de Azure Automation
protected.json
```json
{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}
```
public.json
```json
{
  "ExtensionAction" : "Register",
  "NodeConfigurationName" : "<node-configuration-name>",
  "RefreshFrequencyMins" : "<value>",
  "ConfigurationMode" : "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins" : "<value>"
}
```

Formato de PowerShell
```powershell
$privateConfig = '{
  "RegistrationUrl": "<azure-automation-account-url>",
  "RegistrationKey": "<azure-automation-account-key>"
}'

$publicConfig = '{
  "ExtensionAction" : "Register",
  "NodeConfigurationName": "<node-configuration-name>",
  "RefreshFrequencyMins": "<value>",
  "ConfigurationMode": "<ApplyAndMonitor | ApplyAndAutoCorrect | ApplyOnly>",
  "ConfigurationModeFrequencyMins": "<value>"
}'
```

### <a name="apply-a-mof-configuration-file-in-azure-storage-account-to-the-vm"></a>Aplicación de un archivo de configuración MOF (en la cuenta de Azure Storage) en la VM

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}
```

Formato de PowerShell
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "FileUri": "<mof-file-uri>",
  "ExtensionAction": "Push"
}'
```


### <a name="apply-a-mof-configuration-file-in-public-storage-to-the-vm"></a>Aplicación de un archivo de configuración MOF (en el almacenamiento público) en la VM

public.json
```json
{
  "FileUri": "<mof-file-uri>"
}
```

Formato de PowerShell
```powershell
$publicConfig = '{
  "FileUri": "<mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-azure-storage-account-to-the-vm"></a>Aplicación de un archivo de configuración Meta MOF (en la cuenta de Azure Storage) en la VM

protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```

public.json
```json
{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}
```

Formato de PowerShell
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Pull",
  "FileUri": "<meta-mof-file-uri>"
}'
```

### <a name="apply-a-meta-mof-configuration-file-in-public-storage-to-the-vm"></a>Aplicación de un archivo de configuración Meta MOF (en el almacenamiento público) en la VM
public.json
```json
{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}
```
Formato de PowerShell
```powershell
$publicConfig = '{
  "FileUri": "<meta-mof-file-uri>",
  "ExtensionAction": "Pull"
}'
```

### <a name="install-a-custom-resource-module-zip-file-in-azure-storage-account-to-the-vm"></a>Instalación de un módulo de recursos personalizados (archivo ZIP en la cuenta de Azure Storage) en la VM
protected.json
```json
{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}
```
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```

Formato de PowerShell
```powershell
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'

$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="install-a-custom-resource-module-zip-file-in-public-storage-to-the-vm"></a>Instalación de un módulo de recursos personalizados (archivo ZIP en el almacenamiento público) en la VM
public.json
```json
{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}
```
Formato de PowerShell
```powershell
$publicConfig = '{
  "ExtensionAction": "Install",
  "FileUri": "<resource-zip-file-uri>"
}'
```

### <a name="remove-a-custom-resource-module-from-the-vm"></a>Eliminación de un módulo de recursos personalizados de la VM
public.json
```json
{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}
```
Formato de PowerShell
```powershell
$publicConfig = '{
  "ResourceName": "<resource-name>",
  "ExtensionAction": "Remove"
}'
```

## <a name="template-deployment"></a>Implementación de plantilla

Las extensiones de VM de Azure pueden implementarse con plantillas de Azure Resource Manager. Las plantillas resultan ideales al implementar una o varias máquinas virtuales que requieren configurarse tras la implementación, por ejemplo, para incorporarse a Azure Automation. 

La plantilla de Resource Manager de ejemplo es [201-dsc-linux-azure-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-azure-storage-on-ubuntu) y [201-dsc-linux-public-storage-on-ubuntu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-dsc-linux-public-storage-on-ubuntu).

Para obtener más información sobre las plantillas de Azure Resource Manager, consulte [Authoring Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md) (Creación de plantillas de Azure Resource Manager).


## <a name="azure-cli-deployment"></a>Implementación de la CLI de Azure

### <a name="21-using-azure-cliazure-cli"></a>2.1. Uso de la [**CLI de Azure**] [azure-cli]
Antes de implementar la extensión DSCForLinux, debe configurar `public.json` y `protected.json`, según los diferentes escenarios en la sección 3.

#### <a name="211-classic"></a>2.1.1. Clásico
El modo clásico también se denomina modo de administración de servicio de Azure. Puede cambiarlo mediante la ejecución de:
```
$ azure config mode asm
```

Puede implementar la extensión DSCForLinux mediante la ejecución de:
```
$ azure vm extension set <vm-name> DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```

Para obtener información sobre la última versión disponible de la extensión, ejecute:
```
$ azure vm extension list
```

#### <a name="212-resource-manager"></a>2.1.2. Resource Manager
Cambie al modo Azure Resource Manager mediante la ejecución de:
```
$ azure config mode arm
```

Puede implementar la extensión DSCForLinux mediante la ejecución de:
```
$ azure vm extension set <resource-group> <vm-name> \
DSCForLinux Microsoft.OSTCExtensions <version> \
--private-config-path protected.json --public-config-path public.json
```
> [!NOTE]
> En el modo de Azure Resource Manager, `azure vm extension list` no está disponible por ahora.
>

### <a name="22-using-azure-powershellazure-powershell"></a>2.2. Uso de [**Azure PowerShell**][azure-powershell]

#### <a name="221-classic"></a>2.2.1 Clásico

Puede iniciar sesión en su cuenta de Azure (modo de administración de servicios de Azure) mediante la ejecución de:

```powershell>
Add-AzureAccount
```

Asimismo, puede implementar la extensión DSCForLinux mediante la ejecución de:

```powershell>
$vmname = '<vm-name>'
$vm = Get-AzureVM -ServiceName $vmname -Name $vmname
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

Deberá cambiar el contenido de $privateConfig y $publicConfig según los diferentes escenarios en la sección anterior. 
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzureVMExtension -ExtensionName $extensionName -VM $vm -Publisher $publisher `
  -Version $version -PrivateConfiguration $privateConfig `
  -PublicConfiguration $publicConfig | Update-AzureVM
```

#### <a name="222resource-manager"></a>2.2.2.Resource Manager

Puede iniciar sesión en su cuenta de Azure (modo de Azure Resource Manager) mediante la ejecución de:

```powershell>
Login-AzAccount
```

Haga clic [**AQUÍ**](../../azure-resource-manager/manage-resources-powershell.md) para obtener más información sobre cómo usar Azure PowerShell con Azure Resource Manager.

Puede implementar la extensión DSCForLinux mediante la ejecución de:

```powershell>
$rgName = '<resource-group-name>'
$vmName = '<vm-name>'
$location = '< location>'
$extensionName = 'DSCForLinux'
$publisher = 'Microsoft.OSTCExtensions'
$version = '< version>'
```

Deberá cambiar el contenido de $privateConfig y $publicConfig según los diferentes escenarios en la sección anterior. 
```
$privateConfig = '{
  "StorageAccountName": "<storage-account-name>",
  "StorageAccountKey": "<storage-account-key>"
}'
```

```
$publicConfig = '{
  "ExtensionAction": "Push",
  "FileUri": "<mof-file-uri>"
}'
```

```
Set-AzVMExtension -ResourceGroupName $rgName -VMName $vmName -Location $location `
  -Name $extensionName -Publisher $publisher -ExtensionType $extensionName `
  -TypeHandlerVersion $version -SettingString $publicConfig -ProtectedSettingString $privateConfig
```

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Los datos sobre el estado de las implementaciones de extensiones pueden recuperarse desde Azure Portal y mediante la CLI de Azure. Para ver el estado de implementación de las extensiones de una máquina virtual determinada, ejecute el comando siguiente con la CLI de Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

El resultado de la ejecución de las extensiones se registra en el archivo siguiente:

```
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Código de error: 51 representa una distribución o una acción de extensión no admitidas.
En algunos casos, la extensión DSC de Linux no puede instalar OMI cuando ya existe una versión posterior de OMI en la máquina. [Respuesta de error: (000003) Downgrade not allowed (Cambio a una versión anterior no permitido)]



### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/community/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las extensiones, consulte el artículo [Características y extensiones de las máquinas virtuales para Linux](features-linux.md).
