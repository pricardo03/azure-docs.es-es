---
title: Información sobre los resultados de la comprobación del agente de Azure Update Management
description: Obtenga información acerca de la solución de problemas con el agente de Update Management.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 20323afe79ad3de1e3dfccd4752c4f7e28d22266
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095378"
---
# <a name="understand-the-agent-check-results-in-update-management"></a>Información sobre los resultados de la comprobación del agente de Update Management

Puede haber muchas razones por las que la máquina que no es de Azure no muestra el estado **Listo** en Update Management. En Update Management, puede comprobar el estado de un agente de Hybrid Worker para determinar el problema subyacente. En este artículo se explica cómo ejecutar el solucionador de problemas desde Azure Portal y en escenarios sin conexión.

## <a name="start-the-troubleshooter"></a>Iniciar el solucionador de problemas

Si hace clic en el vínculo **Solucionar problemas** en la columna **Preparación de actualizaciones del agente** del portal, abra la página **Solucionar problemas del Agente de actualización**. Esta página muestra los problemas con el agente y un vínculo a este artículo para ayudarle a solucionar sus problemas.

![Página de Lista de VM](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Las comprobaciones requieren que la máquina esté en ejecución. Si la máquina virtual no se encuentra en ejecución, aparece un botón para **Iniciar la máquina virtual**.

En la página **Solucionar problemas del Agente de actualización**, haga clic en **Ejecutar comprobaciones** para iniciar el solucionador de problemas. El solucionador de problemas usa el [Comando Ejecutar](../../virtual-machines/windows/run-command.md) para ejecutar un script en la máquina a fin de verificar las dependencias que el agente tiene. Una vez completado el solucionador de problemas, devuelve el resultado de las comprobaciones.

![Página de solución de problemas](../media/update-agent-issues/troubleshoot-page.png)

Una vez finalizada la operación, los resultados se devuelven en la ventana. En las [secciones de comprobación](#pre-requisistes-checks) se ofrece información sobre qué se busca en cada comprobación.

![Página de comprobaciones del agente de actualización](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Comprobaciones de requisitos previos

### <a name="operating-system"></a>Sistema operativo

La comprobación del sistema operativo comprueba si el Hybrid Runbook Worker ejecuta alguno de los siguientes sistemas operativos:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Solo admite evaluaciones de actualización.         |
|Windows Server 2008 R2 SP1 y posterior     |Se requiere .NET Framework 4.5.1 o cualquier versión posterior. ([Descargar .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Se requiere Windows PowerShell 4.0 o posterior. ([Descargar WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Se recomienda Windows PowerShell 5.1 para aumentar la confiabilidad.  ([Descargar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) y 7 (x64)      | Los agentes de Linux deben tener acceso a un repositorio de actualización. La aplicación de revisiones basada en la clasificación requiere "yum" para devolver los datos de seguridad que CentOS no tiene de forma nativa.         |
|Red Hat Enterprise (x86/x64) 6 y 7 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|SUSE Linux Enterprise Server 11 (x86/x64) y 12 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|Ubuntu 14.04 LTS, 16.04 LTS y 18.04 LTS (x86/x64)      |Los agentes de Linux deben tener acceso a un repositorio de actualización.         |

### <a name="net-451"></a>.NET 4.5.1

La comprobación de .NET Framework verifica si en el sistema existe la versión mínima de [.NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=30653).

### <a name="wmf-51"></a>WMF 5.1

La comprobación de WMF verifica si el sistema tiene la versión requerida de Windows Management Framework. [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) es la versión mínima que admite. Se recomienda que instale [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) para ofrecer mayor confiabilidad del Hybrid Runbook Worker.

### <a name="tls-12"></a>TLS 1.2

Esta comprobación determina si usa TLS 1.2 para cifrar las comunicaciones. TLS 1.0 ya no es compatible con la plataforma y se recomienda que los clientes usen TLS 1.2 para comunicarse con Update Management.

## <a name="connectivity-checks"></a>Comprobaciones de conectividad

### <a name="registration-endpoint"></a>Punto de conexión del registro

Esta comprobación determina si el agente puede comunicarse correctamente con el servicio del agente.

Las configuraciones de proxy y firewall deben permitir que el agente de Hybrid Runbook Worker se comunique con el punto de conexión de registro. Para obtener una lista de direcciones y puertos que deben abrirse, vea [Planeación de red para Hybrid Worker](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>Punto de conexión de las operaciones

Esta comprobación determina si el agente puede comunicarse correctamente con el servicio de datos en tiempo de ejecución del trabajo.

Las configuraciones de proxy y firewall deben permitir que el agente de Hybrid Runbook Worker se comunique con el servicio de datos en tiempo de ejecución del trabajo. Para obtener una lista de direcciones y puertos que deben abrirse, vea [Planeación de red para Hybrid Worker](../automation-hybrid-runbook-worker.md#network-planning)

## <a name="vm-service-health-checks"></a>Comprobaciones del estado de servicio de las máquinas virtuales

### <a name="monitoring-agent-service-status"></a>Supervisión del estado de servicio del agente

Esta comprobación determina si Microsoft Monitoring Agent `HealthService` se ejecuta en la máquina.

Para más información sobre cómo solucionar problemas del servicio, vea [No se está ejecutando Microsoft Monitoring Agent](hybrid-runbook-worker.md#mma-not-running).

Para volver a instalar Microsoft Monitoring Agent, vea [Instalación y configuración de Microsoft Monitoring Agent](/log-analytics/log-analytics-concept-hybrid.md#install-and-configure-agent)

### <a name="monitoring-agent-service-events"></a>Supervisión de eventos de servicio del agente

Esta comprobación determina si se ha producido algún evento `4502` en el registro de Operations Manager en la maquina durante las veinticuatro últimas horas.

Para más información sobre este evento, vea la [guía de solución de problemas](hybrid-runbook-worker.md#event-4502) para este evento.

## <a name="access-permissions-checks"></a>Comprobaciones de permisos de acceso

### <a name="machinekeys-folder-access"></a>Acceso a la carpeta MachineKeys

La comprobación de acceso a la carpeta criptográfica determina si la cuenta del sistema local tiene acceso a `C:\ProgramData\Microsoft\Crypto\RSA`

## <a name="troubleshoot-offline"></a>Solución de problemas sin conexión

Puede utilizar el solucionador de problemas sin conexión en un Hybrid Runbook Worker mediante la ejecución local del script. El script [Troubleshoot-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) puede encontrarse en la Galería de PowerShell. En el ejemplo siguiente se muestra un ejemplo del resultado de este script:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.1.7601 (Windows Server 2008 R2 SP1) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .Net Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             : 
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas adicionales con los Hybrid Runbook Workers, vea [Solución de problemas de Hybrid Runbook Worker](hybrid-runbook-worker.md).
