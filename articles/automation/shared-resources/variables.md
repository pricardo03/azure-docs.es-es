---
title: Recursos de variables en Azure Automation
description: Los activos de variables son valores que están disponibles para todos los runbooks y configuraciones de DSC en Azure Automation.  En este artículo se explican los detalles de las variables y cómo trabajar con ellas en la creación de texto y gráficos.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4ce56b64502904308f45c74a5471447d93419452
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303059"
---
# <a name="variable-assets-in-azure-automation"></a>Recursos de variables en Azure Automation

Los recursos de variables son valores que están disponibles para todos los runbooks y configuraciones de DSC en su cuenta de Automation. Puede administrarlos desde Azure Portal, PowerShell, un runbook o en una configuración de DSC.

Las variables de Automation son útiles para los siguientes escenarios:

- Uso compartido de un valor entre varios runbooks o configuraciones de DSC.

- Uso compartido de un valor entre varios trabajos del mismo runbook o configuración de DSC.

- Administración de un valor que usan los runbooks o las configuraciones de DSC desde el portal o la línea de comandos de PowerShell. Un ejemplo es un conjunto de elementos de configuración comunes, como una lista de nombres de máquinas virtuales específica, un grupo de recursos específico, un nombre de dominio de AD y más.  

Como las variables de Automation se conservan, están disponibles aunque se produzca un error en el runbook o en la configuración de DSC. Este comportamiento permite que un runbook o configuración de DSC establezca un valor para que otro runbook lo use, o bien que lo use el mismo runbook o la misma configuración de DSC la siguiente vez que se ejecute.

Al crear una variable, puede especificar su cifrado y almacenamiento de Azure Automation como recursos seguros. Otros recursos seguros incluyen credenciales, certificados y conexiones. Azure Automation cifra estos recursos y los almacena con una clave única que se genera para cada cuenta de Automation. La clave se almacena en una instancia de Key Vault administrada por el sistema. Antes de almacenar un recurso seguro, Azure Automation carga la clave desde Key Vault y después la usa para cifrar el recurso. 

Azure Automation almacena cada variable cifrada de forma segura. No se puede recuperar su valor mediante el cmdlet [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) que se incluye en el módulo de Azure PowerShell. Solo se puede recuperar un valor cifrado si se usa la actividad **Get-AutomationVariable** en un runbook o una configuración de DSC.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="variable-types"></a>Tipos de variables

Cuando se crea una variable con Azure Portal, debe especificar un tipo de datos en la lista desplegable para que el portal pueda mostrar el control adecuado para escribir el valor de la variable. Los siguientes tipos de variable están disponibles en Azure Automation:

* String
* Entero
* DateTime
* Boolean
* Null

La variable no se limita al tipo de datos designado. Debe establecer la variable mediante Windows PowerShell si se desea especificar un valor de un tipo diferente. Si indica **No definido**, el valor de la variable se establecerá en **null** y deberá establecer el valor con el cmdlet [Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) o con la actividad **Set-AutomationVariable**.

No puede usar el portal para crear o cambiar el valor de un tipo de variable complejo. Sin embargo, puede proporcionar un valor de cualquier tipo mediante Windows PowerShell. Los tipos complejos se recuperan como un objeto [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

Puede almacenar varios valores en una única variable al crear una matriz o tabla hash y guardarla en una variable.

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>Cmdlets de PowerShell que crean y administran recursos de variable

En el módulo Az, los cmdlets de la tabla siguiente se usan para crear y administrar recursos de variable de Automation con Windows PowerShell. Estos cmdlets se incluyen como parte del [módulo Az.Automation](/powershell/azure/overview), que está disponible para usarse en las configuraciones de DSC y los runbooks de Automation.

| Cmdlet | Descripción |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Recupera el valor de una variable existente.|
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Crea una nueva variable y establece su valor.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Quita una variable existente.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Establece el valor de una variable existente.|

## <a name="activities-to-access-variables"></a>Actividades para tener acceso a las variables

Las actividades de la tabla siguiente se usan para acceder a las variables en runbooks y configuraciones de DSC. La diferencia entre **Get-AzAutomationVariable** y **Get-AutomationVariable** se explica para las variables cifradas al comienzo de este artículo.

| Actividad | Descripción |
|:---|:---|
|**Get-AutomationVariable**|Recupera el valor de una variable existente.|
|**Set-AutomationVariable**|Establece el valor de una variable existente.|

> [!NOTE]
> Evite el uso de variables en el parámetro *Name* de **Get-AutomationVariable** en un runbook o configuración de DSC. El uso de dicho parámetro puede complicar la detección de dependencias entre runbooks o configuraciones de DSC y variables de Automation durante el tiempo de diseño.

Las funciones de la tabla siguiente se usan para obtener acceso a las variables de un runbook de Python2 y recuperarlas.

|Funciones de Python2|Descripción|
|:---|:---|
|automationassets.get_automation_variable|Recupera el valor de una variable existente. |
|automationassets.set_automation_variable|Establece el valor de una variable existente. |

> [!NOTE]
> Debe importar el módulo **automationassets** en la parte superior del runbook de Python para tener acceso a las funciones del recurso.

## <a name="creating-a-new-automation-variable"></a>Creación de una nueva variable de Automation

### <a name="create-a-new-variable-using-the-azure-portal"></a>Creación de una nueva variable mediante Azure Portal

1. En la cuenta de Automation, haga clic en el icono **Activos** y, en la hoja **Activos**, seleccione **Variables**.
2. En la hoja **Variables**, seleccione **Agregar una variable**.
3. Complete las opciones en la hoja **Nueva variable** y después haga clic en **Crear** para guardar la nueva variable.

>[!NOTE]
>Si va a quitar el cifrado de una variable, debe eliminarla y volver a crearla sin cifrado.

### <a name="create-a-new-variable-with-windows-powershell"></a>Creación de una nueva variable con Windows PowerShell

El script usa el cmdlet **New-AzAutomationVariable** para crear una nueva variable y configura su valor inicial. A continuación, puede recuperar el valor mediante **Get-AzAutomationVariable**. Si el valor es de un tipo simple, se recupera ese mismo tipo. Si es un tipo complejo, se recupera un tipo **PSCustomObject**.

En el siguiente ejemplo se muestra cómo crear una variable de tipo cadena y después se devuelve su valor.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

En el siguiente ejemplo se muestra cómo crear una variable con un tipo complejo y después se recuperan sus propiedades. En este caso, se usa un objeto de máquina virtual de [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0).

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Uso de una variable en un runbook o una configuración de DSC

Use la actividad **Set-AutomationVariable** para establecer el valor de una variable de Automation en un runbook de PowerShell o una configuración de DSC, y la actividad **Get-AutomationVariable** para recuperarlo. No debe usar los cmdlets **Set-AzAutomationVariable** o **Get-AzAutomationVariable**, ni sus equivalentes en el módulo AzureRM en un runbook o una configuración de DSC, ya que son menos eficientes que las actividades de flujo de trabajo. 

Tenga en cuenta que no puede recuperar el valor de una variable segura con **Get-AzAutomationVariable**, ni con su equivalente del módulo AzureRM. 

La única forma de crear una nueva variable desde un runbook o una configuración de DSC es usar el cmdlet **New-AzAutomationVariable**.

### <a name="textual-runbook-samples"></a>Ejemplos de runbook textual

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>Establecimiento y recuperación de un valor simple de una variable

Los comandos de ejemplo siguientes muestran cómo establecer y recuperar una variable en un runbook textual. En este ejemplo se da por hecho que ha creado variables de entero con el nombre *NumberOfIterations* y *NumberOfRunnings*, así como una variable de cadena con el nombre *SampleMessage*.

```powershell
$NumberOfIterations = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python2-runbook"></a>Establecimiento y recuperación de una variable en un runbook de Python2

En el siguiente ejemplo se muestra cómo usar una variable, establecer una variable y controlar una excepción de una variable inexistente en un runbook de Python2.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Ejemplos de runbook gráfico

En un runbook gráfico, puede agregar la actividad **Get-AutomationVariable** o **Set-AutomationVariable**. Basta con hacer clic derecho en la variable desde el panel Biblioteca del editor gráfico y seleccionar la actividad que se va a agregar.

![Adición de una variable al lienzo](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>Establecimiento de valores en una variable

La imagen siguiente muestra las actividades de ejemplo para actualizar una variable con un valor simple en un runbook gráfico. En este ejemplo, **Get-AzVM** recupera una única máquina virtual de Azure y guarda el nombre del equipo en una variable de Automation existente con tipo Cadena. No importa si el [vínculo es una canalización o una secuencia](../automation-graphical-authoring-intro.md#links-and-workflow), ya que el código solo espera un único objeto en la salida.

![Establecimiento de una variable simple](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre cómo conectar actividades en la creación gráfica, consulte [Vínculos en Creación gráfica](../automation-graphical-authoring-intro.md#links-and-workflow).
- Para empezar a trabajar con runbooks gráficos, consulte [Mi primer runbook gráfico](../automation-first-runbook-graphical.md).
