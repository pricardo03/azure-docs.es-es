---
title: Solución de problemas con Desired State Configuration (DSC) de Azure Automation
description: En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 63bb5c6338cf230c2bb47cb0a2c03810053f970a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61087275"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Solución de problemas de Desired State Configuration (DSC)

En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Errores comunes al trabajar con la Configuración de estado deseado (DSC)

### <a name="unsupported-characters"></a>Escenario: No se puede eliminar una configuración con caracteres especiales en el portal

#### <a name="issue"></a>Problema

Cuando se intenta eliminar una configuración de DSC desde el portal, consulte el siguiente error:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Causa

Este error es un problema temporal que se prevé que se resuelva.

#### <a name="resolution"></a>Resolución

* Use el Cmdlet de Az "Remove-AzAutomationDscConfiguration" para eliminar la configuración.
* Todavía no se ha actualizado la documentación de este cmdlet.  Hasta entonces, consulte la documentación del módulo de AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Escenario: No se pudo registrar el agente de Dsc

#### <a name="issue"></a>Problema

Cuando se intenta ejecutar `Set-DscLocalConfigurationManager` u otro cmdlet DSC recibe el error:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Causa

Este error se debe normalmente a un servidor de seguridad, la máquina está detrás de un servidor proxy, u otros errores de red.

#### <a name="resolution"></a>Resolución

Compruebe que el equipo tiene acceso a los puntos de conexión adecuadas para DSC de automatización de Azure y vuelva a intentarlo. Para obtener una lista de puertos y direcciones necesarias, vea [planeamiento de red](../automation-dsc-overview.md#network-planning)

### <a name="failed-not-found"></a>Escenario: el nodo se encuentra en estado de error con el error "No encontrado"

#### <a name="issue"></a>Problema

El nodo tiene un informe con estado **erróneo** y contiene el error:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Causa

Este error suele ocurrir cuando se asigna al nodo un nombre de configuración (por ejemplo, ABC), en lugar de un nombre de configuración de nodo (por ejemplo, ABC.WebServer).

#### <a name="resolution"></a>Resolución

* Asegúrese de que se va a asignar el nodo con el "nombre de la configuración de nodo" y no el "nombre de configuración".
* Puede asignar una configuración de nodo a un nodo mediante el Portal de Azure o con un cmdlet de PowerShell.

  * Para asignar una configuración de nodo a un nodo mediante Azure portal, abra el **nodos DSC** página, a continuación, seleccione un nodo y haga clic en **asignar configuración de nodo** botón.  
  * Para asignar una configuración de nodo a un nodo mediante el cmdlet de PowerShell, use **AzureRmAutomationDscNode Set** cmdlet

### <a name="no-mof-files"></a>Escenario: no se produjeron configuraciones de nodo (archivos MOF) al compilarse una configuración

#### <a name="issue"></a>Problema

Su trabajo de compilación de DSC suspende con el error:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Causa

Cuando la expresión que aparece junto a la palabra clave **Node** en la configuración de DSC se evalúa como `$null`, no se produce ninguna configuración de nodo.

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:

* Asegúrese de que la expresión junto a la **nodo** palabra clave en la definición de configuración no se está evaluando como $null.
* Si se pasan datos de configuración al compilar la configuración, asegúrese de que se pasan los valores esperados que la configuración necesita de [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Escenario: el informe de nodo de DSC se queda bloqueado en el estado "en curso"

#### <a name="issue"></a>Problema

El agente DSC genera la salida:

```error
No instance found with given property values
```

#### <a name="cause"></a>Causa

Ha actualizado la versión de WMF y ha dañado WMI.

#### <a name="resolution"></a>Resolución

Para corregir el problema, siga las instrucciones de la [problemas y limitaciones conocidos de DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) artículo.

### <a name="issue-using-credential"></a>Escenario: no se puede usar una credencial en una configuración de DSC

#### <a name="issue"></a>Problema

El trabajo de compilación de DSC se ha suspendido con el error:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Causa

Ha usado una credencial en una configuración, pero no proporcionó adecuado **ConfigurationData** establecer **PSDscAllowPlainTextPassword** en true para cada configuración de nodo.

#### <a name="resolution"></a>Resolución

* Asegúrese de pasar el valor adecuado **ConfigurationData** establecer **PSDscAllowPlainTextPassword** como true para cada configuración de nodo que se menciona en la configuración. Para más información, consulte los [recursos en DSC de Azure Automation](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
