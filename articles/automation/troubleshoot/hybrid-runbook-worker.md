---
title: 'Solución de problemas: Hybrid Runbook Worker de Azure Automation'
description: En este artículo se proporciona información de solución de problemas de Hybrid Runbook Worker de Azure Automation.
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 02/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ea6599152d3cbf1f50132f5b207c19148401f798
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564266"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Solución de problemas de Hybrid Runbook Worker

En este artículo se proporciona información sobre cómo solucionar problemas con Hybrid Runbook Worker.

## <a name="general"></a>General

Hybrid Runbook Worker depende de un agente para comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. En el caso de Windows, este agente es Microsoft Monitoring Agent. En el caso de Linux, es el Agente de OMS para Linux.

### <a name="runbook-execution-fails"></a>Escenario: Error en la ejecución de un runbook

#### <a name="issue"></a>Problema

Se produce un error en la ejecución de un runbook y recibe el error siguiente:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

El runbook se suspende poco después de intentar ejecutarlo tres veces. Hay condiciones que pueden interrumpir la finalización del runbook. Cuando esto sucede, es posible que el mensaje de error relacionado no incluya información adicional que le indique el motivo.

#### <a name="cause"></a>Causa

Las siguientes son causas posibles:

* Los runbooks no se pueden autenticar con recursos locales.

* Hybrid Worker está detrás de un firewall o proxy.

* Los runbooks no se pueden autenticar con recursos locales.

* El equipo configurado para ejecutar la característica Hybrid Runbook Worker no cumple los requisitos mínimos de hardware.

#### <a name="resolution"></a>Resolución

Compruebe que el equipo tenga acceso saliente a *.azure-automation.net en el puerto 443.

Los equipos que ejecutan Hybrid Runbook Worker deben cumplir los requisitos mínimos de hardware antes de configurarlos para que hospeden esta característica. Los runbooks y los procesos en segundo plano que usan pueden provocar un uso excesivo del sistema, así como tiempos de espera y retrasos del trabajo de runbook.

Confirme que el equipo que ejecutará la característica Hybrid Runbook Worker cumple los requisitos mínimos de hardware. Si es así, supervise el uso de la CPU y memoria para determinar las posibles correlaciones entre el rendimiento de los procesos de Hybrid Runbook Worker y Windows. Si hay presión de CPU o memoria, esto puede indicar la necesidad de actualizar los recursos. También puede seleccionar un recurso de proceso diferente que pueda admitir los requisitos mínimos y escalarse cuando las demandas de carga de trabajo indiquen que es necesario un aumento.

Compruebe el registro de eventos **Microsoft SMA** para un evento correspondiente con la descripción *Proceso Win32 cerrado con el código [4294967295]*. La causa de este error es que no ha configurado la autenticación en sus Runbooks o especificado las credenciales de identificación para el grupo de Hybrid Worker. Revise los [permisos de runbook](../automation-hrw-run-runbooks.md#runbook-permissions) para confirmar que ha configurado correctamente la autenticación para los runbooks.

### <a name="no-cert-found"></a>Escenario: No se encontró ningún certificado en el almacén de certificados en Hybrid Runbook Worker.

#### <a name="issue"></a>Problema

Se produce un error en un runbook que se ejecutan en Hybrid Runbook Worker con el mensaje de error siguiente:

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Causa

Este error se produce cuando se intenta usar una [cuenta de ejecución](../manage-runas-account.md) en un runbook que se ejecuta en Hybrid Runbook Worker en el que el certificado de la cuenta de ejecución no está presente. Hybrid Runbook Worker no tiene el recurso de certificado localmente de manera predeterminada, el cual la cuenta de ejecución requiere para su correcto funcionamiento.

#### <a name="resolution"></a>Resolución

Si Hybrid Runbook Worker es una VM de Azure, puede usar [Identidades administradas para recursos de Azure](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) en su lugar. Este escenario le permite autenticar los recursos de Azure mediante la identidad administrada de la VM de Azure en lugar de la cuenta de ejecución, lo que simplifica la autenticación. Cuando Hybrid Runbook Worker es una máquina local, deberá instalar el certificado de cuenta de ejecución en la máquina. Para obtener información sobre cómo instalar el certificado, consulte los pasos necesarios para ejecutar el runbook [Export-RunAsCertificateToHybridWorker](../automation-hrw-run-runbooks.md#runas-script).

## <a name="linux"></a>Linux

Hybrid Runbook Worker de Linux depende del agente de OMS para Linux a fin de comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="oms-agent-not-running"></a>Escenario: El agente de OMS para Linux no se está ejecutando

#### <a name="issue"></a>Problema

El agente de OMS para Linux no se está ejecutando

#### <a name="cause"></a>Causa

Si el Agente de OMS para Linux no se está ejecutando, se impide que Hybrid Runbook Worker de Linux se comunique con Azure Automation. Son varios los motivos por los que el agente podría no estar ejecutándose.

#### <a name="resolution"></a>Resolución

 Escriba el siguiente comando para comprobar si el agente se está ejecutando: `ps -ef | grep python`. Debería ver un resultado similar al siguiente, los procesos de Python con la cuenta de usuario **nxautomation**. Si las soluciones Update Management o Azure Automation no están habilitadas, no se ejecuta ninguno de los procesos siguientes.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

En la siguiente lista, se muestran los procesos que se inician para un Hybrid Runbook Worker de Linux. Se encuentran en el directorio `/var/opt/microsoft/omsagent/state/automationworker/`.


* **oms.conf**: este valor es el proceso de administrador de trabajos. Se inicia directamente desde DSC.

* **worker.conf**: este es el proceso de Auto Registered Hybrid Worker, y lo inicia el administrador de trabajos. Este proceso lo usa Update Management y es transparente para el usuario. Este proceso no está presente si la solución Update Management no está habilitada en el equipo.

* **diy/worker.conf**: este es el proceso de DIY Hybrid Worker. El proceso de DIY Hybrid Worker se usa para ejecutar runbooks de usuario en Hybrid Runbook Worker. Difiere del proceso de Auto Registered Hybrid Worker solo en el detalle clave de que usa otra configuración. Este proceso no está presente si la solución Azure Automation está deshabilitada y el Hybrid Worker de Linux personal (DIY, Hágalo usted mismo) no está registrado.

Si el agente de OMS para Linux no se está ejecutando, ejecute el siguiente comando para iniciar el servicio: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Escenario: La clase especificada no existe

Si ve el error: **La clase especificada no existe...** en `/var/opt/microsoft/omsconfig/omsconfig.log`, el agente de OMS para Linux debe actualizarse. Ejecute el siguiente comando para volver a instalar el agente de OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a> Windows

Hybrid Runbook Worker depende de Microsoft Monitoring Agent para comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="mma-not-running"></a>Escenario: Microsoft Monitoring Agent no está en ejecución.

#### <a name="issue"></a>Problema

El servicio `healthservice` no se está ejecutando en el equipo de Hybrid Runbook Worker.

#### <a name="cause"></a>Causa

Si el servicio de Windows Microsoft Monitoring Agent no se está ejecutando, este estado impide que Hybrid Runbook Worker se comunique con Azure Automation.

#### <a name="resolution"></a>Resolución

Compruebe el agente se está ejecutando escribiendo el comando siguiente en PowerShell: `Get-Service healthservice`. Si se detiene el servicio, escriba el siguiente comando de PowerShell para iniciar el servicio: `Start-Service healthservice`.

### <a name="event-4502"></a>Evento 4502 del registro de Operations Manager

#### <a name="issue"></a>Problema

En el registro de eventos **Aplicaciones y servicios\Operations Manager**, vea el evento 4502 y el EventMessage que contiene **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** con la descripción siguiente: *El certificado presentado por el servicio \<wsid\>.oms.opinsights.azure.com no fue emitido por una entidad de certificación usada para los servicios de Microsoft. Póngase en contacto con el administrador de red para comprobar si están ejecutando un proxy que intercepta la comunicación TLS/SSL. El artículo KB3126513 incluye información adicional para la solución de problemas de conectividad.*

#### <a name="cause"></a>Causa

Este problema se puede deber a que el firewall de red o de proxy está bloqueando la comunicación con Microsoft Azure. Compruebe que el equipo tenga acceso saliente a *.azure-automation.net en los puertos 443.

#### <a name="resolution"></a>Resolución

Los registros se almacenan localmente en cada Hybrid Worker en C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Puede comprobar si hay algún evento de error o advertencia en el registro de eventos **Registros de aplicaciones y servicios\Microsoft-SMA\Operations** y **Registros de aplicaciones y servicios\Operations Manager** que podría indicar un problema de conectividad o de otro tipo que afecta a la incorporación del rol a Azure Automation o un problema bajo las operaciones normales.

[La salida y los mensajes de runbooks](../automation-runbook-output-and-messages.md) se envían a Azure Automation desde los trabajos híbridos igual que los trabajos de runbook que se ejecutan en la nube. También puede habilitar los flujos Detallado y Progreso de la misma manera que haría para otros runbooks.

### <a name="corrupt-cache"></a> Hybrid Runbook Worker sin informes

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

#### <a name="resolution"></a>Resolución

Para resolver este problema, inicie sesión en Hybrid Runbook Worker y ejecute el script siguiente. Este script detiene Microsoft Monitoring Agent, quita su caché y reinicia el servicio. Esta acción obliga a Hybrid Runbook Worker a volver a descargar su configuración de Azure Automation.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>Escenario: no puede agregar una instancia de Hybrid Runbook Worker.

#### <a name="issue"></a>Problema

Recibió el siguiente mensaje cuando intentaba agregar una instancia de Hybrid Runbook Worker mediante el cmdlet `Add-HybridRunbookWorker`.

```error
Machine is already registered
```

#### <a name="cause"></a>Causa

Esto puede surgir si la máquina ya está registrada con una cuenta de Automation diferente o si intenta volver a agregar la instancia de Hybrid Runbook Worker después de retirarla de una máquina.

#### <a name="resolution"></a>Resolución

Para resolver este problema, quite la siguiente clave del Registro, reinicie `HealthService` y pruebe el cmdlet `Add-HybridRunbookWorker` de nuevo:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.

