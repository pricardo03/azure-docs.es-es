---
title: Administrar los módulos en Azure Automation
description: En este artículo se describe cómo administrar los módulos de Azure Automation
services: automation
ms.service: automation
ms.subservice: shared-resources
author: georgewallace
ms.author: gwallace
ms.date: 03/13/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fa7f5d3fb38eb1dbca51dec9b73dca3c998436aa
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57905361"
---
# <a name="manage-modules-in-azure-automation"></a>Administrar los módulos en Azure Automation

Azure Automation proporciona la capacidad de importar los módulos de PowerShell en su cuenta de Automation que va a usar los runbooks basada en PowerShell. Estos módulos pueden ser módulos personalizados que ha creado, desde la Galería de PowerShell, o los módulos AzureRM y Az para Azure.

## <a name="import-modules"></a>Importación de módulos

Hay varias formas que puede importar un módulo en la cuenta de Automation. Las secciones siguientes muestran las distintas formas de importar un módulo.

> [!NOTE]
> La ruta de acceso máxima de un archivo en un módulo que se usará en Azure Automation es 140 caracteres. Cualquier ruta de acceso con más de 140 caracteres no podrá importarse en la sesión de PowerShell con `Import-Module`.

### <a name="powershell"></a>PowerShell

Puede usar el [New-AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) para importar un módulo en la cuenta de Automation. El cmdlet toma una dirección url a un paquete zip del módulo.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="azure-portal"></a>Azure Portal

En el portal de Azure, vaya a la cuenta de Automation y seleccione **módulos** en **recursos compartidos**. Haga clic en **+ agregar un módulo**. Seleccione un **.zip** archivo que contiene el módulo y haga clic en **Aceptar** para empezar a importar el proceso.

### <a name="powershell-gallery"></a>Galería de PowerShell

Módulos desde la Galería de PowerShell pueden importarse desde el [Galería de PowerShell](https://www.powershellgallery.com) directamente o desde su cuenta de Automation.

Para importar un módulo desde la Galería de PowerShell, vaya a https://www.powershellgallery.com y busque el módulo que desea importar. Haga clic en **implementar en Azure Automation** en el **Azure Automation** pestaña **opciones de instalación**. Esta acción abre el portal de Azure. En el **importación** , seleccione la cuenta de Automation y haga clic en **Aceptar**.

![Módulo de importación de galería de PowerShell](../media/modules/powershell-gallery.png)

También puede importar módulos desde la Galería de PowerShell directamente desde su cuenta de Automation. En su cuenta de Automation, seleccione **módulos** en **recursos compartidos**. En la página módulos, haga clic en **Examinar Galería**. Se abrirá el **Examinar Galería** página. Puede usar esta página para buscar la Galería de PowerShell para un módulo. Seleccione el módulo que desea importar y haga clic en **importar**. En el **importar** página, haga clic en **Aceptar** para iniciar el proceso de importación.

![Importación de la Galería de PowerShell de Azure portal](../media/modules/gallery-azure-portal.png)

## <a name="internal-cmdlets"></a>Cmdlets internos

La siguiente es una lista de cmdlets en interno `Orchestrator.AssetManagement.Cmdlets` módulo que se importa en cada cuenta de Automation. Estos cmdlets son accesibles en sus runbooks y configuraciones de DSC y le permiten interactuar con los recursos dentro de su cuenta de Automation. Además, los cmdlets internos le permiten recuperar los secretos de cifrado **Variable** valores, **credenciales**y cifrados **conexión** campos. Los cmdlets de PowerShell de Azure no pueden recuperar estos secretos. Estos cmdlets no requieren implícitamente conectarse a Azure cuando se usen. Esto es útil para escenarios donde haya una conexión, como una cuenta de ejecución que debe usar para autenticarse en Azure.

|NOMBRE|DESCRIPCIÓN|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Wait-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="add-a-connection-type-to-your-module"></a>Agregar un tipo de conexión al módulo

Puede proporcionar una personalizada [tipo de conexión](../automation-connections.md) para su uso en su cuenta de Automation mediante la adición de un archivo opcional para el módulo. Este archivo es un archivo de metadatos que especifica un tipo de conexión de Azure Automation para su uso con los cmdlets del módulo en su cuenta de Automation. Para lograr esto debe saber cómo crear un módulo de PowerShell. Para obtener más información sobre la creación de módulo, consulte [cómo escribir un módulo de Script de PowerShell](/powershell/developer/module/how-to-write-a-powershell-script-module).

![Usar una conexión personalizada en el portal de Azure](../media/modules/connection-create-new.png)

Para agregar un tipo de conexión de Azure Automation, el módulo debe contener un archivo con el nombre `<ModuleName>-Automation.json` que especifica las propiedades de tipo de conexión. Se trata de un archivo json, que se coloca dentro de la carpeta del módulo del archivo .zip comprimido. Este archivo contiene los campos de una conexión que es necesaria para conectarse al sistema o servicio que representa el módulo. Esta configuración finaliza la creación de un tipo de conexión en Azure Automation. Uso de este archivo puede establecer los nombres de campo, tipos, y si los campos deben ser cifrados u opcional para el tipo de conexión del módulo. El ejemplo siguiente es una plantilla en el formato de archivo json que define una propiedad de nombre de usuario y contraseña:

```json
{
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
      "TypeName":  "System.String"
   }
   ],
   "ConnectionTypeName":  "MyModuleConnection",
   "IntegrationModuleName":  "MyModule"
}
```

## <a name="module-best-practices"></a>Prácticas recomendadas de módulo

Los módulos de PowerShell pueden importarse en Azure Automation para que sus cmdlets estén disponibles y puedan usarse en runbooks y para que sus recursos de DSC estén disponibles y puedan usarse en las configuraciones de DSC. En segundo plano, Azure Automation almacena estos módulos y en el tiempo de ejecución del trabajo de compilación de DSC y del trabajo de runbook, los carga en espacios aislados de Azure Automation, donde se ejecutan los runbooks y se compilan las configuraciones de DSC. Todos los recursos de DSC en módulos se sitúan automáticamente en el servidor de extracción de DSC de Automatización. Las máquinas los pueden extraer cuando se aplican las configuraciones de DSC.

Se recomienda que considere lo siguiente al crear un módulo de PowerShell para su uso en Azure Automation:

* Incluya una sinopsis, una descripción y un identificador URI de ayuda para todos los cmdlets del módulo. En PowerShell, puede definir cierta información de ayuda para los cmdlets de forma que al usuario le resulte más fácil usarlos con el cmdlet **Get-Help** . El ejemplo siguiente muestra cómo definir una sinopsis y ayudar a los URI para en un archivo de módulo. psm1:

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  Al proporcionar esta información se muestra esta ayuda mediante el cmdlet **Get-Help** en la consola de PowerShell. Esta descripción se muestra también en el portal de Azure.

  ![Ayuda para los módulos de integración](../media/modules/module-activity-description.png)

* Si el módulo se conecta a un servicio externo, debe contener un [tipo de conexión](#add-a-connection-type-to-your-module). En segundo lugar, cada cmdlet del módulo debería poder tomar como parámetro un objeto de conexión (una instancia del tipo de conexión). Esto permite a los usuarios asignar parámetros del recurso de conexión a los parámetros correspondientes del cmdlet cada vez que invoquen un cmdlet. Según el ejemplo de runbook anterior, usa un recurso de conexión de ejemplo Contoso llamado ContosoConnection para tener acceso a recursos de Contoso y devolver datos desde el servicio externo.

  En el ejemplo siguiente, los campos se asignan a las propiedades de nombre de usuario y contraseña de un `PSCredential` de objetos y, a continuación, se pasa al cmdlet.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Una manera más fácil y eficaz de abordar este comportamiento consiste en pasar directamente el objeto de conexión al cmdlet:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  Para habilitar un comportamiento como el del ejemplo anterior en sus cmdlets, permítales que acepten directamente un objeto de conexión como parámetro en lugar de usar solamente campos de conexión en los parámetros. Normalmente, necesitará un conjunto de parámetros para cada uno de ellos, de forma que los usuarios que no utilicen Azure Automation puedan llamar a los cmdlets sin necesidad de crear una tabla hash que actúe como objeto de conexión. El conjunto de parámetros `UserAccount`, se usa para pasar las propiedades de campo de la conexión. `ConnectionObject` le permite pasar directamente a través de la conexión.

* Defina el tipo de salida de todos los cmdlets del módulo. Definir un tipo de salida en un cmdlet permite que IntelliSense le ayude a determinar, en tiempo de diseño, las propiedades de salida del cmdlet, lo que resultará útil durante la creación. Esto resulta especialmente útil durante la creación gráfica de un runbook de Automation, donde la información en tiempo de diseño resulta esencial para facilitar la experiencia del usuario con el módulo.

  Esto puede lograrse mediante la adición de `[OutputType([<MyOutputType>])]` donde MyOutputType es un tipo válido. Para obtener más información acerca de OutputType, vea [sobre funciones OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute). El código siguiente es un ejemplo de cómo agregar `OutputType` a un cmdlet:

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![Tipo de salida de Runbooks gráficos](../media/modules/runbook-graphical-module-output-type.png)

  Este comportamiento es similar a la funcionalidad "type ahead" de la salida de los cmdlets de PowerShell ISE sin necesidad de ejecutarla.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

* Cree todos los cmdlets del módulo sin estado. Varios trabajos de runbook pueden ejecutar simultáneamente en el mismo elemento AppDomain y el mismo proceso y espacio aislado. Si no hay ningún estado compartido en esos niveles, los trabajos pueden afectar entre sí. Este comportamiento puede provocar intermitentes y difíciles de diagnosticar problemas.  A continuación se muestra un ejemplo de lo que no hay que hacer:

  ```powershell
  $globalNum = 0
  function Set-GlobalNum {
     param(
         [int] $num
     )

     $globalNum = $num
  }
  function Get-GlobalNumTimesTwo {
     $output = $globalNum * 2

     $output
  }
  ```

* El módulo debe estar totalmente contenido en un paquete con Xcopy. Módulos de Azure Automation se distribuyen a los espacios aislados de Automation cuando necesite ejecutar runbooks. Los módulos deben trabajar independientemente del host en que se están ejecutando. Debe ser capaz de comprimir y mover un paquete de módulo y conseguir que funcione con normalidad cuando se importe en el entorno de PowerShell de otro host. Para que esto ocurra, el módulo no debería depender de los archivos de fuera de la carpeta del módulo. Esta carpeta es la carpeta que se comprime al importar el módulo en Azure Automation. El módulo tampoco debe depender de ninguna configuración única del Registro en un host, como los parámetros que se establecen cuando se instala un producto. Todos los archivos del módulo deben tener menos de 140 caracteres de una ruta de acceso. Las rutas de acceso con más de 140 caracteres provocará problemas de importación del runbook. Si no se sigue este procedimiento recomendado, el módulo no podrá utilizarse en Azure Automation.  

* Si hace referencia a [módulos de Azure Powershell Az](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) en el módulo, asegúrese de que no hace también referencia a `AzureRM`. El módulo `Az` no se puede usar junto con los módulos `AzureRM`. `Az` se admite en los runbooks, pero no se importa de forma predeterminada. Para obtener información sobre los módulos de `Az` y las consideraciones que debe tener en cuenta, consulte [Az module support in Azure Automation](../az-modules.md) (Soporte técnico para módulos de Az en Azure Automation).

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la creación de módulos de PowerShell, consulte [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)