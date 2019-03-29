---
title: Programaciones en Azure Automation
description: Las programaciones de Automation se usan para programar runbooks en Azure Automation para que se inicien automáticamente. Describe cómo crear y administrar una programación de modo que pueda iniciar automáticamente un Runbook en un momento determinado o en una programación periódica.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/22/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d99c6b419ee201be50e74849cd95a332845f5b73
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58623174"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Programación de un runbook en Azure Automation

Para programar un runbook en Azure Automation para que se inicie en un momento determinado, vincúlelo a una o más programaciones. Es posible configurar una programación para que se ejecute una vez o según una programación diaria o por hora recurrente para los runbooks en Azure Portal. También las puede programar para que se ejecuten una vez a la semana, una vez al mes, en días específicos de la semana o del mes, o bien en un día determinado del mes. Un runbook puede vincularse a varias programaciones y una programación puede tener varios runbooks vinculados a ella.

> [!NOTE]
> Las programaciones no admiten actualmente las configuraciones de DSC de Azure Automation.

## <a name="powershell-cmdlets"></a>Cmdlets de PowerShell

Los cmdlets en la tabla siguiente se utilizan para crear y administrar programaciones con PowerShell en Azure Automation. Se incluyen como parte del [módulo Azure PowerShell](/powershell/azure/overview).

| Cmdlets | DESCRIPCIÓN |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Recupera una programación. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Crea una nueva programación. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Quita una programación. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Establece las propiedades de una programación existente. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Recupera runbooks programados. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Asocia un runbook con una programación. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Anula la asociación de un runbook con una programación. |

## <a name="creating-a-schedule"></a>Creación de una programación

Puede crear una nueva programación para runbooks en Azure portal o con PowerShell.

> [!NOTE]
> Azure Automation utiliza los módulos más recientes en su cuenta de Automation al ejecutar un nuevo trabajo programado.  Para evitar afectar a los runbooks y a los procesos que automatizan, primero debe probar los runbooks que tengan programaciones vinculadas con una cuenta de Automation dedicada para realizar pruebas.  De este modo valida que los runbooks programados sigan funcionando correctamente y, si no es así, puede seguir investigando para solucionar el problema y aplicar los cambios necesarios antes de migrar la versión actualizada del runbook al entorno de producción.
> La cuenta de Automation no obtiene automáticamente las nuevas versiones de los módulos, a menos que se hayan actualizado manualmente seleccionando la opción [Actualizar módulos de Azure](../automation-update-azure-modules.md) desde **Módulos**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Para crear una programación en el Portal de Azure

1. En Azure Portal, desde su cuenta de Automation, seleccione **programaciones** en la sección **Recursos compartidos** de la izquierda.
2. Haga clic en **Agregar una programación** en la parte superior de la página.
3. En el panel **Nueva programación**, escriba un valor en **Nombre** y, opcionalmente, en **Descripción**, para la nueva programación.
4. Seleccione si la programación se ejecuta una vez o de forma periódica seleccionando **Una vez** o **Periódico**. Si selecciona **Una vez**, especifique una hora en **Hora de inicio** y haga clic en **Crear**. Si selecciona **Periódico**, especifique un valor en **Hora de inicio** y, en **Repetir cada**, seleccione la frecuencia con la que quiere que se repita el runbook: cada **hora**, **día**, **semana** o **mes**.
    1. Si selecciona **semana**, les proporciona una lista de los días de la semana para elegir. Seleccione tantos días como quiera. La primera ejecución de la programación se realizará en el primer día seleccionado después de la hora de inicio. Por ejemplo, para elegir una programación de fin de semana, elija **el sábado** y **el domingo**.

       ![Programación periódica del fin de semana de configuración](../media/schedules/week-end-weekly-recurrence.png)

    2. Si selecciona **mes**, se le asigna diferentes opciones. Para el **las repeticiones mensuales** opción, seleccione **días del mes** o **días de la semana**. Si elige **días del mes**, se muestra un calendario que le permite elegir tantos días como desee. Si elige una fecha como el día 31 que no se producen en el mes actual, no se ejecutará la programación. Si quiere que la programación se ejecute en el último día, elija **Sí** en **Ejecutar en el último día del mes**. Si elige **Días de la semana**, se mostrará la opción **Repetir cada**. Elija **Primero**, **Segundo**, **Tercero**, **Cuarto** o **Último**. Por último, elija un día en el que realizar la repetición.

       ![Programación mensual decimoquinto, primero y último día del mes](../media/schedules/monthly-first-fifteenth-last.png)

5. Cuando termine, haga clic en **Crear**.

### <a name="to-create-a-new-schedule-with-powershell"></a>Para crear una nueva programación con PowerShell

Use el cmdlet [AzureRmAutomationSchedule New](/powershell/module/azurerm.automation/new-azurermautomationschedule) para crear programaciones. Especifique la hora de inicio de la programación y la frecuencia con que se debe ejecutar. Los ejemplos siguientes muestran cómo crear muchos escenarios de programación diferente.

#### <a name="create-a-one-time-schedule"></a>Crear una programación de tiempo

Los siguientes comandos de ejemplo crean una programación de tiempo.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Crear una programación periódica

Los comandos de ejemplo siguientes muestran cómo crear una programación periódica que se ejecuta cada día a las 13:00 durante un año.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Crear una programación periódica semanal

Los comandos de ejemplo siguientes muestran cómo crear una programación semanal que se ejecuta en los días laborables únicamente.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Crear una programación periódica semanal para los fines de semana

Los comandos de ejemplo siguientes muestran cómo crear una programación semanal que se ejecuta solo los fines de semana.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-first-15th-and-last-days-of-the-month"></a>Crear una programación periódica para la primera, 15 y el último día del mes

Los comandos de ejemplo siguientes muestran cómo crear una programación periódica que se ejecuta en el día 1, 15 y último del mes.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzureRmAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="linking-a-schedule-to-a-runbook"></a>Vinculación de una programación a un runbook

Un runbook puede vincularse a varias programaciones y una programación puede tener varios runbooks vinculados a ella. Si un runbook tiene parámetros, puede proporcionar valores para ellos. Debe proporcionar valores para los parámetros obligatorios y puede proporcionar valores para los parámetros opcionales. Estos valores se usan cada vez que esta programación inicia el runbook. Puede adjuntar el mismo runbook a otra programación y especificar distintos valores de parámetros.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Para vincular una programación a un runbook con el Portal de Azure

1. En Azure Portal, desde su cuenta de Automation, seleccione **Runbooks** en la sección **Automatización de procesos** de la izquierda.
2. Haga clic en el nombre del runbook que se va a programar.
3. Si el runbook no está vinculado actualmente a una programación, a continuación, le ofrece la opción para crear una nueva programación o un vínculo a una programación existente.
4. Si el runbook tiene parámetros, puede seleccionar la opción **modificar la configuración de ejecución (predeterminada: Azure)** y **parámetros** se presenta panel donde puede especificar la información.

### <a name="to-link-a-schedule-to-a-runbook-with-powershell"></a>Para vincular una programación a un runbook con PowerShell

Puede usar el cmdlet [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) para vincular una programación. Puede especificar valores para los parámetros del runbook con el parámetro Parameters. Para obtener más información sobre cómo especificar valores de parámetro, vea [Inicio de un runbook en Azure Automation](../automation-starting-a-runbook.md).
Los siguientes comandos de ejemplo muestran cómo vincular una programación a un runbook mediante un cmdlet de Azure Resource Manager con parámetros.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Programación más frecuente de runbooks

El intervalo de mayor frecuencia con que se puede configurar una programación en Azure Automation es una hora. Si necesita programaciones que se ejecuten con mayor frecuencia, tiene dos opciones:

* Crear un [webhook](../automation-webhooks.md) para el runbook y use [Azure Scheduler](../../scheduler/scheduler-get-started-portal.md) para llamar al webhook. Azure Scheduler ofrece una granularidad más específica al definir una programación.

* Crear cuatro programaciones que comiencen con una diferencia de 15 minutos entre sí y que se ejecuten una vez cada hora. Este escenario permite que el runbook se ejecute cada 15 minutos con las distintas programaciones.

## <a name="disabling-a-schedule"></a>Deshabilitación de una programación

Cuando se deshabilita una programación, los runbooks vinculados a ella ya no se ejecutan en ella. Puede deshabilitar una programación manualmente o establecer una fecha de expiración para las programaciones con frecuencia al crearlas. Una vez que se alcanza la fecha de expiración, la programación está deshabilitada.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Para deshabilitar una programación desde el Portal de Azure

1. En Azure Portal, desde su cuenta de Automation, seleccione **programaciones** en la sección **Recursos compartidos** de la izquierda.
2. Haga clic en el nombre de una programación para abrir el panel de detalles.
3. Cambie **Habilitado** a **No**.

### <a name="to-disable-a-schedule-with-powershell"></a>Para deshabilitar una programación con PowerShell

Puede usar el cmdlet [AzureRmAutomationSchedule conjunto](/powershell/module/azurerm.automation/set-azurermautomationschedule) para cambiar las propiedades de una programación existente. Para deshabilitar la programación, especifique **false** para el parámetro **IsEnabled**.

Los siguientes comandos de ejemplo muestran cómo deshabilitar una programación para un runbook mediante un cmdlet de Azure Resource Manager.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Pasos siguientes

* Para empezar a trabajar con Runbooks, vea [Inicio de un runbook en Azure Automation](../automation-starting-a-runbook.md)

