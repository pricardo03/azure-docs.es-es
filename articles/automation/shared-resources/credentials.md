---
title: Recursos de credenciales en Azure Automation
description: Los recursos de credenciales en Azure Automation contienen credenciales de seguridad que se pueden usar para realizar la autenticación a los recursos a los que tiene acceso el runbook o configuración de DSC. En este artículo se describe cómo crear recursos de credenciales y usarlos en un runbook o una configuración de DSC.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 04/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: bb34c3f04302e6a2b5cc307b98bafe93e09fcf2f
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734709"
---
# <a name="credential-assets-in-azure-automation"></a>Recursos de credenciales en Azure Automation

Un recurso de credencial de Automation contiene un objeto que contiene las credenciales de seguridad, como un nombre de usuario y una contraseña. Los runbooks y las configuraciones de DSC pueden usar cmdlets que aceptan un objeto PSCredential para autenticación, o bien pueden extraer el nombre de usuario y la contraseña el objeto PSCredential para proporcionarlos a alguna aplicación o servicio que requiere autenticación. Las propiedades de una credencial se almacenan de manera segura en Azure Automation y es posible tener acceso a ellas en el runbook o la configuración de DSC con la actividad [Get-AutomationPSCredential](#activities) .

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Los recursos protegidos en Azure Automation incluyen credenciales, certificados, conexiones y variables cifradas. Estos recursos se cifran y se almacenan en Azure Automation con una clave única que se genera para cada cuenta de Automation. Esta clave se almacena en Key Vault. Antes de almacenar un recurso seguro, la clave se carga desde Key Vault y luego se usa para cifrar el recurso.

## <a name="azure-classic-powershell-cmdlets"></a>Cmdlets de PowerShell de Azure Classic

Los cmdlets de la tabla siguiente se usan para crear y administrar recursos de credenciales de Automatización con Windows PowerShell.  Se incluyen como parte de la [módulo Azure PowerShell](/powershell/azure/overview), que está disponible para su uso en los runbooks de Automation y las configuraciones de DSC.

| Cmdlets | DESCRIPCIÓN |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/servicemanagement/azure/get-azureautomationcredential) |Recupera información acerca de un recurso de credencial. Solo puede recuperar la propia credencial desde la actividad **Get-AutomationPSCredential** . |
| [New-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Crea una nueva credencial de Automation. |
| [Remove-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Quita una credencial de Automation. |
| [Set-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Establece las propiedades para una credencial de Automatización existente. |

## <a name="azurerm-powershell-cmdlets"></a>Cmdlets de AzureRM PowerShell

En AzureRM, los cmdlets de la tabla siguiente se usan para crear y administrar recursos de credenciales de automatización con Windows PowerShell.  Se incluyen como parte de la [módulo Azurerm](/powershell/azure/overview), que está disponible para su uso en los runbooks de Automation y las configuraciones de DSC.

| Cmdlets | DESCRIPCIÓN |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](/powershell/module/azurerm.automation/get-azurermautomationcredential) |Recupera información acerca de un recurso de credencial. Esto no devuelve un objeto PSCredential.  |
| [New-AzureRmAutomationCredential](/powershell/module/azurerm.automation/new-azurermautomationcredential) |Crea una nueva credencial de Automation. |
| [Remove-AzureRmAutomationCredential](/powershell/module/azurerm.automation/remove-azurermautomationcredential) |Quita una credencial de Automation. |
| [Set-AzureRmAutomationCredential](/powershell/module/azurerm.automation/set-azurermautomationcredential) |Establece las propiedades para una credencial de Automatización existente. |

## <a name="activities"></a>Actividades

Las actividades de la tabla siguiente se usan para tener acceso a las credenciales de un runbook y configuraciones de DSC.

| Actividades | DESCRIPCIÓN |
|:--- |:--- |
| Get-AutomationPSCredential |Obtiene una credencial para usarla en un runbook o una configuración de DSC. Devuelve un objeto [System.Management.Automation.PSCredential](/dotnet/api/system.management.automation.pscredential) . |

> [!NOTE]
> Debe evitar el uso de variables en el parámetro –Name de Get-AutomationPSCredential, debido a que esto podría complicar la detección de las dependencias entre runbooks o configuraciones de DSC, y recursos de credenciales en tiempo de diseño.

## <a name="python2-functions"></a>Funciones de Python2

La función de la tabla siguiente se usa para obtener acceso a las credenciales de un runbook de Python2.

| Función | DESCRIPCIÓN |
|:---|:---|
| automationassets.get_automation_credential | Recupera información acerca de un recurso de credencial. |

> [!NOTE]
> Debe importar el módulo "automationassets" en la parte superior del runbook de Python para poder tener acceso a las funciones del recurso.

## <a name="creating-a-new-credential-asset"></a>Creación de un nuevo recurso de credencial

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Para crear un nuevo recurso de credencial con el Portal de Azure

1. En la cuenta de Automation, seleccione **Credenciales** en **Recursos compartidos**.
1. Haga clic en **+ Agregar una credencial**.
1. Complete el formulario y haga clic en **Crear** para guardar la nueva credencial.

> [!NOTE]
> No se admiten cuentas de usuario que usen la autenticación multifactor en Azure Automation.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Para crear un nuevo recurso de credencial con Windows PowerShell

Los comandos de ejemplo siguientes muestran cómo crear una nueva credencial de automatización. Primero se crea un objeto PSCredential con el nombre y la contraseña y, a continuación, se usa para crear el recurso de credencial. De manera alternativa, podría usar el cmdlet **Get-Credential** para que se le solicite escribir un nombre y una contraseña.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Uso de una credencial de PowerShell

Puede recuperar un activo de credencial en un runbook o una configuración de DSC con la actividad **Get-AutomationPSCredential** . Esto devuelve un [objeto PSCredential](/dotnet/api/system.management.automation.pscredential) que puede usar con una actividad o cmdlet que requiere un parámetro PSCredential. También puede recuperar las propiedades del objeto de credencial para usarlas individualmente. El objeto tiene una propiedad para el nombre de usuario y la contraseña segura, o bien puede usar el método **GetNetworkCredential** para devolver un objeto [NetworkCredential](/dotnet/api/system.net.networkcredential) que proporcionará una versión no protegida de la contraseña.

> [!NOTE]
> **Get-AzureRmAutomationCredential** no devuelve un **PSCredential** que puede utilizarse para la autenticación. Solo se proporciona información acerca de la credencial. Si necesita usar una credencial en un runbook debe usar el **Get-AutomationPSCredential** para recuperar el **PSCredential** objeto.

### <a name="textual-runbook-sample"></a>Ejemplo de runbook de texto

Los comandos de ejemplo siguientes muestran cómo usar una credencial de PowerShell en un runbook. En este ejemplo, se recupera la contraseña y su nombre de usuario y contraseña se asignan a variables.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

También puede usar una credencial para autenticarse en Azure con [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount). En la mayoría de los casos, debe usar un [cuenta de ejecución](../manage-runas-account.md) y recuperarla con [Get-AutomationConnection](../automation-connections.md).

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Ejemplo de runbook gráfico

Para agregar una actividad **Get-AutomationPSCredential** a un runbook gráfico, haga clic con el botón derecho en la credencial en el panel Biblioteca del editor gráfico y, después, seleccione **Agregar a lienzo**.

![Agregar credencial a lienzo](../media/credentials/credential-add-canvas.png)

La imagen siguiente muestra un ejemplo de cómo usar una credencial en un runbook gráfico.  En este caso, se usa para proporcionar autenticación para un runbook a recursos de Azure, como se describe en [autenticar Runbooks con la cuenta de usuario de Azure AD](../automation-create-aduser-account.md).  La primera actividad recupera la credencial que tiene acceso a la suscripción de Azure.  A continuación, la actividad **Add-AzureAccount** usa esta credencial para proporcionar autenticación para cualquier actividad que venga después.  Aquí se encuentra un [vínculo de canalización](../automation-graphical-authoring-intro.md#links-and-workflow) , debido a que **Get-AutomationPSCredential** espera un solo objeto.  

![Agregar credencial a lienzo](../media/credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Uso de una credencial de PowerShell en DSC

Mientras las configuraciones de DSC en Azure Automation pueden hacer referencia a recursos de credenciales mediante **Get-AutomationPSCredential**, recursos de credenciales también se pueden pasar en a través de parámetros, si quería. Para obtener más información, consulte [Compilación de configuraciones en DSC de Azure Automation](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Uso de credenciales en Python2

En el ejemplo siguiente se muestra un acceso a credenciales en runbooks de Python2.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información acerca de los vínculos en creación gráfica, consulte [Vínculos en Creación gráfica](../automation-graphical-authoring-intro.md#links-and-workflow)
* Para entender los distintos métodos de autenticación con la automatización, vea [Seguridad de Azure Automation](../automation-security-overview.md)
* Para empezar a trabajar con runbooks gráficos, consulte [Mi primer runbook gráfico](../automation-first-runbook-graphical.md)
* Para empezar a trabajar con runbooks de flujo de trabajo de PowerShell, consulte [Mi primer runbook de flujo de trabajo de PowerShell](../automation-first-runbook-textual.md)
* Para empezar a trabajar con runbooks de Python2, vea [My first Python2 runbook](../automation-first-runbook-textual-python2.md) (Mi primer runbook de Python2). 
