---
title: 'Solución de problemas: Hybrid Runbook Worker de Azure Automation'
description: En este artículo se proporciona información de solución de problemas de Hybrid Runbook Worker de Azure Automation.
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4d804499116631be6f922f67f8b8f6c7063a6d5c
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77030734"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Solución de problemas de Hybrid Runbook Worker

En este artículo se proporciona información sobre cómo solucionar problemas con Hybrid Runbook Worker.

## <a name="general"></a>General

Hybrid Runbook Worker depende de un agente para comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Para Windows, este es el agente de Log Analytics para Windows, también denominado Microsoft Monitoring Agent (MMA). Para Linux, es el agente de Log Analytics para Linux.

### <a name="runbook-execution-fails"></a>Escenario: Error en la ejecución de un runbook

#### <a name="issue"></a>Problema

Se produce un error en la ejecución del runbook y recibe el error siguiente.

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

El runbook se suspende poco después de intentar ejecutarse tres veces. Hay condiciones que pueden interrumpir la finalización del runbook. El mensaje de error relacionado podría no incluir ninguna información adicional.

#### <a name="cause"></a>Causa

Las posibles causas son las siguientes:

* Los runbooks no se pueden autenticar con recursos locales.

* Hybrid Worker está detrás de un firewall o proxy.

* El equipo configurado para ejecutar la característica Hybrid Runbook Worker no cumple los requisitos mínimos de hardware.

#### <a name="resolution"></a>Solución

Verifique que el equipo tenga acceso saliente a *.azure-automation.net en el puerto 443.

Los equipos que ejecutan Hybrid Runbook Worker deben cumplir los requisitos mínimos de hardware antes de que el trabajo esté configurado para hospedar esta característica. Los runbooks y el proceso en segundo plano que utilizan pueden provocar un uso excesivo del sistema, así como tiempos de espera y retrasos del trabajo de los runbooks.

Confirme que el equipo que ejecutará la característica Hybrid Runbook Worker cumple los requisitos mínimos de hardware. Si es así, supervise el uso de la CPU y memoria para determinar las posibles correlaciones entre el rendimiento de los procesos de Hybrid Runbook Worker y Windows. Cualquier presión de CPU o memoria puede indicar la necesidad de actualizar los recursos. También puede seleccionar otro recurso de proceso que pueda admitir los requisitos mínimos y escalarse cuando las demandas de carga de trabajo indiquen que es necesario un aumento.

Compruebe el registro de eventos **Microsoft SMA** para un evento correspondiente con la descripción *Proceso Win32 cerrado con el código [4294967295]* . La causa de este error es que no ha configurado la autenticación en sus runbooks o especificado las credenciales de ejecución para el grupo de Hybrid Worker. Revise los [permisos de runbooks](../automation-hrw-run-runbooks.md#runbook-permissions) para confirmar que ha configurado correctamente la autenticación para los runbooks.

### <a name="no-cert-found"></a>Escenario: No se encontró ningún certificado en el almacén de certificados en Hybrid Runbook Worker.

#### <a name="issue"></a>Problema

Se produce un error en un runbook que se ejecuta en una instancia de Hybrid Runbook Worker con el mensaje de error siguiente.

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```
#### <a name="cause"></a>Causa

Este error se produce cuando se intenta usar una [cuenta de ejecución](../manage-runas-account.md) en un runbook que se ejecuta en una instancia de Hybrid Runbook Worker en la que el certificado de la cuenta de ejecución no existe. De forma predeterminada, las instancias de Hybrid Runbook Worker no cuentan con el recurso de certificado en el entorno local, que resulta necesario para que la cuenta de ejecución funcione correctamente.

#### <a name="resolution"></a>Solución

Si su instancia de Hybrid Runbook Worker es una VM de Azure, puede usar [identidades administradas para recursos de Azure](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) en su lugar. Este escenario simplifica la autenticación, lo que le permite autenticar los recursos de Azure mediante la identidad administrada de la VM de Azure en lugar de la cuenta de ejecución. Cuando la instancia de Hybrid Runbook Worker es una máquina local, deberá instalar el certificado de cuenta de ejecución en la máquina. Para obtener información sobre cómo instalar el certificado, consulte los pasos necesarios para ejecutar el runbook de PowerShell Export-RunAsCertificateToHybridWorker en artículo [Ejecución de runbooks en Hybrid Runbook Worker](../automation-hrw-run-runbooks.md).

### <a name="error-403-on-registration"></a>Escenario: error 403 durante el registro de Hybrid Runbook Worker

#### <a name="issue"></a>Problema

Se produce un error en la fase inicial de registro del trabajo y recibe el siguiente error (403).

```error
"Forbidden: You don't have permission to access / on this server."
```

#### <a name="cause"></a>Causa

Las posibles causas son las siguientes:
* Hay un identificador de área de trabajo o una clave de área de trabajo (principal) mal escritos en la configuración del agente. 
* Hybrid Runbook Worker no puede descargar la configuración, lo que provoca un error de vinculación de la cuenta. Cuando Azure habilita las soluciones, solo en determinadas regiones permite vincular un área de trabajo de Log Analytics y una cuenta de Automation. También es posible que se haya establecido una fecha o una hora incorrectas en el equipo. Si la hora es +/-15 minutos de la hora actual, se produce un error en la incorporación.

#### <a name="resolution"></a>Solución

##### <a name="mistyped-workspace-idkey"></a>Identificador o clave de área de trabajo mal escritos
Para verificar si el identificador o la clave del área de trabajo del agente se han escrito incorrectamente, consulte [Adición o eliminación de un área de trabajo: agente de Windows](../../azure-monitor/platform/agent-manage.md#windows-agent) para el agente de Windows o bien [Adición o eliminación de un área de trabajo: agente de Linux](../../azure-monitor/platform/agent-manage.md#linux-agent) para el agente de Linux.  Asegúrese de seleccionar la cadena completa de Azure Portal y cópiela y péguela con cuidado.

##### <a name="configuration-not-downloaded"></a>Configuración no descargada

Su área de trabajo de Log Analytics y su cuenta de Automation deben estar en una región vinculada. Para obtener una lista de las regiones admitidas, consulte [Asignaciones de áreas de trabajo de Log Analytics y Azure Automation](../how-to/region-mappings.md).

También es posible que necesite actualizar la fecha y la zona horaria del equipo. Si selecciona un intervalo de tiempo personalizado, asegúrese de que esté en formato UTC, que puede diferir del de la zona horaria local.

## <a name="linux"></a>Linux

Hybrid Runbook Worker de Linux depende del [agente de Log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md) a fin de comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="oms-agent-not-running"></a>Escenario: el agente de Log Analytics para Linux no se está ejecutando

#### <a name="issue"></a>Problema

el agente de Log Analytics para Linux no se está ejecutando

#### <a name="cause"></a>Causa

Si el agente no se está ejecutando, se impide que Hybrid Runbook Worker de Linux se comunique con Azure Automation. Son varios los motivos por los que el agente podría no estar ejecutándose.

#### <a name="resolution"></a>Solución

 Escriba el siguiente comando para comprobar si el agente se está ejecutando: `ps -ef | grep python`. Debería ver un resultado similar al siguiente, los procesos de Python con la cuenta de usuario **nxautomation**. Si las soluciones Update Management o Azure Automation no están habilitadas, no se ejecuta ninguno de los procesos siguientes.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

En la siguiente lista, se muestran los procesos que se inician para un Hybrid Runbook Worker de Linux. Se encuentran en el directorio `/var/opt/microsoft/omsagent/state/automationworker/`.

* **oms.conf**: proceso del administrador de trabajos. Se inicia directamente desde DSC.

* **worker.conf**: proceso de Hybrid Worker registrado automáticamente. Lo inicia el administrador de trabajos. Este proceso lo usa Update Management y es transparente para el usuario. Este proceso no está presente si la solución Update Management no está habilitada en el equipo.

* **diy/worker.conf**: proceso de Hybrid Worker de implementación manual. El proceso de DIY Hybrid Worker se usa para ejecutar runbooks de usuario en Hybrid Runbook Worker. Difiere del proceso de Hybrid Worker registrado automáticamente solo en el detalle clave de que usa otra configuración. Este proceso no existe si la solución Azure Automation está deshabilitada y el rol de Hybrid Worker de Linux de implementación manual no está registrado.

Si el agente no se está ejecutando, ejecute el siguiente comando para iniciar el servicio: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Escenario: La clase especificada no existe

Si ve el error **La clase especificada no existe...** en `/var/opt/microsoft/omsconfig/omsconfig.log`, significa que el agente de Log Analytics para Linux debe actualizarse. Ejecute el siguiente comando para volver a instalar el agente:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Hybrid Runbook Worker de Windows depende del [agente de Log Analytics para Windows ](../../azure-monitor/platform/log-analytics-agent.md) a fin de comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="mma-not-running"></a>Escenario: Microsoft Monitoring Agent no está en ejecución.

#### <a name="issue"></a>Problema

El servicio `healthservice` no se está ejecutando en el equipo de Hybrid Runbook Worker.

#### <a name="cause"></a>Causa

Si el servicio Microsoft Monitoring Agent no se está ejecutando, este estado impide que Hybrid Runbook Worker se comunique con Azure Automation.

#### <a name="resolution"></a>Solución

Compruebe el agente se está ejecutando escribiendo el comando siguiente en PowerShell: `Get-Service healthservice`. Si se detiene el servicio, escriba el siguiente comando de PowerShell para iniciar el servicio: `Start-Service healthservice`.

### <a name="event-4502"></a>Escenario: Evento 4502 del registro de Operations Manager

#### <a name="issue"></a>Problema

En el registro de eventos **Aplicaciones y servicios\Operations Manager**, vea el evento 4502 y el elemento EventMessage que contiene **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** con la descripción siguiente: *El certificado presentado por el servicio \<wsid\>.oms.opinsights.azure.com no fue emitido por una entidad de certificación usada para los servicios de Microsoft. Póngase en contacto con el administrador de red para comprobar si está ejecutando un proxy que intercepta la comunicación TLS/SSL.*

#### <a name="cause"></a>Causa

Este problema se puede deber a que el firewall de red o de proxy está bloqueando la comunicación con Microsoft Azure. Compruebe que el equipo tenga acceso saliente a *.azure-automation.net en los puertos 443. 

#### <a name="resolution"></a>Solución

Los registros se almacenan localmente en cada Hybrid Worker en C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Puede verificar si hay algún evento de error o advertencia en los registros de eventos **Application and Services Logs\Microsoft-SMA\Operations** y **Application and Services Logs\Operations Manager** que indique un problema de conectividad o de otro tipo que afecte a la incorporación del rol a Azure Automation, o un problema durante las operaciones normales. Para obtener ayuda adicional para solucionar problemas relacionados con el agente de Log Analytics, consulte [Procedimientos para solucionar problemas relacionados con el agente de Log Analytics para Windows](../../azure-monitor/platform/agent-windows-troubleshoot.md).

[La salida y los mensajes de runbooks](../automation-runbook-output-and-messages.md) se envían a Azure Automation desde los trabajos híbridos igual que los trabajos de runbook que se ejecutan en la nube. También puede habilitar los flujos Detallado y Progreso de la misma manera que haría para otros runbooks.

### <a name="corrupt-cache"></a>Escenario: Hybrid Runbook Worker sin informes

#### <a name="issue"></a>Problema

El equipo de Hybrid Runbook Worker se está ejecutando, pero no ve los datos de latido para el equipo en el área de trabajo.

En la consulta de ejemplo siguiente se muestran los equipos en un área de trabajo y su último latido:

```loganalytics
// Last heartbeat of each computer
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Causa

Este problema se puede deber a una caché dañada en el Hybrid Runbook Worker.

#### <a name="resolution"></a>Solución

Para resolver este problema, inicie sesión en Hybrid Runbook Worker y ejecute el script siguiente. Este script detiene Microsoft Monitoring Agent, quita su caché y reinicia el servicio. Esta acción obliga a Hybrid Runbook Worker a volver a descargar su configuración de Azure Automation.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>Escenario: No puede agregar una instancia de Hybrid Runbook Worker.

#### <a name="issue"></a>Problema

Recibió el siguiente mensaje cuando intentaba agregar una instancia de Hybrid Runbook Worker mediante el cmdlet `Add-HybridRunbookWorker`.

```error
Machine is already registered
```

#### <a name="cause"></a>Causa

Este problema puede surgir si la máquina ya está registrada con una cuenta de Automation diferente o si intenta volver a agregar la instancia de Hybrid Runbook Worker después de quitarla de una máquina.

#### <a name="resolution"></a>Solución

Para resolver este problema, quite la siguiente clave del Registro, reinicie `HealthService` y pruebe el cmdlet `Add-HybridRunbookWorker` de nuevo:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.