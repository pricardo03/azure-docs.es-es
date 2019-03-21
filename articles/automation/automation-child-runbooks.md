---
title: Runbooks secundarios en Azure Automation
description: Describe los diferentes métodos para iniciar un runbook en Azure Automation desde otro runbook y compartir información entre ellos.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 84f17b76f03c01d0b1441a50b9bcbddc1dfe2ef3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57851320"
---
# <a name="child-runbooks-in-azure-automation"></a>Runbooks secundarios en Azure Automation

Un procedimiento recomendado en Azure Automation es escribir runbooks que sean reutilizables y modulares con una función independiente que puedan usar otros runbooks. Con frecuencia, un runbook primario llama a uno o varios runbooks secundarios para realizar la funcionalidad necesaria. Existen dos maneras de llamar a un runbook secundario y cada una tiene varias diferencias que debería conocer para poder determinar cuál de ellas es mejor para los diferentes escenarios.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Invocación de un runbook secundario mediante la ejecución en línea

Para invocar un runbook en línea desde otro runbook, use el nombre del runbook y proporcione valores para sus parámetros, exactamente igual a como usaría una actividad o un cmdlet.  De esta manera, todos los runbooks de la misma cuenta de Automation estarán disponibles para que todos los demás los usen. El runbook principal esperará a que el runbook secundario se complete antes de pasar a la siguiente línea, y cualquier resultado se devolverá directamente al elemento primario.

Al invocar un runbook en línea, este se ejecuta en el mismo trabajo que el runbook primario. No hay ninguna indicación en el historial de trabajos del runbook secundario que se haya ejecutado. Todas las excepciones y salidas de flujo pertenecientes al runbook secundario se asociarán al primario. Como resultado de este comportamiento, hay menos trabajos y se facilita el seguimiento y la solución de problemas, ya que las excepciones producidas por el runbook secundario y las salidas de flujo se asocian al trabajo primario.

Cuando se publica un runbook, cualquier runbook secundario al que llame deberá estar ya estar publicado. Esto es así porque Azure Automation crea una asociación con los runbooks secundarios cuando se compila un runbook. En caso contrario, parece que el runbook primario se publica correctamente, pero generará una excepción cuando se inicie. Si esto sucediera, puede volver a publicar el runbook primario para que haga referencia correctamente a los runbooks secundarios. No es necesario que vuelva a publicar el runbook primario si cambia uno de los runbooks secundarios, porque ya se habrá creado la asociación.

Los parámetros de un runbook secundario llamado insertado pueden ser de cualquier tipo de datos (incluidos objetos complejos). No hay ninguna [serialización JSON](start-runbooks.md#runbook-parameters) como sucede cuando se inicia el runbook mediante Azure Portal o mediante el cmdlet Start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Tipos de runbook

Qué tipos pueden llamarse entre sí:

* Un [runbook de PowerShell](automation-runbook-types.md#powershell-runbooks) y los [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) pueden llamarse entre sí en línea (ambos se basan en PowerShell).
* Un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) y los runbooks gráficos de flujo de trabajo de PowerShell pueden llamarse entre sí en línea (ambos tipos se basan en el flujo de trabajo de PowerShell).
* Los tipos de PowerShell y los tipos de flujo de trabajo de PowerShell no pueden llamarse entre sí en línea, y deben utilizar Start-AzureRmAutomationRunbook.

Cuándo es importante el orden de publicación:

* El orden de publicación de runbooks solo es importante para los runbooks de flujo de trabajo de PowerShell y los runbooks gráficos de flujo de trabajo de PowerShell.

Cuando se llama a un runbook secundario gráfico o de flujo de trabajo de PowerShell con la ejecución insertada, use el nombre del runbook.  Cuando se llama a un runbook secundario de PowerShell, el nombre debe comenzar con *.\\* para especificar que el script se encuentra en el directorio local.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se inicia un runbook secundario de prueba que acepta tres parámetros: un objeto complejo, un entero y un valor booleano. Los resultados del runbook secundario se asignan a una variable.  En este caso, el runbook secundario es un runbook de flujo de trabajo de PowerShell.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

A continuación, se muestra el mismo ejemplo con un runbook de PowerShell como elemento secundario.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>Inicio de un runbook secundario mediante un cmdlet

> [!IMPORTANT]
> Si invoca un runbook secundario con el cmdlet `Start-AzureRmAutomationRunbook` con el conmutador `-Wait` y el resultado del runbook secundario es un objeto, puede que encuentre errores. Para solucionar el error, consulte [Runbooks secundarios con salida de objetos](troubleshoot/runbooks.md#child-runbook-object) para aprender a implementar la lógica de sondeo en busca de resultados y a usar el cmdlet [Get AzureRmAutomationJobOutputRecord](/powershell/module/azurerm.automation/get-azurermautomationjoboutputrecord)

Puede utilizar el cmdlet [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) para iniciar un runbook, tal como se describe en [To start a runbook with Windows PowerShell](start-runbooks.md#start-a-runbook-with-powershell) (Inicio de un runbook con Windows PowerShell). Existen dos modos de usar este cmdlet.  En un modo, el cmdlet devuelve el identificador del trabajo cuando se crea el trabajo secundario del runbook secundario.  En el otro, que se habilita mediante la especificación del parámetro **-wait**, el cmdlet espera a que finalice el trabajo secundario y devuelve los resultados del runbook secundario.

Si inició el trabajo de un runbook secundario con un cmdlet, este se ejecuta en un trabajo independiente al runbook primario. Como resultado de este comportamiento, habrá más trabajos que cuando se inicia el runbook insertado, por lo que realizar un seguimiento será más difícil. Asimismo, el primario puede iniciar varios runbooks secundarios de forma asincrónica sin tener que esperar a que se complete cada uno de ellos. Para realizar este mismo tipo de ejecución en paralelo, al llamar a los runbooks secundarios en línea, el runbook primario necesitará usar la [palabra clave parallel](automation-powershell-workflow.md#parallel-processing).

La salida de los runbooks secundarios no se devuelve al runbook primario de forma confiable debido al tiempo. Además, ciertas variables como $VerbosePreference, $WarningPreference, entre otras, no se pueden propagar a los runbooks secundarios. Para evitar estos problemas, puede iniciar los runbooks secundarios como trabajos de Automation independientes mediante el cmdlet `Start-AzureRmAutomationRunbook` con el modificador `-Wait`. Esto bloquea el runbook primario hasta que se completa el runbook secundario.

Si no quiere bloquear el runbook primario durante la espera, puede iniciar el runbook secundario mediante el cmdlet `Start-AzureRmAutomationRunbook` sin el modificador `-Wait`. Luego tendría que usar `Get-AzureRmAutomationJob` para esperar a que se complete el trabajo y `Get-AzureRmAutomationJobOutput` y `Get-AzureRmAutomationJobOutputRecord` para recuperar los resultados.

Los parámetros de un runbook secundario iniciado con un cmdlet se proporcionan como una tabla hash, tal y como se describe en [Parámetros de runbook](start-runbooks.md#runbook-parameters). Solo pueden usarse los tipos de datos simples. Si el runbook tiene un parámetro con un tipo de datos complejo, debe llamarse en línea.

Es posible que se pierda el contexto de suscripción cuando inicia runbooks secundarios como trabajos individuales. Para que el runbook secundario ejecute los cmdlet de Azure RM en una suscripción de Azure específica, el runbook secundario debe autenticarse en esta suscripción de forma independiente al runbook primario.

Si los trabajos de una misma cuenta de Automation funcionan con varias suscripciones, seleccionar una suscripción en uno de los trabajos puede cambiar también el contexto de suscripción ya seleccionado en otros trabajos. Para evitar este problema, use `Disable-AzureRmContextAutosave –Scope Processsave` al principio de cada runbook. Esta acción solo guarda el contexto de esa ejecución de runbook.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se inicia un Runbook secundario con parámetros y se espera a que finalice mediante el parámetro Start-AzureRmAutomationRunbook -wait. Una vez completado, los resultados se recopilan desde el Runbook secundario. Para usar `Start-AzureRmAutomationRunbook`, debe autenticarse en su suscripción de Azure.

```azurepowershell-interactive
# Ensures you do not inherit an AzureRMContext in your runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRMContext $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Comparación de métodos para llamar a un runbook secundario

En la siguiente tabla se resumen las diferencias entre los dos métodos para llamar a un runbook desde otro runbook.

|  | En línea | Cmdlet |
|:--- |:--- |:--- |
| Trabajo |Los runbooks secundarios se ejecutan en el mismo trabajo que el primario. |Se crea un trabajo independiente para el runbook secundario. |
| Ejecución |El runbook primario espera a que el runbook secundario se complete antes de continuar. |El runbook primario continúa inmediatamente después de que se inicie el runbook secundario *o* el runbook primario espera a que finalice el trabajo secundario. |
| Salida |El runbook primario puede obtener los resultados directamente del runbook secundario. |El runbook primario debe obtener los resultados directamente del trabajo de runbook secundario *o* el runbook primario puede obtener los resultados directamente del runbook secundario. |
| Parámetros |Los valores de los parámetros del runbook secundario se especifican por separado y pueden usar cualquier tipo de datos. |Los valores de los parámetros del runbook secundario deben combinarse en una sola tabla hash. Esta tabla hash solo puede incluir tipos de datos simples, de matriz y de objeto que usen la serialización JSON. |
| Cuenta de Automation |El runbook primario solo puede usar el runbook secundario en la misma cuenta de automatización. |Los runbooks primarios pueden usar un runbook secundario desde cualquier cuenta de Automation de la misma suscripción de Azure, e incluso de otra suscripción si tiene una conexión a ella. |
| Publicación |El runbook secundario debe publicarse antes de publicar el runbook primario. |El runbook secundario debe publicarse en cualquier momento antes de iniciar el runbook primario. |

## <a name="next-steps"></a>Pasos siguientes

* [Inicio de un runbook en Azure Automation](start-runbooks.md)
* [Salidas de runbook y mensajes en Azure Automation](automation-runbook-output-and-messages.md)

