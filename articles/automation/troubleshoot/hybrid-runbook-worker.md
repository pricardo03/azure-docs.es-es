---
title: 'Solución de problemas: Hybrid Runbook Worker de Azure Automation'
description: En este artículo se proporciona información de solución de problemas de Hybrid Runbook Worker de Azure Automation.
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6c8dc240172451118fd75b042ba267740999882d
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321774"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Solución de problemas de Hybrid Runbook Worker

En este artículo se proporciona información sobre cómo solucionar problemas con Hybrid Runbook Worker.

## <a name="general"></a>General

Hybrid Runbook Worker depende de un agente para comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. En el caso de Windows, este agente es Microsoft Monitoring Agent. En el caso de Linux, es el Agente de OMS para Linux.

### <a name="runbook-execution-fails"></a>Escenario: Error en la ejecución de un runbook

#### <a name="issue"></a>Problema

Se produce un error en la ejecución de un runbook y recibe el error siguiente:

```
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

El Runbook se suspende poco después de intentar ejecutarlo tres veces. Hay condiciones que pueden interrumpir el Runbook antes de su correcta finalización y el mensaje de error relacionado no incluye información adicional que indique el motivo.

#### <a name="cause"></a>Causa

Las siguientes son causas posibles:

* Los runbooks no se pueden autenticar con recursos locales.

* Hybrid Worker está detrás de un firewall o proxy.

* Los runbooks no se pueden autenticar con recursos locales.

* El equipo designado para ejecutar la característica Hybrid Runbook Worker cumple los requisitos mínimos de hardware.

#### <a name="resolution"></a>Resolución

Compruebe que el equipo tenga acceso saliente a *.azure-automation.net en el puerto 443.

Los equipos que ejecutan Hybrid Runbook Worker deben cumplir los requisitos mínimos de hardware antes de indicar que hospede esta característica. De lo contrario, en función del uso de recursos de otros procesos en segundo plano y la contención provocada por runbooks durante la ejecución, el equipo estará sobreutilizado y provocará retrasos o tiempos de espera en los trabajos de runbook.

Confirme que el equipo designado para ejecutar la característica Hybrid Runbook Worker cumple los requisitos mínimos de hardware. Si es así, supervise el uso de la CPU y de memoria para determinar las posibles correlaciones entre el rendimiento de los procesos de Hybrid Runbook Worker y Windows. Si hay presión de memoria o de la CPU, esto puede indicar que se deben actualizar o agregar procesadores adicionales, o aumentar la memoria para resolver el cuello de botella de recursos y el error. También puede seleccionar un recurso de proceso diferente que pueda admitir los requisitos mínimos y escalarse cuando las demandas de trabajo indiquen que es necesario un aumento.

Compruebe el registro de eventos **Microsoft SMA** para un evento correspondiente con la descripción *Proceso Win32 cerrado con el código [4294967295]*. La causa de este error es que no ha configurado la autenticación en sus Runbooks o especificado las credenciales de identificación para el grupo de Hybrid Worker. Revise los [permisos de runbook](../automation-hrw-run-runbooks.md#runbook-permissions) para confirmar que ha configurado correctamente la autenticación para los runbooks.

## <a name="linux"></a>Linux

Hybrid Runbook Worker de Linux depende del agente de OMS para Linux a fin de comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="oms-agent-not-running"></a>Escenario: El Agente de OMS para Linux no se está ejecutando

Si el agente de OMS para Linux no se está ejecutando, esto impide que Hybrid Runbook Worker de Linux se comunique con Azure Automation. Escriba el siguiente comando para comprobar si el agente se está ejecutando: `ps -ef | grep python`. Debería ver un resultado similar al siguiente, los procesos de Python con la cuenta de usuario **nxautomation**. Si no están habilitadas las soluciones Update Management o Azure Automation, no se ejecutará ninguno de los siguientes procesos.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

En la siguiente lista, se muestran los procesos que se inician para un Hybrid Runbook Worker de Linux. Se encuentran en el directorio `/var/opt/microsoft/omsagent/state/automationworker/`.

* **oms.conf**: este es el proceso de administrador de trabajos, este se inicia directamente desde la DSC.

* **worker.conf**: este es el proceso de Auto Registered Hybrid Worker, lo inicia el administrador de trabajos. Este proceso lo usa Update Management y es transparente para el usuario. Este proceso no debe estar presente si la solución Update Management no está habilitada en la máquina.

* **diy/worker.conf**: este es el proceso de DIY Hybrid Worker. El proceso de DIY Hybrid Worker se usa para ejecutar runbooks de usuario en Hybrid Runbook Worker. Difiere del proceso de Auto Registered Hybrid Worker solo en el detalle clave de que usa otra configuración. Este proceso no está presente si la solución Azure Automation no está habilitada y el Hybrid Worker de Linux personal (DIY, Hágalo usted mismo) no está registrado.

Si el agente de OMS para Linux no se está ejecutando, ejecute el siguiente comando para iniciar el servicio: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Escenario: La clase especificada no existe

Si ve el error **La clase especificada no existe...** en `/var/opt/microsoft/omsconfig/omsconfig.log`, el agente de OMS para Linux debe actualizarse. Ejecute el siguiente comando para volver a instalar el agente de OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Hybrid Runbook Worker depende de Microsoft Monitoring Agent para comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="mma-not-running"></a>Escenario: No se está ejecutando Microsoft Monitoring Agent

#### <a name="issue"></a>Problema

El servicio `healthservice` no se está ejecutando en la máquina de Hybrid Runbook Worker.

#### <a name="cause"></a>Causa

El servicio de Windows Microsoft Monitoring Agent no se está ejecutando, lo que impide que Hybrid Runbook Worker se comunique con Azure Automation.

#### <a name="resolution"></a>Resolución

Compruebe el agente se está ejecutando escribiendo el comando siguiente en PowerShell: `Get-Service healthservice`. Si se detiene el servicio, escriba el siguiente comando de PowerShell para iniciar el servicio: `Start-Service healthservice`.

### <a name="event-4502"></a>Evento 4502 del registro de Operations Manager

#### <a name="issue"></a>Problema

En el registro de eventos **Registros de aplicaciones y servicios\Operations Manager**, puede ver el evento 4502 y un mensaje de evento que contiene **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**, donde se indica que *el certificado presentado por el servicio \<wsid\>.oms.opinsights.azure.com no fue emitido por una entidad de certificación usada para los servicios de Microsoft. Póngase en contacto con el administrador de red para comprobar si están ejecutando un proxy que intercepta la comunicación TLS/SSL. El artículo KB3126513 incluye información adicional para la solución de problemas de conectividad.*

#### <a name="cause"></a>Causa

Esto puede deberse a que el firewall de red o de proxy está bloqueando la comunicación con Microsoft Azure. Compruebe que el equipo tenga acceso saliente a *.azure-automation.net en los puertos 443.

#### <a name="resolution"></a>Resolución

Los registros se almacenan localmente en cada Hybrid Worker en C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Puede comprobar si hay algún evento de error o advertencia en el registro de eventos **Registros de aplicaciones y servicios\Microsoft-SMA\Operations** y **Registros de aplicaciones y servicios\Operations Manager** que podría indicar que un problema de conectividad u otro problema afecta a la incorporación del rol a Azure Automation o un problema al realizar las operaciones normales.

[La salida y los mensajes de runbooks](../automation-runbook-output-and-messages.md) se envían a Azure Automation desde los trabajos híbridos igual que los trabajos de runbook que se ejecutan en la nube. También puede habilitar los flujos Detallado y Progreso de la misma manera que haría para otros runbooks.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
