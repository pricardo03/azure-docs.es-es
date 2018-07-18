---
title: Solución de problemas con Desired State Configuration (DSC) de Azure Automation
description: En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5d2eae67fcff74a7016f7f6125e31a9c8c2bda97
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064698"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Solución de problemas de Desired State Configuration (DSC)

En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Errores comunes al trabajar con la Configuración de estado deseado (DSC)

### <a name="failed-not-found"></a>Escenario: el nodo se encuentra en estado de error con el error "No encontrado"

#### <a name="issue"></a>Problema

El nodo tiene un informe con estado **erróneo** y contiene el error:

```
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Causa

Este error suele ocurrir cuando se asigna al nodo un nombre de configuración (por ejemplo, ABC), en lugar de un nombre de configuración de nodo (por ejemplo, ABC.WebServer).

#### <a name="resolution"></a>Resolución

* Asegúrese de estar asignando al nodo un "nombre de configuración de nodo" y no el "nombre de configuración".
* Puede asignar una configuración de nodo a un nodo mediante el Portal de Azure o con un cmdlet de PowerShell.

  * Para asignar una configuración de nodo a un nodo mediante Azure Portal, abra la página **Nodos DSC**, seleccione un nodo y haga clic en el botón **Asignar configuración de nodo**.  
  * Para asignar una configuración de nodo a un nodo mediante un cmdlet de PowerShell, use el cmdlet **AzureRmAutomationDscNode Set** .

### <a name="no-mof-files"></a>Escenario: no se produjeron configuraciones de nodo (archivos MOF) al compilarse una configuración

#### <a name="issue"></a>Problema

Su trabajo de compilación de DSC suspende con el error:

```
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Causa

Cuando la expresión que aparece junto a la palabra clave **Node** en la configuración de DSC se evalúa como `$null`, no se produce ninguna configuración de nodo.

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:

* Asegúrese de que la expresión junto a la palabra clave **Node** en la definición de configuración no se está evaluando como $null.
* Si se pasan datos de configuración al compilar la configuración, asegúrese de que se pasan los valores esperados que la configuración necesita de [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Escenario: el informe de nodo de DSC se queda bloqueado en el estado "en curso"

#### <a name="issue"></a>Problema

El agente DSC genera la salida:

```
No instance found with given property values
```

#### <a name="cause"></a>Causa

Ha actualizado la versión de WMF y ha dañado WMI.

#### <a name="resolution"></a>Resolución

Para solucionar el problema, siga las instrucciones que se indican en la entrada del blog [Problemas y limitaciones conocidos de DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc).

### <a name="issue-using-credential"></a>Escenario: no se puede usar una credencial en una configuración de DSC

#### <a name="issue"></a>Problema

El trabajo de compilación de DSC se ha suspendido con el error:

```
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Causa

Ha usado una credencial en la configuración pero no ha proporcionado el valor adecuado de **ConfigurationData** para establecer **PSDscAllowPlainTextPassword** en true para cada configuración de nodo.

#### <a name="resolution"></a>Resolución

* Asegúrese de que pasa el valor adecuado de **ConfigurationData** para establecer **PSAllowPlainTextPassword** como true para cada configuración de nodo mencionada en la configuración. Para más información, consulte los [recursos en DSC de Azure Automation](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.