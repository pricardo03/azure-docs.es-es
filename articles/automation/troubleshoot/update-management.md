---
title: Solucionar problemas relativos a errores con Update Management
description: Obtenga información acerca de la solución de problemas relacionados con Update Management.
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 884ded67c25aca78225baef2d7e4c5de1cc94fd0
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782286"
---
# <a name="troubleshooting-issues-with-update-management"></a>Solución de problemas relacionados con Update Management

En este artículo se describen soluciones para resolver problemas que pueden surgir al usar Update Management.

Existe un solucionador de problemas de agente para que el agente de Hybrid Worker determine el problema subyacente. Para más información sobre el solucionador de problemas, consulte el artículo sobre [cómo solucionar problemas con el agente de actualización](update-agent-issues.md). Para todos los demás problemas, consulte la información detallada que aparece a continuación sobre posibles problemas.

## <a name="general"></a>General

### <a name="components-enabled-not-working"></a>Escenario: Se han habilitado los componentes de la solución "Update Management" y ahora se está configurando esta máquina virtual

#### <a name="issue"></a>Problema

Continúa recibiendo el mensaje siguiente en una máquina virtual 15 minutos después de la incorporación:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Causa

Este error puede deberse a las siguientes razones:

1. Está bloqueada la comunicación a la cuenta de Automation.
2. La máquina virtual que se incorpora puede provenir de una máquina clonada que no estaba preparada con sysprep con Microsoft Monitoring Agent instalado.

#### <a name="resolution"></a>Resolución

1. Visite [Planeamiento de red](../automation-hybrid-runbook-worker.md#network-planning) para obtener información acerca de qué direcciones y puertos deben permitirse para que Update Management funcione.
2. Si usa una imagen clonada:
   1. En el área de trabajo de Log Analytics, quite la máquina virtual de la búsqueda guardada en la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates` si se muestra. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.
   2. Ejecute `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force`
   3. Ejecute `Restart-Service HealthService` para reiniciar `HealthService`. Se volverá a crear la clave y se generará un nuevo UUID.
   4. Si esta solución no funciona, primero prepare la imagen con sysprep y, después, instale el agente MMA.

### <a name="multi-tenant"></a>Escenario: Recibe un error de la suscripción vinculada al crear una implementación de actualización para las máquinas en otro inquilino de Azure.

#### <a name="issue"></a>Problema

Recibe el error siguiente al intentar crear una implementación de actualización para las máquinas en otro inquilino de Azure:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

#### <a name="cause"></a>Causa

Este error se produce cuando se crea una implementación de actualización que tiene máquinas virtuales de Azure en otro inquilino incluidas en una implementación de actualización.

#### <a name="resolution"></a>Resolución

Tendrá que usar la solución alternativa siguiente para programarlas. Puede usar el cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) con el modificador `-ForUpdate` para crear una programación y usar el cmdlet [New AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) y pasar las máquinas del otro inquilino al parámetro `-NonAzureComputer`. En el ejemplo siguiente se muestra cómo hacerlo:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

### <a name="nologs"></a>Escenario: Las máquinas no se muestran en el portal en Update Management

#### <a name="issue"></a>Problema

Pueden aparecer los siguientes escenarios:

* La máquina muestra **No configurada** en la vista de Update Management de una máquina virtual.

* Faltan las máquinas en la vista de Update Management de la cuenta de Automation.

* Tiene máquinas que se muestran con el estado **No evaluada** en **Cumplimiento**, pero ve datos de latido en los registros de Azure Monitor correspondientes a Hybrid Runbook Worker, pero no a Update Management.

#### <a name="cause"></a>Causa

Esta situación puede deberse a posibles problemas de configuración local o a que la configuración de ámbito no está realizada correctamente.

Es posible que sea necesario volver a registrar e instalar Hybrid Runbook Worker.

Puede que haya definido una cuota en el área de trabajo que se ha alcanzado y ha impedido que se almacenen los datos.

#### <a name="resolution"></a>Resolución

* Asegúrese de que las notificaciones de la máquina se envían al área de trabajo correcta. Compruebe cuál es el área de trabajo al que la máquina envía notificaciones. Para saber cómo hacerlo, consulte [Comprobación de la conectividad del agente a Log Analytics](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). A continuación, asegúrese de que esta sea el área de trabajo que está vinculada a su cuenta de Azure Automation. Para ello, vaya a la cuenta de Automation y haga clic en **Linked workspace** (Área de trabajo vinculada) en **Related Resources** (Recursos relacionados).

* Compruebe que las máquinas se muestran en el área de trabajo de Log Analytics. Ejecute la siguiente consulta en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation. Si no ve la máquina en los resultados de consulta, es que no está latente, lo que significa que probablemente haya un problema de configuración local. Puede ejecutar el solucionador de problemas para [Windows](update-agent-issues.md#troubleshoot-offline) o [Linux](update-agent-issues-linux.md#troubleshoot-offline), según el sistema operativo, o puede [volver a instalar el agente](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Si la máquina se muestra en los resultados de consulta, será preciso comprobar la configuración de ámbito especificada en la siguiente viñeta.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Compruebe si hay problemas de configuración de ámbito. La [configuración de ámbito](../automation-onboard-solutions-from-automation-account.md#scope-configuration) determina qué máquinas se configuran para la solución. Si la máquina aparece en el área de trabajo pero no se muestra, tendrá que realizar la configuración de ámbito para dirigirse a ella. Para información sobre cómo hacerlo, consulte [Incorporación de máquinas en el área de trabajo](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Si los pasos anteriores no resuelven el problema, siga los pasos que se describen en [Implementación de Hybrid Runbook Worker en Windows](../automation-windows-hrw-install.md) para volver a instalar Hybrid Worker para Windows o los de [Implementación de Hybrid Runbook Worker en Linux](../automation-linux-hrw-install.md) para Linux.

* En el área de trabajo, ejecute la siguiente consulta. Si ve el resultado `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota`, tiene una cuota definida en el área de trabajo que se ha alcanzado y ha impedido que se guarden los datos. En el área de trabajo, vaya a **Uso y costos estimados** > **administración del volumen de datos** y compruebe su cuota o elimine la cuota que tiene.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

## <a name="windows"></a>Windows

Si se producen problemas al intentar incorporar la solución en una máquina virtual, compruebe el registro de eventos de **Operations Manager** en **Registros de aplicaciones y servicios** en la máquina local en busca de eventos con el identificador de evento **4502** y el mensaje de evento que contenga **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

En la sección siguiente se destacan los mensajes de error específicos y una posible solución para cada uno. Para otros problemas sobre la incorporación, consulte el artículo sobre la [solución de problemas de incorporación de la solución](onboarding.md).

### <a name="machine-already-registered"></a>Escenario: La máquina ya está registrada en otra cuenta

#### <a name="issue"></a>Problema

Aparece el siguiente mensaje de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Causa

La máquina ya está incorporada a otra área de trabajo para Update Management.

#### <a name="resolution"></a>Resolución

Realice una limpieza de los artefactos antiguos en la máquina mediante la [eliminación del grupo Hybrid Runbook](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) y vuelva a intentarlo.

### <a name="machine-unable-to-communicate"></a>Escenario: La máquina no se puede comunicar con el servicio

#### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Causa

Puede haber un servidor proxy, una puerta de enlace o un firewall bloqueando la comunicación de red.

#### <a name="resolution"></a>Resolución

Revise la red y asegúrese de que se permiten las direcciones y los puertos correspondientes. Consulte los [requisitos de red](../automation-hybrid-runbook-worker.md#network-planning), para obtener una lista de puertos y direcciones que necesitan Update Management y las instancias de Hybrid Runbook Worker.

### <a name="unable-to-create-selfsigned-cert"></a>Escenario: Error al crear el certificado autofirmado

#### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Causa

Hybrid Runbook Worker no pudo generar un certificado autofirmado

#### <a name="resolution"></a>Resolución

Verifique que la cuenta del sistema tiene acceso de lectura a la carpeta **C:\ProgramData\Microsoft\Crypto\RSA** e inténtelo de nuevo.

### <a name="failed-to-start"></a>Escenario: Una máquina muestra No se pudo iniciar en una implementación de actualizaciones

#### <a name="issue"></a>Problema

Una máquina tiene el estado **No se pudo iniciar**. Al ver los detalles específicos de la máquina, observará el siguiente error:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

#### <a name="cause"></a>Causa

Este error puede ocurrir principalmente por los siguientes motivos:

* La máquina ya no existe.
* La máquina está desactivada y no se posible establecer comunicación con ella.
* La máquina tiene un problema de conectividad de red y no se puede acceder a la instancia de Hybrid Worker de la máquina.
* Ha habido una actualización de Microsoft Monitoring Agent que ha cambiado el valor de SourceComputerId.
* La ejecución de actualizaciones puede haber limitado si se alcanza el tope de 2000 trabajos simultáneos en una cuenta de Automation. Cada implementación se considera un trabajo y cada máquina de una implementación de actualizaciones se cuenta como un trabajo. Cualquier otro trabajo de automatización o implementación de actualizaciones actualmente en ejecución en la cuenta de Automation cuenta para el límite de trabajos simultáneos.

#### <a name="resolution"></a>Resolución

Cuando proceda, use [grupos dinámicos](../automation-update-management.md#using-dynamic-groups) para las implementaciones de actualizaciones.

* Compruebe que la máquina aún exista y sea accesible. Si no existe, edite la implementación y quite la máquina.
* Consulte la sección sobre el [planeamiento de red](../automation-update-management.md#ports) para obtener una lista de puertos y direcciones que son necesarios para Update Management y asegúrese de que la máquina cumple estos requisitos.
* Ejecute la siguiente consulta en Log Analytics para encontrar máquinas en el entorno cuyo valor de `SourceComputerId` haya cambiado. Busque los equipos que tienen el mismo valor de `Computer`, pero distinto valor de `SourceComputerId`. Una vez que encuentre las máquinas afectadas, debe editar las implementaciones de actualizaciones que se dirijan a esas máquinas, y quitar y volver a agregar las máquinas para que `SourceComputerId` refleje el valor correcto.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

### <a name="hresult"></a>Escenario: La máquina aparece como No evaluado y se muestra una excepción HResult

#### <a name="issue"></a>Problema

Tiene máquinas que aparecen como **No evaluado** en **Cumplimiento**, y verá un mensaje de excepción debajo de él.

#### <a name="cause"></a>Causa

Windows Update o WSUS no están configurados correctamente en la máquina. Update Management se basa en Windows Update o WSUS para proporcionar las actualizaciones necesarias, el estado de la revisión y los resultados de las revisiones implementadas. Sin esta información, Update Management no puede informar correctamente de las revisiones que son necesarias o que están instaladas.

#### <a name="resolution"></a>Resolución

Haga doble clic en la excepción que se muestra en rojo para ver el mensaje de excepción completo. Revise la siguiente tabla para ver posibles soluciones o acciones que deben realizarse:

|Excepción  |Acción o resolución  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Busque el código de error correspondiente en [Lista de códigos de error de Windows Update](https://support.microsoft.com/help/938205/windows-update-error-code-list) para buscar detalles adicionales sobre la causa de la excepción.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Estos errores son problemas de conectividad de red. Asegúrese de que la máquina tenga la conectividad de red adecuada para Update Management. Consulte la sección sobre [planeamiento de red](../automation-update-management.md#ports) para obtener una lista de puertos y direcciones que se requieren.        |
|`0x8024001E`| No se completó la operación de actualización porque se estaban cerrando el servicio o el sistema.|
|`0x8024002E`| El servicio de Windows Update está deshabilitado.|
|`0x8024402C`     | Si usa un servidor WSUS, asegúrese de que los valores de registro de `WUServer` y `WUStatusServer` bajo la clave del registro `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` tienen el servidor WSUS correcto.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Asegúrese de que el servicio Windows Update (wuauserv) se está ejecutando y no está deshabilitado.        |
|Cualquier otra excepción genérica     | Realice una búsqueda en Internet para ver las soluciones posibles y colabore con el equipo de soporte técnico de TI local.         |

Puede que también le ayude revisar el archivo `windowsupdate.log` para intentar determinar la causa posible. Para más información sobre cómo leer el registro, consulte [Cómo leer el archivo Windowsupdate.log](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Además, puede descargar y ejecutar el [solucionador de problemas de Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) para comprobar si hay algún problema con Windows Update en el equipo.

> [!NOTE]
> El [Solucionador de problemas de Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indica que es para clientes de Windows, pero también funciona en Windows Server.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Escenario: No se puede iniciar la ejecución de actualización

#### <a name="issue"></a>Problema

No se pudo iniciar una ejecución de actualización en una máquina Linux.

#### <a name="cause"></a>Causa

La instancia de Hybrid Worker de Linux no es correcta.

#### <a name="resolution"></a>Resolución

Realice una copia del archivo de registro siguiente y consérvelo para la solución de problemas:

```bash
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Escenario: La ejecución de actualización se inicia, pero encuentra errores

#### <a name="issue"></a>Problema

Se inicia una ejecución de la actualización, pero encuentra errores durante la ejecución.

#### <a name="cause"></a>Causa

Las posibles causas pueden ser:

* El estado del administrador de paquetes no es correcto
* Paquetes específicos pueden interferir con la aplicación de revisiones basada en la nube
* Otros motivos

#### <a name="resolution"></a>Resolución

Si se producen errores durante una ejecución de actualizaciones después de que se ha iniciado correctamente en Linux, compruebe el trabajo de salida desde la máquina afectada en la ejecución. Puede encontrar mensajes de error específicos procedentes del Administrador de paquetes de su máquina que puede investigar e intentar solucionar. Update Management requiere que el administrador de paquetes tenga un estado correcto para que las implementaciones de actualizaciones se realicen con éxito.

En algunos casos, las actualizaciones de paquetes pueden interferir con Update Management e impedir que finalice una implementación de actualizaciones. Si ese fuera el caso, deberá excluir estos paquetes de las futuras ejecuciones de actualizaciones o instalarlos manualmente por su cuenta.

Si no puede resolver un problema de aplicación de revisiones, realice una copia del archivo de registro siguiente y consérvela **antes** de que se inicie la siguiente implementación de actualizaciones para solucionar los problemas:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

### <a name="other"></a>Escenario: Mi problema no se ha indicado anteriormente

### <a name="issue"></a>Problema

Tiene un problema que no se resuelve en los otros escenarios que aparecen en la lista.

### <a name="cause"></a>Causa

Las claves del Registro faltantes o configuradas de manera incorrecta pueden provocar problemas con Update Management.

### <a name="resolution"></a>Resolución

Elimine la clave del Registro `HKLM:\SOFTWARE\Microsoft\HybridRunbookWorker` y reinicie **HealthService**.

También puede usar estos comandos de PowerShell.

```powershell
Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
Restart-Service healthservice
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
