---
title: Solución de problemas con Desired State Configuration (DSC) de Azure Automation
description: En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).
services: automation
ms.service: automation
ms.subservice: ''
author: bobbytreed
ms.author: robreed
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7be5e814d8092b523fa69fdd84f0e1476736fda2
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73887715"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Solución de problemas de Desired State Configuration (DSC)

En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).

## <a name="steps-to-troubleshoot-desired-state-configuration-dsc"></a>Pasos para solucionar problemas de Desired State Configuration (DSC)

Si se producen errores al compilar o implementar configuraciones en Azure State Configuration, aquí tiene algunos pasos que le ayudarán a diagnosticar el problema.

1. **Asegúrese de que la configuración se compila correctamente en la máquina local:**  Azure State Configuration se basa en DSC de PowerShell. Puede encontrar la documentación del lenguaje y la sintaxis de DSC en los [documentos de DSC de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).

   Al compilar la configuración de DSC en la máquina local, puede detectar y resolver errores comunes, por ejemplo:

   - **Módulos faltantes**
   - **Errores de sintaxis**
   - **Errores lógicos**

2. **Vea los registros de DSC en el nodo:** Si la configuración se compila correctamente, pero se produce un error cuando se aplica a un nodo, puede encontrar información detallada en los registros. Para obtener información sobre dónde encontrar los registros de DSC, consulte [¿Dónde se encuentran los registros de eventos de DSC?](/powershell/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs)

   Además, [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) puede ayudarlo a analizar información detallada de los registros de DSC. Si se pone en contacto con el soporte técnico, necesitarán estos registros para diagnosticar el problema.

   Puede instalar **xDscDiagnostics** en la máquina local con las instrucciones que se encuentran en [Install the stable version module](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module) (Instalar el módulo de versión estable).

   Para instalar **xDscDiagnostics** en la máquina de Azure, puede usar [az vm run-command](/cli/azure/vm/run-command) o [Invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand). También puede usar la opción **Ejecutar comando** desde el portal, siguiendo los pasos que se describen en [Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar](../../virtual-machines/windows/run-command.md).

   Para obtener información sobre **xDscDiagnostics**, consulte [Uso de xDscDiagnostics para analizar registros de DSC](/powershell/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs), así como los [cmdlets de xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics#cmdlets).
3. **Asegúrese de que los nodos y el área de trabajo de Automation tengan los módulos necesarios:** Desired State Configuration depende de los módulos instalados en el nodo.  Al usar Azure Automation State Configuration, importe los módulos necesarios a la cuenta de Automation mediante los pasos indicados en [Importación de módulos](../shared-resources/modules.md#import-modules). Las configuraciones también pueden tener una dependencia en versiones específicas de los módulos.  Para más información, consulte [Solución de problemas de los módulos](shared-resources.md#modules).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Errores comunes al trabajar con la Configuración de estado deseado (DSC)

### <a name="unsupported-characters"></a>Escenario: No se puede eliminar una configuración con caracteres especiales del portal

#### <a name="issue"></a>Problema

Al intentar eliminar una configuración de DSC del portal, ve el siguiente error:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Causa

Este error es un problema temporal que está previsto que se resuelva.

#### <a name="resolution"></a>Resolución

* Use el cmdlet de Az "Remove-AzAutomationDscConfiguration" para eliminar la configuración.
* Todavía no se ha actualizado la documentación de este cmdlet.  Hasta entonces, consulte la documentación del módulo AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Escenario: No se pudo registrar el agente de DSC

#### <a name="issue"></a>Problema

Al intentar ejecutar `Set-DscLocalConfigurationManager` u otro cmdlet de DSC, recibe el error:

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

Este error se debe normalmente a un firewall, a la máquina que está detrás de un servidor proxy o a otros errores de red.

#### <a name="resolution"></a>Resolución

Compruebe que la máquina tenga acceso a los puntos de conexión adecuadas para DSC de Automatización de Azure y vuelva a intentarlo. Para ver una lista de puertos y direcciones necesarios, consulte [Planeamiento de red](../automation-dsc-overview.md#network-planning).

### <a name="failed-not-found"></a>Escenario: el nodo se encuentra en estado de error con el error "No encontrado"

#### <a name="issue"></a>Problema

El nodo tiene un informe con estado **erróneo** y contiene el error:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Causa

Este error suele ocurrir cuando se asigna al nodo un nombre de configuración (por ejemplo, ABC), en lugar de un nombre de configuración de nodo (por ejemplo, ABC.WebServer).

#### <a name="resolution"></a>Resolución

* Asegúrese de estar asignando al nodo un "nombre de configuración de nodo" y no el "nombre de configuración".
* Puede asignar una configuración de nodo a un nodo mediante el Portal de Azure o con un cmdlet de PowerShell.

  * Para asignar una configuración de nodo a un nodo mediante Azure Portal, abra la página **Nodos DSC**, seleccione un nodo y haga clic en el botón **Asignar configuración de nodo**.
  * Para asignar una configuración de nodo a un nodo mediante un cmdlet de PowerShell, use el cmdlet **Set-AzureRmAutomationDscNode**.

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

* Asegúrese de que la expresión junto a la palabra clave **Node** en la definición de configuración no se está evaluando como $null.
* Si se pasan datos de configuración al compilar la configuración, asegúrese de que se pasan los valores esperados que la configuración necesita de [ConfigurationData](../automation-dsc-compile.md).

### <a name="dsc-in-progress"></a>Escenario: el informe de nodo de DSC se queda bloqueado en el estado "en curso"

#### <a name="issue"></a>Problema

El agente DSC genera la salida:

```error
No instance found with given property values
```

#### <a name="cause"></a>Causa

Ha actualizado la versión de WMF y ha dañado WMI.

#### <a name="resolution"></a>Resolución

Para solucionar el problema, siga las instrucciones que se indican en el artículo [Problemas y limitaciones conocidos de DSC](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc).

### <a name="issue-using-credential"></a>Escenario: no se puede usar una credencial en una configuración de DSC

#### <a name="issue"></a>Problema

El trabajo de compilación de DSC se ha suspendido con el error:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Causa

Ha usado una credencial en una configuración pero no ha proporcionado el valor adecuado de **ConfigurationData** para establecer **PSDscAllowPlainTextPassword** en true para cada configuración de nodo.

#### <a name="resolution"></a>Resolución

* Asegúrese de pasar el valor adecuado de **ConfigurationData** para establecer **PSDAllowPlainTextPassword** como true para cada configuración de nodo mencionada en la configuración. Para más información, consulte los [recursos en DSC de Azure Automation](../automation-dsc-compile.md#working-with-assets-in-azure-automation-during-compilation).

### <a name="failure-processing-extension"></a>Escenario: Se produce un error de procesamiento de la extensión al realizar la incorporación desde la extensión DSC

#### <a name="issue"></a>Problema

Al realizar la incorporación mediante la extensión DSC, se produce el error:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Causa

Este error suele producirse cuando se asigna al nodo un nombre de configuración que no existe en el servicio.

#### <a name="resolution"></a>Resolución

* Asegúrese de que asigna al nodo un nombre de configuración que coincida exactamente con el nombre del servicio.
* Puede elegir no incluir el nombre de configuración del nodo, lo que dará lugar a la incorporación del nodo, pero no a la asignación de una configuración del nodo

### <a name="failure-linux-temp-noexec"></a>Escenario: Al aplicar una configuración en Linux, se produce un error general

#### <a name="issue"></a>Problema

Al aplicar una configuración en Linux, se produce el error:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Causa

Los clientes han identificado que si se establece la ubicación `/tmp` en `noexec`, la versión actual de DSC no podrá aplicar las configuraciones.

#### <a name="resolution"></a>Resolución

* Quite la opción `noexec` de la ubicación `/tmp`.

### <a name="compilation-node-name-overlap"></a>Escenario: los nombres de configuración de nodos que se superponen podrían provocar una versión incorrecta

#### <a name="issue"></a>Problema

Si se usa un único script de configuración para generar varias configuraciones de nodo y algunas de ellas tienen un nombre que es un subconjunto de otros, un problema en el servicio de compilación podría dar lugar a la asignación de la configuración incorrecta.  Este problema solo se produce cuando se usa un único script para generar configuraciones con datos de configuración por nodo y solo cuando la superposición de los nombres tiene lugar al principio de la cadena.

Por ejemplo, si se usa un único script de configuración para generar configuraciones basadas en los datos de nodo pasados como una tabla hash mediante cmdlets, y los datos del nodo incluyen un servidor llamado "server" y "1server".

#### <a name="cause"></a>Causa

Problema conocido con el servicio de compilación.

#### <a name="resolution"></a>Resolución

La mejor solución sería realizar la compilación localmente o en una canalización de CI/CD y cargar los archivos MOF directamente en el servicio.  Si es necesario que la compilación tenga lugar en el servicio, la siguiente mejor solución sería dividir los trabajos de compilación para que no haya nombres superpuestos.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
