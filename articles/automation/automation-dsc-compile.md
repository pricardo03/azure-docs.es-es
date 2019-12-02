---
title: Compilación de configuraciones en Azure Automation State Configuration
description: En este artículo se describe cómo compilar configuraciones de configuración de estado deseado (DSC) para Azure Automation.
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 09/10/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6d3cca9d8954d9ac158d88b393c46672da3faa19
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74231714"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>Compilación de configuraciones de DSC en Azure Automation State Configuration

Azure Automation State Configuration permite compilar las configuraciones de Desired State Configuration (DSC) de dos formas: en Azure y en Windows PowerShell. La tabla siguiente le ayudará a determinar cuándo se debe usar cada método en base a las características de cada uno:

- Servicio de compilación de Azure State Configuration
  - Método para principiantes, con una interfaz de usuario interactiva
   - Seguimiento sencillo del estado del trabajo

- Windows PowerShell
  - Llamada desde Windows PowerShell en una estación de trabajo local o un servicio de compilación
  - Integración con la canalización de pruebas de desarrollo
  - Disponibilidad de valores de parámetro complejos
  - Trabajo con datos de nodo y datos que no son de nodo a escala
  - Mejora significativa del rendimiento

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>Compilación de una configuración de DSC en Azure State Configuration

### <a name="portal"></a>Portal

1. En su cuenta de Automation, haga clic en **State Configuration (DSC)** .
1. Haga clic en la pestaña **Configuraciones** y, a continuación, haga clic en el nombre de la configuración que se va a compilar.
1. Haga clic en **Compilar**.
1. Si la configuración no tiene parámetros, se le pedirá que confirme si desea compilarla. Si la configuración tiene parámetros, se abrirá la hoja **Compilar configuración** para que pueda proporcionar valores de parámetro. Consulte más abajo la sección [**Parámetros básicos**](#basic-parameters) para más información acerca de los parámetros.
1. La página **Trabajo de compilación** está abierta para que puede realizar un seguimiento de estado del trabajo de compilación y las configuraciones de nodo (documentos de configuración de MOF) que ocasiona que se colocará en el servidor de extracción de Azure Automation State Configuration.

### <a name="azure-powershell"></a>Azure PowerShell

Puede usar [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) para iniciar la compilación con Windows PowerShell. El código de ejemplo siguiente inicia la compilación de una configuración DSC denominada **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

`Start-AzureRmAutomationDscCompilationJob` devuelve un objeto de trabajo de compilación que puede usar para realizar el seguimiento de su estado. A continuación, puede usar este objeto de trabajo de compilación con [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob)
para determinar el estado del trabajo de compilación, y [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput)
para ver sus flujos de datos (salida). El código de ejemplo siguiente inicia la compilación de la configuración **SampleConfig** , espera hasta que finalice y, a continuación, muestra sus flujos.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($null -eq $CompilationJob.EndTime -and $null -eq $CompilationJob.Exception)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

###  <a name="basic-parameters"></a>Parámetros básicos

La declaración de parámetro en configuraciones DSC, incluidos los tipos de parámetro y las propiedades, funciona igual que en los runbooks de Azure Automation. Vea [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md) para obtener más información acerca de los parámetros del runbook.

En el ejemplo siguiente se usan dos parámetros, **FeatureName** e **IsPresent**, para determinar los valores de las propiedades de la configuración del nodo **ParametersExample.sample**, que se ha generado durante la compilación.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

Puede compilar configuraciones DSC que usan parámetros básicos en el portal de DSC de Azure Automation State Configuration o Azure PowerShell:

#### <a name="portal"></a>Portal

En el portal, puede especificar valores de parámetro después de hacer clic **Compilar**.

![Parámetros compilación de configuración](./media/automation-dsc-compile/DSC_compiling_1.png)

#### <a name="azure-powershell"></a>Azure PowerShell

PowerShell requiere parámetros de un [hashtable](/powershell/module/microsoft.powershell.core/about/about_hash_tables) donde la clave coincide con el nombre del parámetro y el valor es igual al valor de parámetro.

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

Para obtener información acerca de cómo pasar PSCredentials como parámetros, consulte más abajo la sección [Recursos de credenciales](#credential-assets).

### <a name="compiling-configurations-in-azure-automation-that-contain-composite-resources"></a>Compilación de configuraciones en Azure Automation que contienen recursos compuestos

Los **recursos compuestos** le permiten utilizar las configuraciones de DSC como recursos anidados dentro de una configuración. Esto le permite aplicar varias configuraciones a un solo recurso. Consulte [Recursos compuestos: uso de una configuración de DSC como un recurso](/powershell/scripting/dsc/resources/authoringresourcecomposite) para más información acerca de los **recursos compuestos**.

> [!NOTE]
> Para que las configuraciones que contienen **recursos compuestos** se compilen correctamente, antes debe asegurarse de que los recursos de DSC en los que se basa el compuesto se importan en primer lugar en Azure Automation.

La incorporación de un **recurso compuesto** de DSC no es diferente de la incorporación de cualquier módulo de PowerShell a Azure Automation.
Las instrucciones paso a paso para este proceso se documentan en el artículo [Administración de módulos en Azure Automation](/azure/automation/shared-resources/modules).

### <a name="managing-configurationdata-when-compiling-configuration-in-azure-automation"></a>Administración de ConfigurationData al compilar la configuración en Azure Automation

**ConfigurationData** le permite separar las opciones estructurales de cualquier configuración específica del entorno al usar DSC de PowerShell. Consulte [Diferenciación de "Qué" y "Dónde" en DSC de PowerShell](https://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) para obtener más información sobre **ConfigurationData**.

> [!NOTE]
> Puede usar **ConfigurationData** cuando compile en Azure Automation State Configuration con Azure PowerShell, pero no en Azure Portal.

En el siguiente ejemplo la configuración de DSC usa **ConfigurationData** a través de las palabras clave **$ConfigurationData** y **$AllNodes**. En este ejemplo, también necesitará el [módulo **xWebAdministration**](https://www.powershellgallery.com/packages/xWebAdministration/):

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

Puede compilar la configuración de DSC anterior con Windows PowerShell. El siguiente script agrega dos configuraciones de nodo al servicio de extracción de Azure Automation State Configuration: **ConfigurationDataSample.MyVM1** y **ConfigurationDataSample.MyVM3**:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

### <a name="working-with-assets-in-azure-automation-during-compilation"></a>Trabajo con recursos en Azure Automation durante la compilación

Las referencias de recursos son las mismas en Azure Automation State Configuration y en los runbooks. Para obtener más información, vea lo siguiente:

- [Certificates](automation-certificates.md)
- [Conexiones](automation-connections.md)
- [Credenciales](automation-credentials.md)
- [Variables](automation-variables.md)

#### <a name="credential-assets"></a>Recursos de credenciales

Las configuraciones de DSC en Azure Automation pueden hacer referencia a los activos de credenciales de Automation mediante el uso del cmdlet `Get-AutomationPSCredential`. Si una configuración tiene un parámetro con el tipo **PSCredential**, puede usar el cmdlet `Get-AutomationPSCredential` pasando el nombre de cadena de un activo de credencial de Azure Automation al cmdlet para recuperar la credencial. Luego puede usar ese objeto para el parámetro que requiere el objeto **PSCredential**. En segundo plano, se recuperará el recurso de credenciales de Azure Automation con ese nombre y se pasará a la configuración. El ejemplo siguiente muestra esto en acción.

Mantener las credenciales seguras en configuraciones de nodo (documentos de configuración MOF) exige el cifrado de las credenciales en el archivo MOF de configuración de nodo. Sin embargo, actualmente tiene que indicar a DSC de PowerShell que no importa que las credenciales tengan salida como texto sin formato durante la generación del MOF de configuración de nodo, porque PowerShell DSC desconoce que Azure Automation cifrará todo el archivo MOF después de su generación a través de un trabajo de compilación.

Puede indicar a DSC de PowerShell que es correcto que las credenciales tengan salida como texto sin formato en el MOF de configuración de nodo generado mediante ConfigurationData. Debe pasar `PSDscAllowPlainTextPassword = $true` a través de **ConfigurationData** para el nombre de cada bloque de nodo que aparezca en la configuración de DSC y use las credenciales.

En el ejemplo siguiente se muestra una configuración de DSC que usa un recurso de la credencial de Automation.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

Puede compilar la configuración de DSC anterior con PowerShell. El siguiente código de PowerShell agrega las configuraciones de dos nodos al servidor de extracción de Azure Automation State Configuration: **CredentialSample.MyVM1** y **CredentialSample.MyVM2**.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

> [!NOTE]
> Cuando se completa la compilación, puede recibir un error que indica: **The "Microsoft.PowerShell.Management" module was not imported because the "Microsoft.PowerShell.Management" snap-in was already imported.** ("El módulo "Microsoft.PowerShell.Management" no se importó porque el complemento "Microsoft.PowerShell.Management" ya se había importado"). Esta advertencia se puede omitir sin ningún riesgo.

## <a name="compiling-configurations-in-windows-powershell-and-publishing-to-azure-automation"></a>Compilación de configuraciones en Windows PowerShell y publicación en Azure Automation

También puede importar las configuraciones de nodo (MOF) que se hayan compilado fuera de Azure.
Esto incluye la compilación desde una estación de trabajo de desarrollador o en un servicio como [Azure DevOps](https://dev.azure.com).
Este enfoque tiene múltiples ventajas, como el rendimiento y la confiabilidad.
La compilación en Windows PowerShell también ofrece la opción de firmar el contenido de la configuración.
Una configuración de nodo firmada se comprueba localmente en un nodo administrado mediante el agente DSC; de este modo, se garantiza que la configuración que se va a aplicar al nodo proviene de una fuente autorizada.

> [!NOTE]
> Un archivo de configuración de nodo debe ocupar más de 1 MB para poderlo importar en Azure Automation.

Para más información sobre cómo firmar configuraciones de nodo, consulte [Improvements in WMF 5.1 - How to sign configuration and module](/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations). (Mejoras en WMF 5.1: firma de la configuración y del módulo).

### <a name="compiling-a-configuration-in-windows-powershell"></a>Compilación de una configuración en Windows PowerShell

El proceso de compilación de las configuraciones de DSC en Windows PowerShell se incluye en la documentación de DSC de PowerShell, en [Escritura, compilación y aplicación de una configuración](/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration).
Esta compilación se puede ejecutar desde una estación de trabajo de desarrollador o dentro de un servicio de compilación como [Azure DevOps](https://dev.azure.com).

El archivo o los archivos MOF generados al compilar la configuración se pueden después importar directamente en el servicio Azure State Configuration.

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Importación de una configuración de nodo en Azure Portal

1. En la cuenta de Automation, haga clic en **State Configuration (DSC)** en **Administración de configuración**.
1. En la página **State Configuration (DSC)** , haga clic en la pestaña **Configuraciones** y, después, haga clic en **+ Agregar**.
1. En la página **Importar**, haga clic en el icono de carpeta situado junto al cuadro de texto **Archivo de configuración de nodo** para buscar un archivo de configuración de nodo (MOF) en el equipo local.

   ![Buscar archivo local](./media/automation-dsc-compile/import-browse.png)

1. Escriba un nombre en el cuadro de texto **Nombre de la configuración**. Este nombre debe coincidir con el de la configuración desde la que se compiló la configuración del nodo.
1. Haga clic en **OK**.

### <a name="importing-a-node-configuration-with-azure-powershell"></a>Importación de una configuración de nodo con Azure PowerShell

Puede usar el cmdlet [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) para importar una configuración de nodo en su cuenta de Automation.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a usar Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md)
- Para obtener información acerca de la compilación de configuraciones de DSC para que poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones en Azure Automation State Configuration](automation-dsc-compile.md)
- Para la referencia de cmdlets de PowerShell, consulte [cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- Para obtener información de precios, consulte [Precios de Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/)
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua mediante Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md)
