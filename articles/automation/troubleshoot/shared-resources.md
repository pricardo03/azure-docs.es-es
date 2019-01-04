---
title: Solución de problemas relativos a errores con recursos compartidos de Azure Automation
description: Aprenda sobre la solución de problemas relacionados con los recursos compartidos de Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ce78c86cdae9a06100fd17d00e0229805e42983b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848466"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Solución de problemas relativos a errores con recursos compartidos

En este artículo se describen soluciones para resolver problemas que pueden surgir al usar recursos compartidos en Azure Automation.

## <a name="modules"></a>Módulos

### <a name="module-stuck-importing"></a>Escenario: Un módulo está bloqueado en el estado de importación

#### <a name="issue"></a>Problema

Al importar o actualizar los módulos en Azure Automation, encuentra un módulo que está bloqueado en el estado de **importación**.

#### <a name="cause"></a>Causa

La importación de módulos de PowerShell es un proceso complejo de varios pasos. En este proceso existe la posibilidad de que un módulo no se importe correctamente. Si esto ocurre, el módulo que se va a importar puede bloquearse en un estado transitorio. Para más información sobre este proceso, vea [Importing a PowerShell Module]( /powershell/developer/module/importing-a-powershell-module#the-importing-process) (Importación de un módulo de PowerShell).

#### <a name="resolution"></a>Resolución

Para resolver este problema, debe quitar el módulo que está bloqueado en el estado de **importación** con el uso del cmdlet [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule). A continuación, puede volver a intentar importar el módulo.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="run-as-accounts"></a>Cuentas de ejecución

### <a name="unable-create-update"></a>Escenario: No puede crear o actualizar una cuenta de ejecución

#### <a name="issue"></a>Problema

Al intentar crear o actualizar una cuenta de ejecución, recibe un error similar al siguiente mensaje de error:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Causa

No tiene los permisos que necesita para crear o actualizar la cuenta de ejecución o el recurso está bloqueado en un nivel de grupo de recursos.

#### <a name="resolution"></a>Resolución

Para crear o actualizar una cuenta de ejecución, debe tener los permisos adecuados para los distintos recursos que usa la cuenta de ejecución. Para obtener información acerca de los permisos necesarios para crear o actualizar una cuenta de ejecución, consulte [Permisos para configurar cuentas de ejecución](../manage-runas-account.md#permissions).

Si el problema se debe a un bloqueo, verifique que el bloqueo se puede eliminar y navegue hasta el recurso que está bloqueado, haga clic derecho en el bloqueo y seleccione **Eliminar** para eliminar el bloqueo.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.