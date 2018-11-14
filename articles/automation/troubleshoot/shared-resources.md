---
title: Solución de problemas relativos a errores con recursos compartidos de Azure Automation
description: Aprenda sobre la solución de problemas relacionados con los recursos compartidos de Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 385d2969e65647ab0b5c5e21c07b127104587e7e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263420"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Solución de problemas relativos a errores con recursos compartidos

En este artículo se describen soluciones para resolver problemas que pueden surgir al usar recursos compartidos en Azure Automation.

## <a name="modules"></a>Módulos

### <a name="module-stuck-importing"></a>Escenario: Un módulo está bloqueado en el estado de importación

#### <a name="issue"></a>Problema

Al importar o actualizar los módulos en Azure Automation, encuentra un módulo que está bloqueado en el estado de **importación**.

#### <a name="error"></a>Error

La importación de módulos de PowerShell es un proceso complejo de varios pasos. En este proceso existe la posibilidad de que un módulo no se importe correctamente. Si esto ocurre, el módulo que se va a importar puede bloquearse en un estado transitorio. Para más información sobre este proceso, vea [Importing a PowerShell Module]( /powershell/developer/module/importing-a-powershell-module#the-importing-process) (Importación de un módulo de PowerShell).

#### <a name="resolution"></a>Resolución

Para resolver este problema, debe quitar el módulo que está bloqueado en el estado de **importación** con el uso del cmdlet [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule). A continuación, puede volver a intentar importar el módulo.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.