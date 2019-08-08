---
title: Cómo crear una directiva de configuración de invitados
description: Aprenda a crear una directiva de configuración de invitado de Azure Policy para VM de Windows o Linux.
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 8f895b5ac1f1509869a2075c96e3d998e8b57864
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698793"
---
# <a name="how-to-create-guest-configuration-policies"></a>Cómo crear una directiva de configuración de invitados

La configuración de invitados usa un módulo de recursos denominado [Desired State Configuration](/powershell/dsc) (DSC) para crear la configuración para la auditoría de las máquinas virtuales de Azure. La configuración DSC define la condición en la que debe estar la máquina virtual. Si se produce un error en la evaluación de la configuración, se desencadena la **auditoría** del efecto Directiva y se considera que la máquina virtual **no es compatible**.

Use las siguientes acciones para crear su propia configuración para validar el estado de una máquina virtual de Azure.

> [!IMPORTANT]
> Las directivas personalizadas con la configuración de invitados son una característica en vista previa (GB).

## <a name="add-the-guestconfiguration-resource-module"></a>Agregar el módulo de recursos GuestConfiguration

Para crear una directiva de configuración de invitados, debe agregar el módulo de recursos. Este módulo de recursos se puede usar con la instancia de PowerShell instalada localmente, con [Azure Cloud Shell](https://shell.azure.com) o con la [imagen de Docker de Azure PowerShell](https://hub.docker.com/rsdk-powershell/).

### <a name="base-requirements"></a>Requisitos básicos

El módulo de recursos de configuración de invitados requiere el siguiente software:

- PowerShell. Si todavía no está instalado, siga [estas instrucciones](/powershell/).
- Azure PowerShell 1.5.0 o posterior. Si todavía no está instalado, siga [estas instrucciones](/powershell/azure/install-az-ps).

### <a name="install-the-module"></a>Instalación del módulo

La configuración de invitados usa el módulo de recursos **GuestConfiguration** para crear configuraciones de DSC y publicarlas en Azure Policy:

1. En una secuencia de comandos de PowerShell, ejecute el siguiente comando:

   ```azurepowershell-interactive
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. Asegúrese de que el módulo se haya importado:

   ```azurepowershell-interactive
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="create-custom-guest-configuration-configuration"></a>Crear opciones personalizadas de la configuración de invitados

El primer paso para crear una directiva personalizada para la configuración de invitados es crear la configuración de DSC. Para obtener información general sobre los conceptos y la terminología consulte [Información general de DSC de PowerShell](/powershell/dsc/overview/overview).

### <a name="custom-guest-configuration-configuration-on-linux"></a>Opciones personalizadas de la configuración de invitados en Linux

La configuración de DSC para la configuración de invitados en Linux usa el recurso `ChefInSpecResource` para proporcionar al motor el nombre de la definición [Chef InSpec](https://www.chef.io/inspec/). **Name** es la única propiedad de recurso requerida.

En el siguiente ejemplo crea una configuración denominada **Línea de base**, se importa el módulo de recursos **GuestConfiguration** y se usa el conjunto de recursos `ChefInSpecResource` como nombre de la definición InSpec en **linux-patch-baseline**:

```azurepowershell-interactive
# Define the DSC configuration and import GuestConfiguration
Configuration baseline
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    ChefInSpecResource 'Audit Linux patch baseline'
    {
        Name = 'linux-patch-baseline'
    }
}

# Compile the configuration to create the MOF files
baseline
```

Para obtener más información, consulte [Escribir, compilar y aplicar una configuración](/powershell/dsc/configurations/write-compile-apply-configuration).

### <a name="custom-guest-configuration-configuration-on-windows"></a>Opciones personalizadas de la configuración de invitados en Windows

La configuración de DSC para la configuración de invitados de Azure Policy solo la usa el agente de la configuración de invitados y no entra en conflicto con la configuración de Desired State Configuration de Windows PowerShell.

En el ejemplo siguiente se crea una configuración denominada **AuditBitLocker**, se importa el módulo de recursos **GuestConfiguration** y se usa el recurso `Service` para auditar un servicio en ejecución:

```azurepowershell-interactive
# Define the DSC configuration and import GuestConfiguration
Configuration AuditBitLocker
{
    Import-DscResource -ModuleName 'PSDscResources'

    Service 'Ensure BitLocker service is present and running'
    {
        Name = 'BDESVC'
        Ensure = 'Present'
        State = 'Running'
    }
}

# Compile the configuration to create the MOF files
AuditBitLocker
```

Para obtener más información, consulte [Escribir, compilar y aplicar una configuración](/powershell/dsc/configurations/write-compile-apply-configuration).

## <a name="create-guest-configuration-custom-policy-package"></a>Crear un paquete de directivas personalizado de configuración de invitados

Una vez compilado el MOF, los archivos auxiliares deben empaquetarse juntos. La configuración de invitados usa el paquete completado para crear las definiciones de Azure Policy. El paquete consta de:

- La configuración de DSC compilada como MOF
- Carpeta Modules
  - Módulo GuestConfiguration
  - Módulo DscNativeResources
  - (Linux) Una carpeta con la definición Chef InSpec y contenido adicional
  - (Windows) Módulos de recursos de DSC que no están integrados

El cmdlet `New-GuestConfigurationPackage` crea el paquete. El formato siguiente se usa para crear un paquete personalizado:

```azurepowershell-interactive
New-GuestConfigurationPackage -Name '{PackageName}' -Configuration '{PathToMOF}' `
    -Path '{OutputFolder}' -Verbose
```

Parámetros del cmdlet `New-GuestConfigurationPackage`:

- **Nombre**: nombre del paquete de configuración de invitados.
- **Configuración**: ruta de acceso completa del documento de configuración de DSC compilado.
- **Ruta de acceso**: ruta de acceso de la carpeta de salida. Este parámetro es opcional. Si no se especifica, el paquete se crea en el directorio actual.
- **ChefProfilePath**: ruta de acceso completa al perfil de InSpec. Este parámetro solo se admite cuando se crea contenido para auditar Linux.

El paquete completado debe almacenarse en una ubicación a la que puedan obtener acceso las máquinas virtuales administradas. Algunos ejemplos son los repositorios de GitHub, un repositorio de Azure o Azure Storage. Si prefiere no hacer que el paquete sea público, puede incluir un [token de SAS](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md) en la dirección URL. También puede implementar el [punto de conexión de servicio](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) para las máquinas virtuales en una red privada, aunque esta configuración solo se aplica para obtener acceso al paquete y no para comunicarse con el servicio.

### <a name="working-with-secrets-in-guest-configuration-packages"></a>Trabajar con secretos en paquetes de configuración de invitados

En la configuración de invitados de Azure Policy, la manera óptima de administrar los secretos que se usan en tiempo de ejecución es almacenarlos en Azure Key Vault. Este diseño se implementa en los recursos de DSC personalizados.

En primer lugar, cree una identidad administrada que haya asignado el usuario en Azure. Las máquinas virtuales usan la identidad para obtener acceso a los secretos almacenados en Key Vault. Para obtener los pasos detallados, consulte [Crear, enumerar y eliminar una identidad administrada que haya asignado el usuario mediante Azure PowerShell](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md).

A continuación, cree una instancia de Key Vault. Para obtener los pasos detallados, consulte [Establecimiento y recuperación de un secreto: PowerShell](../../../key-vault/quick-create-powershell.md).
Asigne permisos a la instancia para conceder a la identidad que haya asignado el usuario acceso a los secretos almacenados en Key Vault. Para obtener los pasos detallados, consulte [Establecimiento y recuperación de un secreto: .NET](../../../key-vault/quick-create-net.md#assign-permissions-to-your-application-to-read-secrets-from-key-vault).

A continuación, asigne a su máquina virtual la identidad que haya asignado el usuario. Para obtener los pasos detallados, consulte [Configurar identidades administradas de recursos de Azure en una VM de Azure mediante PowerShell](../../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md#user-assigned-managed-identity).
A escala, asigne esta identidad mediante Azure Resource Manager a través de Azure Policy. Para obtener los pasos detallados, consulte [Configurar identidades administradas de recursos de Azure en una VM de Azure mediante una plantilla](../../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md#assign-a-user-assigned-managed-identity-to-an-azure-vm).

Por último, en el recurso personalizado, use el id. de cliente generado anteriormente para obtener acceso a Key Vault mediante el token disponible en la máquina. El valor de `client_id` y la dirección URL a la instancia de Key Vault se pueden llevar al recurso como [propiedades](/powershell/dsc/resources/authoringresourcemof#creating-the-mof-schema) para que no sea necesario actualizar el recurso en varios entornos o si es necesario cambiar los valores.

El siguiente ejemplo de código se puede usar en un recurso personalizado para recuperar los secretos de Key Vault mediante una identidad que haya asignado el usuario. El valor devuelto de la solicitud a Key Vault es texto sin formato. Como procedimiento recomendado, almacénelo en un objeto de tipo Credential.

```azurepowershell-interactive
# the following values should be input as properties
$client_id = 'e3a78c9b-4dd2-46e1-8bfa-88c0574697ce'
$keyvault_url = 'https://keyvaultname.vault.azure.net/secrets/mysecret'

$access_token = ((Invoke-WebRequest -Uri "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=$client_id&resource=https%3A%2F%2Fvault.azure.net" -Method GET -Headers @{Metadata='true'}).Content | ConvertFrom-Json).access_token

$value = ((Invoke-WebRequest -Uri $($keyvault_url+'?api-version=2016-10-01') -Method GET -Headers @{Authorization="Bearer $access_token"}).content | convertfrom-json).value |  ConvertTo-SecureString -asplaintext -force

$credential = New-Object System.Management.Automation.PSCredential('secret',$value)
```

## <a name="test-a-guest-configuration-package"></a>Probar un paquete de configuración de invitados

Después de crear el paquete de configuración, pero antes de publicarlo en Azure, puede probar el funcionamiento del paquete desde la estación de trabajo o el entorno de CI/CD. El módulo GuestConfiguration incluye un cmdlet `Test-GuestConfigurationPackage` que carga el mismo agente en el entorno de desarrollo que se usa en Azure Virtual Machines. Con esta solución, puede realizar pruebas de integración local antes de la publicación en entornos de prueba, QA y pruebas de facturación.

```azurepowershell-interactive
Test-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Verbose
```

Parámetros del cmdlet `Test-GuestConfigurationPackage`:

- **Nombre**: nombre de la directiva de configuración de invitados.
- **Parámetro**: Parámetros de directiva proporcionados en formato de tabla hash.
- **Ruta de acceso**: ruta de acceso completa del paquete de configuración de invitados.

El cmdlet también admite la entrada de la canalización de PowerShell. Canaliza la salida del cmdlet `New-GuestConfigurationPackage` al cmdlet `Test-GuestConfigurationPackage`.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditWindowsService -Configuration .\DSCConfig\localhost.mof -Path .\package -Verbose | Test-GuestConfigurationPackage -Verbose
```

Para obtener más información sobre cómo realizar pruebas con parámetros, consulte la sección [Uso de parámetros en directivas de configuración de invitados personalizadas](/azure/governance/policy/how-to/guest-configuration-create#using-parameters-in-custom-guest-configuration-policies).

## <a name="create-the-azure-policy-definition-and-initiative-deployment-files"></a>Crear los archivos de implementación de la definición y la iniciativa de Azure Policy

Una vez que se ha creado un paquete de directivas personalizadas de configuración de invitados y se ha cargado en una ubicación a la que pueden acceder las máquinas virtuales, cree la definición de la directiva de configuración de invitados para Azure Policy. El cmdlet `New-GuestConfigurationPolicy` toma un paquete de directivas personalizadas de configuración de invitados que es accesible de forma pública y crea una definición de directiva de **auditIfNotExists** y **deployIfNotExists**. También se crea una definición de iniciativa de directivas que incluye ambas definiciones de directiva.

En el ejemplo siguiente se crean las definiciones de directiva e iniciativa en una ruta de acceso especificada desde un paquete de directivas personalizado de configuración de invitados para Windows y se proporciona un nombre, una descripción y una versión:

```azurepowershell-interactive
New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -DestinationPath '.\policyDefinitions' `
    -Platform 'Windows' `
    -Version 1.2.3.4 `
    -Verbose
```

Parámetros del cmdlet `New-GuestConfigurationPolicy`:

- **ContentUri**: Uri de http(s) público del paquete de contenido de configuración de invitados.
- **DisplayName**: Nombre para mostrar de la directiva.
- **Descripción**: Descripción de la directiva.
- **Parámetro**: Parámetros de directiva proporcionados en formato de tabla hash.
- **Versión**: Versión de la directiva.
- **Ruta de acceso**: Ruta de acceso de destino donde se crean las definiciones de directiva.
- **Plataforma**: Plataforma de destino (Windows/Linux) para la directiva de configuración de invitados y el paquete de contenido.

`New-GuestConfigurationPolicy` se encarga de crear los siguientes archivos:

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

La salida del cmdlet devuelve un objeto que contiene el nombre para mostrar de la iniciativa y la ruta de acceso de los archivos de directiva.

Si quiere usar este comando para aplicar técnicas de scaffolding a un proyecto de directiva personalizado, puede realizar cambios en estos archivos. Un ejemplo sería modificar la sección "If" para evaluar si una etiqueta específica está presente en las máquinas virtuales. Para obtener información detallada sobre la creación de directivas, consulte [Creación de directivas mediante programación](./programmatically-create.md).

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Usar parámetros en directivas de configuración de invitados personalizadas

La configuración de invitados admite la invalidación de propiedades de una configuración en tiempo de ejecución. Esta característica significa que los valores del archivo MOF del paquete no tienen que considerarse estáticos. Los valores de invalidación se proporcionan a través de Azure Policy y no afectan al modo en que se crean o se compilan las configuraciones.

Los cmdlets `New-GuestConfigurationPolicy` e `Test-GuestConfigurationPolicyPackage` incluyen un parámetro denominado **Parameters**.
Este parámetro toma una definición de tabla hash que incluye todos los detalles sobre cada parámetro y crea automáticamente todas las secciones necesarias de los archivos que se usan para crear cada definición de Azure Policy.

En el ejemplo siguiente se crea una instancia de Azure Policy para auditar un servicio que el usuario selecciona de una lista de servicios en el momento de la asignación de la directiva.

```azurepowershell-interactive
$PolicyParameterInfo = @(
    @{
        Name = 'ServiceName'                                            # Policy parameter name (mandatory)
        DisplayName = 'windows service name.'                           # Policy parameter display name (mandatory)
        Description = "Name of the windows service to be audited."      # Policy parameter description (optional)
        ResourceType = "Service"                                        # DSC configuration resource type (mandatory)
        ResourceId = 'windowsService'                                   # DSC configuration resource property name (mandatory)
        ResourcePropertyName = "Name"                                   # DSC configuration resource property name (mandatory)
        DefaultValue = 'winrm'                                          # Policy parameter default value (optional)
        AllowedValues = @('BDESVC','TermService','wuauserv','winrm')    # Policy parameter allowed values (optional)
    }
)

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Windows Service.' `
    -Description 'Audit if a Windows Service is not enabled on Windows machine.' `
    -DestinationPath '.\policyDefinitions' `
    -Parameters $PolicyParameterInfo `
    -Platform 'Windows' `
    -Version 1.2.3.4 `
    -Verbose
```

En el caso de las directivas de Linux, incluya la propiedad `AttributesYmlContent` en la configuración y sobrescriba los valores según corresponda. El agente de configuración de invitados crea automáticamente el archivo YaML que usa InSpec para almacenar los atributos. Observe el ejemplo siguiente.

```azurepowershell-interactive
Configuration FirewalldEnabled {

    Import-DscResource -ModuleName 'GuestConfiguration'

    Node FirewalldEnabled {

        ChefInSpecResource FirewalldEnabled {
            Name = 'FirewalldEnabled'
            AttributesYmlContent = "DefaultFirewalldProfile: [public]"
        }
    }
}
```

Para cada parámetro adicional, agregue una tabla hash a la matriz. En los archivos de directivas, verá las propiedades que se agregaron a la configuración de invitados configurationName que identifican el tipo de recurso, el nombre, la propiedad y el valor.

```json
{
    "apiVersion": "2018-11-20",
    "type": "Microsoft.Compute/virtualMachines/providers/guestConfigurationAssignments",
    "name": "[concat(parameters('vmName'), '/Microsoft.GuestConfiguration/', parameters('configurationName'))]",
    "location": "[parameters('location')]",
    "properties": {
        "guestConfiguration": {
            "name": "[parameters('configurationName')]",
            "version": "1.*",
            "configurationParameter": [{
                "name": "[Service]windowsService;Name",
                "value": "[parameters('ServiceName')]"
            }]
        }
    }
}
```

## <a name="publish-to-azure-policy"></a>Publicación en Azure Policy

El módulo de recursos **GuestConfiguration** ofrece una manera de crear definiciones de directivas y la definición de iniciativa en Azure con un solo paso mediante el cmdlet `Publish-GuestConfigurationPolicy`.
El cmdlet solo tiene el parámetro **Path** que apunta a la ubicación de los tres archivos JSON que creó `New-GuestConfigurationPolicy`.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions' -Verbose
```

El cmdlet `Publish-GuestConfigurationPolicy` acepta la ruta de acceso de la canalización de PowerShell. Esta característica significa que puede crear los archivos de directiva y publicarlos en un único conjunto de comandos canalizados.

```azurepowershell-interactive
New-GuestConfigurationPolicy -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' -DisplayName 'Audit BitLocker service.' -Description 'Audit if the BitLocker service is not enabled on Windows machine.' -DestinationPath '.\policyDefinitions' -Platform 'Windows' -Version 1.2.3.4 -Verbose | ForEach-Object {$_.Path} | Publish-GuestConfigurationPolicy -Verbose
```

Con las definiciones de directivas e iniciativas creadas en Azure, el último paso es asignar la iniciativa. Consulte cómo puede asignar la iniciativa con el [portal](../assign-policy-portal.md), la [CLI de Azure](../assign-policy-azurecli.md) y [Azure PowerShell](../assign-policy-powershell.md).

> [!IMPORTANT]
> Las directivas de configuración de invitados **siempre** se deben asignar mediante la iniciativa que combina las directivas _AuditIfNotExists_ y _DeployIfNotExists_. Si solo se asigna la directiva _AuditIfNotExists_, los requisitos previos no se implementan y la directiva siempre muestra que los servidores "0" son compatibles.

## <a name="policy-lifecycle"></a>Ciclo de vida de la directiva

Después de haber publicado una instancia de Azure Policy personalizada mediante el paquete de contenido personalizado, hay dos campos que deben actualizarse si quiere publicar una nueva versión.

- **Versión**: al ejecutar el cmdlet `New-GuestConfigurationPolicy`, debe especificar un número de versión mayor que el que se ha publicado actualmente.  Esto actualizará la versión de la asignación de la configuración de invitados en el nuevo archivo de la directiva para que la extensión reconozca que se ha actualizado el paquete.
- **contentHash**: el cmdlet `New-GuestConfigurationPolicy` lo actualiza automáticamente.  Es un valor hash del paquete que creó `New-GuestConfigurationPackage`.  Debe ser correcto en el archivo `.zip` que quiera publicar.  Si solo se actualiza la propiedad `contentUri`, como en el caso donde alguien pueda efectuar un cambio manual en la definición de la directiva desde el portal, la extensión no aceptará el paquete de contenido.

La manera más fácil de publicar un paquete actualizado es repetir el proceso que se describe en este artículo y proporcionar un número de versión actualizado.
Esto garantizará que todas las propiedades se hayan actualizado correctamente.

## <a name="optional-signing-guest-configuration-packages"></a>OPCIONAL: Firma de paquetes de configuración de invitados

De forma predeterminada, las directivas personalizadas de configuración de invitados usan el hash SHA256 para confirmar que el paquete de directivas no ha cambiado desde que se publicó hasta que lo lea servidor que se está auditando.
Opcionalmente, los clientes también pueden usar un certificado para firmar paquetes y forzar a la extensión de configuración de invitados a permitir solo el contenido firmado.

Para habilitar este escenario, hay dos pasos que debe completar. Ejecute el cmdlet para firmar el paquete de contenido y anexe una etiqueta a las máquinas virtuales que deben solicitar la firma del código.

Para usar la característica de validación de firmas, ejecute el cmdlet `Protect-GuestConfigurationPackage` para firmar el paquete antes de publicarlo. Este cmdlet requiere un certificado de "firma de código".

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

Parámetros del cmdlet `Protect-GuestConfigurationPackage`:

- **Ruta de acceso**: ruta de acceso completa del paquete de configuración de invitados.
- **Certificate**: certificado de firma de código para firmar el paquete. Este parámetro solo se admite cuando se firma contenido para Windows.
- **PrivateGpgKeyPath**: ruta de acceso de la clave GPG privada. Este parámetro solo se admite cuando se firma contenido para Linux.
- **PublicGpgKeyPath**: ruta de acceso de la clave GPG pública. Este parámetro solo se admite cuando se firma contenido para Linux.

El agente GuestConfiguration espera que la clave pública del certificado esté presente en la opción "Entidades de certificación raíz de confianza" de las máquinas Windows y en la ruta de acceso `/usr/local/share/ca-certificates/extra` de las máquinas Linux. Para que el nodo compruebe el contenido firmado, instale la clave pública del certificado en la máquina virtual antes de aplicar la directiva personalizada. Este proceso se puede realizar con cualquier técnica dentro de la máquina virtual o mediante Azure Policy. [Aquí se proporciona](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows) una plantilla de ejemplo.
La directiva de acceso de Key Vault debe permitir que el proveedor de recursos del proceso obtenga acceso a los certificados durante las implementaciones. Para obtener los pasos detallados, consulte [Configuración de Key Vault para máquinas virtuales en Azure Resource Manager](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

El siguiente es un ejemplo para exportar la clave pública de un certificado de firma, para importarla a la máquina virtual.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

Una buena referencia para la creación de claves GPG para usarlas con máquinas virtuales Linux se proporciona en el artículo de Github [Generar una clave de GPG nueva](https://help.github.com/en/articles/generating-a-new-gpg-key).

Una vez publicado el contenido, anexe una etiqueta con el nombre `GuestConfigPolicyCertificateValidation` y el valor `enabled` a todas las máquinas virtuales en las que se debe solicitar la firma de código. Esta etiqueta se puede entregar a escala mediante Azure Policy. Consulte el ejemplo [Aplicar una etiqueta y su valor predeterminado](../samples/apply-tag-default-value.md).
Una vez que esta etiqueta esté en su lugar, la definición de la directiva que se genera mediante el cmdlet `New-GuestConfigurationPolicy` habilita el requisito a través de la extensión de configuración de invitados.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la auditoría de VM con la [configuración de invitados](../concepts/guest-configuration.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](getting-compliance-data.md).
