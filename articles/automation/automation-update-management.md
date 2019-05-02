---
title: Solución Update Management de Azure
description: Este artículo está pensado para ayudarle a entender cómo usar la solución Azure Update Management para administrar las actualizaciones de los equipos Windows y Linux.
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/29/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b39d9788372fb0f682bc1e5b737542b400dd4035
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919702"
---
# <a name="update-management-solution-in-azure"></a>Solución Update Management de Azure

Puede usar la solución Update Management en Azure Automation para administrar las actualizaciones de sistema operativo de los equipos de Windows y Linux en Azure, en entornos locales o en otros proveedores de nube. Puede evaluar rápidamente el estado de las actualizaciones disponibles en todos los equipos agente y administrar el proceso de instalación de las actualizaciones necesarias para los servidores.

Update Management se puede habilitar en máquinas virtuales directamente desde una cuenta de Azure Automation. Para aprender a habilitar Update Management en máquinas virtuales desde una cuenta de Automation, consulte [Administración de actualizaciones para varias máquinas virtuales](manage-update-multi.md). También puede habilitar Update Management para una máquina virtual desde la página de la máquina virtual en Azure Portal. Este escenario está disponible para máquinas virtuales [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) y [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Información general de la solución

Los equipos administrados por Update Management usan las siguientes configuraciones para evaluar e implementar actualizaciones:

* Microsoft Monitoring Agent (MMA) para Windows o Linux
* Extensión DSC (configuración de estado deseado) de PowerShell para Linux
* Hybrid Runbook Worker de Automation
* Microsoft Update o Windows Server Update Services (WSUS) para equipos Windows

El siguiente diagrama muestra una vista conceptual del comportamiento y un flujo de datos con el modo en que la solución evalúa y aplica las actualizaciones de seguridad a todos los equipos Windows Server y Linux conectados en un área de trabajo:

![Flujo del proceso de Update Management](./media/automation-update-management/update-mgmt-updateworkflow.png)

Se puede utilizar Update Management para incorporar de forma nativa máquinas en varias suscripciones en el mismo inquilino.

Una vez que se publique un CVE, la revisión tarda de 2 a 3 horas en aparecer en las máquinas Linux para su evaluación.  Para las máquinas Windows, la revisión tarda de 12 a 15 horas en aparecer para su evaluación tras su publicación.

Después de un equipo realiza un examen de cumplimiento de las actualizaciones, el agente reenvía la información de forma masiva a los registros de Azure Monitor. En un equipo Windows, el examen de cumplimiento se ejecuta cada 12 horas de forma predeterminada.

Además del examen programado, el examen de cumplimiento de las actualizaciones se inicia a los 15 minutos del reinicio de MMA, así como antes y después de la instalación de actualizaciones.

En un equipo Linux, el examen de cumplimiento se realiza cada 3 horas de forma predeterminada. Si se reinicia el agente MMA, se inicia un examen de cumplimiento al cabo de 15 minutos.

La solución informa del grado de actualización del equipo en función del origen configurado para la sincronización. Si el equipo Windows está configurado para informar a WSUS, en función de cuándo WSUS se sincronizó por última vez con Microsoft Update, los resultados pueden diferir de lo que se muestra en Microsoft Updates. Este comportamiento es el mismo para los equipos Linux que están configurados para informar a un repositorio local en lugar de a uno público.

> [!NOTE]
> Para informar correctamente al servicio, Update Management requiere la habilitación de ciertas direcciones URL y puertos. Para más información sobre estos requisitos, consulte [Planeamiento de red para Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Puede implementar e instalar las actualizaciones de software en equipos que requieren las actualizaciones mediante la creación de una implementación programada. Las actualizaciones clasificadas como *Opcional* no se incluyen en el ámbito de implementación en equipos Windows. Solo se incluyen las actualizaciones necesarias.

La implementación programada define qué equipos de destino reciben las actualizaciones aplicables, ya sea explícitamente especificando equipos o seleccionando un [grupo de equipos](../azure-monitor/platform/computer-groups.md) que se basa en las búsquedas de registros de un conjunto específico de equipos, o un [Consulta azure](#azure-machines) que selecciona de forma dinámica las máquinas virtuales de Azure según los criterios especificados. Estos grupos son diferentes de [configuración de ámbito](../azure-monitor/insights/solution-targeting.md), que solo se usa para determinar qué máquinas obtención los módulos de administración que habilitan la solución. 

También se especifica una programación para aprobar y establecer un período de tiempo durante el que se pueden instalar actualizaciones. Este período de tiempo se llama a la ventana de mantenimiento. Diez minutos de la ventana de mantenimiento está reservado para reinicios si es necesario reiniciar el equipo y ha seleccionado la opción de reiniciar el equipo adecuado. Si la aplicación de revisiones tarda más de lo esperado y hay menos de diez minutos en la ventana de mantenimiento, no se producirá un reinicio.

Los Runbooks instalan las actualizaciones en Azure Automation. No puede ver estos runbooks, y los runbooks no requieren ninguna configuración. Cuando se crea una implementación de actualizaciones, esta crea una programación que inicia un runbook de actualización maestro a la hora especificada para los equipos incluidos. El runbook maestro inicia un runbook secundario en cada agente para instalar las actualizaciones necesarias.

En la fecha y hora especificadas en la implementación de actualizaciones, los equipos de destino ejecutan la implementación en paralelo. Antes de la instalación, se ejecuta un examen para comprobar que las actualizaciones siguen siendo necesarias. En los equipos cliente WSUS, si no se aprueban las actualizaciones en WSUS, se produce un error en la implementación de actualizaciones.

No se admite tener un equipo registrado para la administración de actualizaciones en varias áreas de trabajo de Log Analytics (hospedaje múltiple).

## <a name="clients"></a>Clientes

### <a name="supported-client-types"></a>Tipos de cliente admitidos

En la tabla siguiente se muestra una lista de sistemas operativos compatibles:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Solo admite evaluaciones de actualización.         |
|Windows Server 2008 R2 SP1 y versiones posteriores (incluye Windows Server 2012 y 2016)    |Se requiere .NET Framework 4.5.1 o cualquier versión posterior. ([Descargar .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Se requiere Windows PowerShell 4.0 o posterior. ([Descargar WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Se recomienda Windows PowerShell 5.1 para aumentar la confiabilidad.  ([Descargar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) y 7 (x64)      | Los agentes de Linux deben tener acceso a un repositorio de actualización. La aplicación de revisiones basada en la clasificación requiere "yum" para devolver los datos de seguridad que CentOS no tiene de forma nativa. Para obtener más información sobre en función de clasificación de aplicación de revisiones en CentOS, consulte [actualizar clasificaciones en Linux](#linux-2)          |
|Red Hat Enterprise (x86/x64) 6 y 7 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|SUSE Linux Enterprise Server 11 (x86/x64) y 12 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|Ubuntu 14.04 LTS, 16.04 LTS y 18.04 (x86/x64)      |Los agentes de Linux deben tener acceso a un repositorio de actualización.         |

> [!NOTE]
> Conjuntos de escalado de máquina virtual de Azure se pueden administrar con administración de actualizaciones. Administración de actualizaciones funciona en las instancias de sí mismos y no la imagen base. Deberá programar las actualizaciones de forma incremental, como para no actualizar todas las instancias de máquina virtual a la vez.

### <a name="unsupported-client-types"></a>Tipos de cliente no admitidos

En la tabla siguiente se enumeran los sistemas operativos no admitidos:

|Sistema operativo  |Notas  |
|---------|---------|
|Cliente Windows     | No se admiten sistemas operativos cliente (por ejemplo, Windows 7 y Windows 10).        |
|Windows Server 2016 Nano Server     | No compatible.       |

### <a name="client-requirements"></a>Requisitos del cliente

#### <a name="windows"></a> Windows

Los agentes de Windows deben estar configurados para comunicarse con un servidor de WSUS o tener acceso a Microsoft Update. Puede usar Update Management con System Center Configuration Manager. Para más información sobre escenarios de integración, consulte [Integración de System Center Configuration Manager con Update Management](oms-solution-updatemgmt-sccmintegration.md#configuration). El [agente de Windows](../azure-monitor/platform/agent-windows.md) es necesario. Este agente se instala automáticamente si va a incorporar una máquina virtual de Azure.

#### <a name="linux"></a>Linux

En Linux, la máquina debe tener acceso a un repositorio de actualización. El repositorio de actualización puede ser público o privado. Se requiere TLS 1.1 o TLS 1.2 para interactuar con Update Management. Esta solución no es compatible con un agente de Log Analytics para Linux que esté configurado para informar a varias áreas de trabajo de Log Analytics.

Para obtener información acerca de cómo instalar el agente de Log Analytics para Linux y descargar la versión más reciente, consulte [agente de Log Analytics para Linux](https://github.com/microsoft/oms-agent-for-linux). Para obtener información sobre cómo instalar el agente de Log Analytics para Windows, vea [supervisión de Microsoft del agente para Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Permisos

Para crear y administrar implementaciones de actualizaciones, necesita permisos concretos. Para más información sobre estos permisos, consulte [Acceso basado en rol: Update Management](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Componentes de soluciones

La solución consta de los siguientes recursos. Los recursos se agregan a la cuenta de Automation. Estos son agentes directamente conectados o están en un grupo de administración conectado por Operations Manager.

### <a name="hybrid-worker-groups"></a>Grupos de Hybrid Worker

Después de habilitar esta solución, los equipos Windows conectados directamente al área de trabajo de Log Analytics se configurarán automáticamente como Hybrid Runbook Worker para admitir los runbooks que se incluyen en esta solución.

Cada equipo Windows administrado por la solución se muestra en el panel **Grupos de Hybrid Worker** como un **grupo Hybrid Worker del sistema** para la cuenta de Automation. Las soluciones usan la convención de nomenclatura *FQDN_GUID de nombre de host*. No puede usar estos grupos como destino con runbooks de su cuenta. Si lo hace, generarán un error. Estos grupos están diseñados únicamente para admitir la solución de administración.

Puede agregar equipos Windows a un grupo de Hybrid Runbook Worker en la cuenta de Automation para admitir runbooks de Automation si usa la misma cuenta para la solución y la pertenencia a grupos de Hybrid Runbook Worker. Esta funcionalidad se agregó en la versión 7.2.12024.0 de Hybrid Runbook Worker.

### <a name="management-packs"></a>Módulos de administración

Si el grupo de administración de System Center Operations Manager está conectado a un área de trabajo de Log Analytics, se instalarán los siguientes módulos de administración en Operations Manager. Estos módulos de administración también se instalan en equipos Windows directamente conectados después de agregar esta solución. No es necesario configurar ni administrar dichos módulos.

* Intelligence Pack Update Assessment de Microsoft System Center Advisor (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Módulo de administración de Update Deployment

> [!NOTE]
> Si tiene un grupo de administración de Operations Manager 1807 con agentes configurados en el nivel de grupo de administración que se asociará a un área de trabajo, la solución actual para conseguir que se muestran es reemplazar **IsAutoRegistrationEnabled** a **True** en el **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init** regla.

Para obtener más información acerca de cómo se actualizan los paquetes de soluciones de administración, consulte [conectar Operations Manager a Azure Monitor registra](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Para los sistemas con Operations Manager Agent, se debe actualizar el agente a Microsoft Monitoring Agent para que Update Management pueda administrarlo por completo. Para aprender a actualizar el agente, consulte [Actualización de Operations Manager Agent](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). Para entornos con Operations Manager, se requiere que se está ejecutando System Center Operations Manager 2012 R2 UR 14 o posterior.

## <a name="onboard"></a>Habilitar Update Management

Para empezar a aplicar revisiones a sistemas, debe habilitar la solución Update Management. Hay muchas maneras de incorporar máquinas a Update Management. Las siguientes son las recomendadas y admiten maneras de incorporar la solución:

* [Desde una máquina virtual](automation-onboard-solutions-from-vm.md)
* [Navegando desde diferentes máquinas](automation-onboard-solutions-from-browse.md)
* [Desde la cuenta de Automation](automation-onboard-solutions-from-automation-account.md)
* [Con un runbook de Azure Automation](automation-onboard-solutions.md)
  
### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Confirmación de que las máquinas que no son de Azure están incorporadas

Para confirmar que las máquinas conectadas directamente se comunican con los registros de Azure Monitor, después de unos minutos, puede ejecutar uno las búsquedas de registros siguientes.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a> Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

En un equipo Windows, puede revisar la información siguiente para comprobar la conectividad del agente con los registros de Azure Monitor:

1. En el Panel de Control, abra **Microsoft Monitoring Agent**. En la pestaña **Azure Log Analytics**, el agente muestra el mensaje siguiente: **Microsoft Monitoring Agent se ha conectado correctamente a Log Analytics.**
2. Abra el registro de eventos de Windows. Vaya a **Application and Services Logs\Operations Manager** y busque los identificadores de evento 3000 y 5002 desde el **Conector de servicio** de origen. Estos eventos indican que el equipo se ha registrado con el área de trabajo de Log Analytics y está recibiendo la configuración.

Si el agente no puede comunicarse con los registros de Azure Monitor y el agente está configurado para comunicarse con internet a través de un servidor proxy o firewall, confirme el firewall o servidor proxy está configurado correctamente. Para aprender a comprobar que un firewall o un servidor proxy estén correctamente configurados, consulte [Configuración de red para el agente Windows](../azure-monitor/platform/agent-windows.md) o [Configuración de red para el agente de Linux](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Si los sistemas Linux están configurados para comunicarse con un servidor proxy o una puerta de enlace de Log Analytics y va a incorporar esta solución, actualice los permisos *proxy.conf* para conceder al grupo omiuser permiso de lectura sobre este archivo mediante la ejecución de los siguientes comandos:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Los agentes de Linux recién agregados muestran un estado **Actualizado** después de haber realizado una evaluación. Este proceso puede tardar hasta 6 horas.

Para confirmar que un grupo de administración de Operations Manager se comunica con los registros de Azure Monitor, consulte [validar Operations Manager integration con registros de Azure Monitor](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

## <a name="data-collection"></a>Colección de datos

### <a name="supported-agents"></a>Agentes admitidos

En la tabla siguiente se describen los orígenes conectados que son compatibles con esta solución:

| Origen conectado | Compatible | DESCRIPCIÓN |
| --- | --- | --- |
| Agentes de Windows |Sí |La solución recopila información sobre las actualizaciones del sistema de los agentes de Windows e inicia a continuación la instalación de las actualizaciones necesarias. |
| Agentes de Linux |Sí |La solución recopila información sobre las actualizaciones del sistema de los agentes de Linux e inicia a continuación la instalación de las actualizaciones necesarias en las distribuciones admitidas. |
| Grupo de administración de Operations Manager |Sí |La solución recopila información acerca de las actualizaciones del sistema de agentes en un grupo de administración conectado.<br/>Una conexión directa desde el agente de Operations Manager a los registros de Azure Monitor no es necesaria. Los datos se reenvían desde el grupo de administración al área de trabajo de Log Analytics. |

### <a name="collection-frequency"></a>Frecuencia de recopilación

Para cada equipo Windows administrado, se realiza un examen dos veces al día. Cada 15 minutos, se llama a la API de Windows para consultar la hora de la última actualización y determinar si ha cambiado el estado. Si ha cambiado el estado, se inicia un examen de cumplimiento.

Para cada equipo Linux administrado, se realiza un examen cada 3 horas.

Puede que transcurran entre 30 minutos y 6 horas antes de que se muestren los datos actualizados de los equipos administrados.

El promedio de uso de datos de los registros de Azure Monitor para una máquina mediante la administración de actualizaciones es de aproximadamente 25MB al mes. Este valor es solo una aproximación y está sujeto a cambios en función de su entorno. Se recomienda que supervise el entorno para ver el uso exacto que tiene.

## <a name="viewing-update-assessments"></a>Visualización de la evaluación de actualización

En la cuenta de Automation, seleccione **Update Management** para ver el estado de sus máquinas.

Esta vista proporciona información sobre las máquinas, las actualizaciones que faltan, las implementaciones de actualizaciones y las implementaciones de actualizaciones programadas. En la columna **Cumplimiento**, puede ver la última vez que se evaluó la máquina. En la columna **Preparación de actualizaciones del agente**, puede ver el estado del agente de actualización. Si hay un problema, seleccione el vínculo para dirigirse a la documentación de solución de problemas, que le ayudará a conocer los pasos que debe realizar para corregirlo.

Para ejecutar una búsqueda de registros que devuelva información sobre la máquina, las actualizaciones o las implementaciones, seleccione el elemento de la lista. Se abre el panel **Búsqueda de registros** con una consulta para el elemento seleccionado:

![Vista predeterminada de Update Management](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Instalación de actualizaciones

Una vez que se han evaluado las actualizaciones de todos los equipos Linux y Windows del área de trabajo, puede instalar las actualizaciones necesarias mediante la creación de una *implementación de actualizaciones*. Para crear una implementación de actualización, debe tener acceso de escritura a la cuenta de Automation y acceso de escritura a cualquier VM de Azure que tienen como destino de la implementación. Una implementación de actualizaciones es una instalación programada de actualizaciones necesarias en uno o más equipos. Se especifica la fecha y la hora de la implementación y el equipo o el grupo de equipos que se incluirán en el ámbito de esta. Para obtener más información acerca de los grupos de equipos, vea [grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md).

Al incluir grupos de equipos en la implementación de actualizaciones, la pertenencia al grupo se evalúa solo en el momento de la creación de la programación. Los cambios posteriores en un grupo no se reflejan. Para evitar este uso [grupos dinámicos](#using-dynamic-groups), estos grupos se resuelven en tiempo de implementación y se definen mediante una consulta para máquinas virtuales de Azure o una búsqueda guardada para máquinas virtuales que no son de Azure.

> [!NOTE]
> Las máquinas virtuales Windows implementadas desde Azure Marketplace se establecen de forma predeterminada para recibir actualizaciones automáticas del servicio Windows Update. Este comportamiento no cambia cuando se agrega esta solución o se agregan máquinas virtuales Windows al área de trabajo. Si no administra activamente las actualizaciones mediante esta solución, se aplica el comportamiento predeterminado (las actualizaciones se aplican automáticamente).

Para evitar que las actualizaciones se apliquen fuera de una ventana de mantenimiento en Ubuntu, vuelva a configurar el paquete de actualizaciones desatendidas para deshabilitar las actualizaciones automáticas. Para más información sobre cómo configurar el paquete, consulte el tema [Actualizaciones automáticas](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) en la Guía de Ubuntu Server.

Las máquinas virtuales que se crearon a partir de las imágenes a petición de Red Hat Enterprise Linux (RHEL) que están disponibles en Azure Marketplace están registradas para acceder a la instancia de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) implementada en Azure. Cualquier otra distribución de Linux se debe actualizar desde el repositorio de archivos en línea de la distribución en cuestión según los métodos admitidos por esta.

Para crear una nueva implementación de actualizaciones, seleccione **Programar implementación de actualizaciones**. El **nueva implementación de actualización** abre la página. Escriba valores para las propiedades descritas en la tabla siguiente y haga clic en **Crear**:

| Propiedad | Descripción |
| --- | --- |
| NOMBRE |Nombre único para identificar la implementación de actualizaciones. |
|Sistema operativo| Linux o Windows|
| Para actualizar los grupos |Para máquinas de Azure, defina una consulta basada en una combinación de suscripción, grupos de recursos, ubicaciones y etiquetas para crear un grupo dinámico de máquinas virtuales de Azure para incluir en la implementación. </br></br>Para las máquinas que no son de Azure, seleccione una búsqueda para seleccionar un grupo de máquinas que no son de Azure debe incluir en la implementación existente. </br></br>Para más información, consulte los [grupos dinámicos](automation-update-management.md#using-dynamic-groups).|
| Máquinas para actualizar |Seleccione una búsqueda guardada, un grupo importado o elija la máquina en la lista desplegable y seleccione equipos individuales. Si elige **Máquinas**, la preparación de la máquina se muestra en la columna **PREPARACIÓN DE ACTUALIZACIONES DEL AGENTE**.</br> Para información sobre los distintos métodos de creación de grupos de equipos en los registros de Azure Monitor, consulte el artículo sobre los [Grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md) |
|Clasificaciones de actualizaciones|Seleccione todas las clasificaciones de actualizaciones que necesite|
|Incluir o excluir actualizaciones|Se abrirá la página **Incluir/Excluir**. Las actualizaciones que se incluirán o excluirán están en pestañas independientes. Para más información sobre cómo se controla la inclusión, consulte la sección [Comportamiento de inclusión](automation-update-management.md#inclusion-behavior). |
|Configuración de programación|Seleccione la hora de inicio y seleccione Una vez o de manera periódica para la periodicidad|
| Scripts previos + scripts posteriores|Seleccione los scripts que se ejecutarán antes y después de la implementación.|
| Ventana de mantenimiento |Número de minutos establecido para las actualizaciones. El valor no puede ser inferior a 30 minutos ni más de 6 horas |
| Control de reinicio| Determina cómo se deben controlar los reinicios. Las opciones disponibles son la siguientes:</br>Reboot if required (Default) [Reiniciar si es necesario (predeterminada)]</br>Always reboot (Reiniciar siempre)</br>Never reboot (No reiniciar nunca)</br>Only reboot (solo reiniciar), no se instalarán las actualizaciones|

Las implementaciones de actualizaciones también se pueden crear mediante programación. Para información sobre cómo crear una implementación de actualizaciones con la API de REST, consulte [Software Update Configurations - Create](/rest/api/automation/softwareupdateconfigurations/create) (Configuraciones de actualización de software: Creación). También hay un runbook de ejemplo que puede usarse para crear una implementación de actualizaciones semanal. Para más información acerca de este runbook, consulte [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Crear una implementación de actualización semanal para una o más VM en un grupo de recursos).

### <a name="multi-tenant"></a>Implementaciones de actualizaciones entre inquilinos

Si tiene máquinas en otro inquilino de Azure que informen a Update Management de que necesita una revisión, tendrá que usar la solución alternativa siguiente para programarlas. Puede usar el cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) con el modificador `-ForUpdate` para crear una programación y usar el cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) y pasar las máquinas del otro inquilino al parámetro `-NonAzureComputer`. En el ejemplo siguiente se muestra cómo hacerlo:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="view-missing-updates"></a>Visualización de actualizaciones que faltan

Haga clic en **Actualizaciones que faltan** para ver la lista de actualizaciones que faltan en las máquinas. Se muestran todas las actualizaciones, y se pueden seleccionar. Aparece información sobre el número de máquinas que requieren la actualización, el sistema operativo y un vínculo para más información. El panel **Búsqueda de registros** muestra más información sobre las actualizaciones.

## <a name="view-update-deployments"></a>Visualización de implementaciones de actualizaciones

Seleccione la pestaña **Implementaciones de actualizaciones** para ver la lista de implementaciones de actualizaciones existentes. Seleccione cualquiera de las implementaciones de actualizaciones de la tabla para abrir el panel **Actualizar la ejecución de implementación** para esa implementación de actualizaciones.

![Información general de los resultados de la implementación de actualizaciones](./media/automation-update-management/update-deployment-run.png)

Para ver una implementación de actualizaciones de la API de REST, consulte [Software Update Configuration Runs](/rest/api/automation/softwareupdateconfigurationruns) (Ejecuciones de configuración de actualización de software).

## <a name="update-classifications"></a>Clasificaciones de actualizaciones

En las siguientes tablas se muestran las clasificaciones de actualizaciones en Update Management, junto con una definición de cada clasificación.

### <a name="windows"></a> Windows

|clasificación  |DESCRIPCIÓN  |
|---------|---------|
|Actualizaciones críticas     | Actualización para un problema específico que resuelve un error crítico no relacionado con la seguridad.        |
|Actualizaciones de seguridad     | Actualización para un problema específico del producto relacionado con la seguridad.        |
|Paquetes acumulativos de actualizaciones     | Conjunto acumulativo de revisiones que se empaquetan para facilitar la implementación.        |
|Feature Packs     | Nuevas características del producto que se distribuyen fuera de una versión del producto.        |
|Service Packs     | Un conjunto acumulativo de revisiones que se aplican a una aplicación.        |
|Actualizaciones de definiciones     | Una actualización de archivos de definiciones de virus o de otra índole.        |
|Herramientas     | Utilidad o característica que ayuda a realizar una o más tareas.        |
|Actualizaciones     | Actualización de una aplicación o archivo que están instalados actualmente.        |

### <a name="linux"></a>Linux

|clasificación  |DESCRIPCIÓN  |
|---------|---------|
|Actualizaciones críticas y de seguridad     | Actualizaciones para un problema específico o un problema de un producto específico relacionado con la seguridad.         |
|Otras actualizaciones     | Todas las demás actualizaciones que ni son críticas por naturaleza ni son actualizaciones de seguridad.        |

En Linux, Update Management puede distinguir entre actualizaciones críticas y de seguridad en la nube y mostrar al mismo tiempo datos de evaluación debido al enriquecimiento de datos en la nube. Para aplicar revisiones, Update Management se basa en los datos de clasificación disponibles en la máquina. A diferencia de otras distribuciones, CentOS no dispone de esta información de forma nativa. Si tiene máquinas de CentOS configuradas para devolver datos de seguridad para el siguiente comando, Update Management podrá aplicar revisiones basadas en clasificaciones.

```bash
sudo yum -q --security check-update
```

Actualmente no hay ningún método compatible para habilitar la disponibilidad de datos de clasificación nativos en CentOS. En este momento, solo se proporciona soporte técnico a clientes que pueden haber habilitado esto por su cuenta.

## <a name="firstparty-predownload"></a>Configuración avanzada

Update Management se basa en Windows Update para descargar e instalar las actualizaciones de Windows. Como resultado, se respetan muchas de las configuraciones usadas por Windows Update. Si usa la configuración para habilitar las actualizaciones que no son de Windows, Update Management también administrará dichas actualizaciones. Si desea habilitar la descarga de actualizaciones antes de que se lleve a cabo la implementación de la actualización, las implementaciones de las actualizaciones pueden ejecutarse más rápido con menos probabilidades de exceder la ventana de mantenimiento.

### <a name="pre-download-updates"></a>Actualizaciones de descarga previa

Para configurar la descarga automática de actualizaciones en la directiva de grupo, puede establecer la opción [Configurar actualizaciones automáticas](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#BKMK_comp5) en **3**. De esta forma se descargan las actualizaciones necesarias en segundo plano, pero no se instalan. Esto permite que Update Management controle las programaciones, pero permite que las actualizaciones se descarguen fuera de la ventana de mantenimiento de Update Management. Esto puede evitar errores **Ventana de mantenimiento superada** en Update Management.

También puede configurar esto con PowerShell, con la ejecución del siguiente comando de PowerShell en un sistema en el que desee descargar las actualizaciones automáticamente.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

### <a name="disable-automatic-installation"></a>Deshabilitar la instalación automática

Máquinas virtuales de Azure tienen la instalación automática de actualizaciones habilitado de forma predeterminada. Esto puede provocar que las actualizaciones se instalen antes de programar que puedan instalarse mediante la administración de actualizaciones. Puede deshabilitar este comportamiento estableciendo el `NoAutoUpdate` clave del registro para `1`. El siguiente fragmento de PowerShell muestra una manera de hacerlo.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

### <a name="enable-updates-for-other-microsoft-products"></a>Habilitar actualizaciones de otros productos de Microsoft

De forma predeterminada, Windows Update solo proporciona actualizaciones para Windows. Si habilita **ofrecer actualizaciones de otros productos de Microsoft al actualizar Windows**, se le ofrece actualizaciones para otros productos, incluidas las revisiones de seguridad para SQL Server u otro software de terceros primero. Esta opción no se puede configurar mediante la directiva de grupo. Ejecute el siguiente comando de PowerShell en los sistemas en los que desea habilitar otras revisiones propias, y Update Management respetará esta configuración.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="third-party"></a> Revisiones de aplicaciones de terceros en Windows

Administración de actualizaciones se basa en el repositorio de actualización configuradas localmente para aplicar revisiones a sistemas de Windows compatibles. Se trata de WSUS o Windows Update. Herramientas como [System Center Updates Publisher](/sccm/sum/tools/updates-publisher
) (Updates Publisher) le permiten publicar actualizaciones personalizadas en WSUS. Este escenario permite la administración de actualización a las máquinas de revisiones que usan System Center Configuration Manager como su repositorio de actualizaciones de software de terceros. Para obtener información sobre cómo configurar Updates Publisher, consulte [Instalar Updates Publisher](/sccm/sum/tools/install-updates-publisher).

## <a name="ports"></a>Planeamiento de red

Las direcciones siguientes se requieren específicamente para Update Management. La comunicación con estas direcciones se realiza a través del puerto 443.

|Azure Public  |Azure Government  |
|---------|---------|
|* .ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|* .oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|* .blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|* .azure-automation.net|*.azure-automation.us|

Para más información sobre los puertos que necesita Hybrid Runbook Worker, consulte los [puertos de roles de Hybrid Worker](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Se recomienda utilizar las direcciones mostradas al definir las excepciones. Puede descargar los [intervalos de direcciones IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Este archivo se actualiza semanalmente y refleja los intervalos implementados en ese momento y los próximos cambios en los intervalos de direcciones.

## <a name="search-logs"></a>Búsqueda de registros

Además de los detalles que se proporcionan en Azure Portal, se pueden realizar búsquedas en los registros. En la página de la solución, seleccione **Log Analytics**. Se abre el panel **Búsqueda de registros**.

También puede aprender a personalizar las consultas o a usarlas desde diferentes clientes, entre otras cosas visitando:  [Documentación de Log Analytics Search API](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Consultas de ejemplo

En las secciones siguientes se proporcionan ejemplos de consultas de registros para los registros de actualizaciones recopilados por esta solución:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Consultas individuales de evaluación de Azure VM (Windows)

Reemplace el valor de VMUUID con el GUID de la máquina virtual en la que va a realizar la consulta. Puede encontrar el VMUUID que debe usarse al ejecutar la consulta siguiente en los registros de Azure Monitor: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Falta resumen de actualizaciones

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="missing-updates-list"></a>Falta lista de actualizaciones

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Consultas individuales de evaluación de Azure VM (Linux)

Para algunas distribuciones de Linux, hay un [endian](https://en.wikipedia.org/wiki/Endianness) no coincide con el valor VMUUID que procede de Azure Resource Manager y lo que se almacena en los registros de Azure Monitor. La siguiente consulta busca una concordancia en cualquier codificación Endiannes. Reemplace los valores de VMUUID por el formato de big-endian y little-endian del GUID para devolver correctamente los resultados. Puede encontrar el VMUUID que debe usarse al ejecutar la consulta siguiente en los registros de Azure Monitor: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Falta resumen de actualizaciones

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Falta lista de actualizaciones

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Consultas de evaluación de máquinas virtuales múltiples

##### <a name="computers-summary"></a>Resumen de equipos

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Falta resumen de actualizaciones

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="computers-list"></a>Lista de equipos

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Falta lista de actualizaciones

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="using-dynamic-groups"></a>Uso de grupos dinámicos

Administración de actualizaciones proporciona la capacidad para tener como destino un grupo dinámico de Azure o máquinas virtuales que no son de Azure para implementaciones de actualizaciones. Estos grupos se evalúan en tiempo de implementación, por lo que no debe modificar la implementación para agregar máquinas.

> [!NOTE]
> Debe tener los permisos adecuados al crear una implementación de actualizaciones. Para obtener más información, consulte [instalar actualizaciones](#install-updates).

### <a name="azure-machines"></a>Máquinas de Azure

Estos grupos se definen mediante una consulta, cuando se inicia una implementación de actualizaciones, se evalúan los miembros de ese grupo. Grupos dinámicos no funcionan con máquinas virtuales clásicas. Al definir la consulta, se pueden usar los siguientes elementos juntos para rellenar el grupo dinámico

* Subscription
* Grupos de recursos
* Ubicaciones
* Etiquetas

![Selección de grupos](./media/automation-update-management/select-groups.png)

Para obtener una vista previa de los resultados de un grupo dinámico, haga clic en el botón **Vista previa**. Esta vista previa muestra la pertenencia al grupo en ese momento; en este ejemplo, estamos buscando máquinas con la etiqueta **Role** es igual a **BackendServer**. Si se ha agregado esta etiqueta a varias máquinas, se agregarán a las implementaciones futuras en ese grupo.

![vista previa de grupos](./media/automation-update-management/preview-groups.png)

### <a name="non-azure-machines"></a>Máquinas que no son de Azure

Para que no son de Azure las máquinas, las búsquedas guardadas también conocen como grupos de equipos se utilizan para crear el grupo dinámico. Para obtener información sobre cómo crear una búsqueda guardada, consulte [crear un grupo de equipos](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Una vez creado el grupo puede seleccionarla en la lista de búsquedas guardadas. Haga clic en **Preview** para obtener una vista previa de los equipos de la búsqueda guardada en ese momento.

![Selección de grupos](./media/automation-update-management/select-groups-2.png)

## <a name="integrate-with-system-center-configuration-manager"></a>Integración con System Center Configuration Manager

Los clientes que han invertido en System Center Configuration Manager para administrar equipos, servidores y dispositivos móviles también dependen de la fuerza y la madurez de Configuration Manager para ayudarles a administrar actualizaciones de software. Configuration Manager es parte de su ciclo de administración de actualizaciones de software (SUM).

Para aprender a integrar la solución de administración con System Center Configuration Manager, consulte [Integración de System Center Configuration Manager con Update Management](oms-solution-updatemgmt-sccmintegration.md).

## <a name="inclusion-behavior"></a>Comportamiento de inclusión

Inclusión de actualizaciones le permite especificar las actualizaciones específicas que quiere aplicar. Se instalan las revisiones o los paquetes que se incluyen. Cuando se incluyen revisiones o paquetes y, además, se selecciona una clasificación, se instalan tanto los elementos incluidos como los elementos que cumplen la clasificación.

Es importante saber que las exclusiones invalidan las inclusiones. Por ejemplo, si define una regla de exclusión de `*`, no se instalan revisiones ni paquetes, ya que se excluyen todas. Las revisiones excluidas aparecen todavía como que faltan en la máquina. Para las máquinas Linux, si se incluye un paquete, pero tiene un paquete dependiente que se ha excluido, el paquete no se instala.

## <a name="patch-linux-machines"></a>Aplicación de revisiones en máquinas Linux

Las siguientes secciones explican posibles problemas con la aplicación de revisiones de Linux.

### <a name="unexpected-os-level-upgrades"></a>Actualizaciones de nivel de sistema operativo inesperadas

En algunas variantes de Linux, como Red Hat Enterprise Linux, las actualizaciones de nivel de sistema operativo pueden producirse mediante paquetes. Esto puede llevar a ejecuciones de Update Management en las que cambia el número de versión del sistema operativo. Puesto que Update Management usa los mismos métodos para actualizar paquetes que un administrador utilizaría localmente en el equipo Linux, este comportamiento es deliberado.

Para evitar la actualización de la versión del sistema operativo con las ejecuciones de Update Management, use la característica **Exclusión**.

En Red Hat Enterprise Linux, el nombre del paquete para excluir sería: redhat-release-server.x86_64.

![Paquetes que se van a excluir para Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>No se aplican las revisiones críticas y de seguridad

Al implementar actualizaciones en una máquina Linux, puede seleccionar clasificaciones de actualizaciones. Esto filtra las actualizaciones que se aplican a la máquina que cumple los criterios especificados. Este filtro se aplica localmente en el equipo cuando se implementa la actualización.

Dado que Update Management realiza enriquecimiento de actualizaciones en la nube, algunas actualizaciones pueden marcarse en Update Management como impacto para la seguridad, aunque el equipo local no tiene esa información. Como resultado, si aplica actualizaciones críticas a una máquina Linux, puede haber actualizaciones que no estén marcadas como de impacto para la seguridad en esa máquina y no se apliquen.

Sin embargo, es posible que Update Management todavía notifique que esa máquina no es compatible porque tiene información adicional acerca de la actualización pertinente.

La implementación de actualizaciones mediante la clasificación de actualizaciones no funciona en CentOS de forma nativa. Para implementar correctamente actualizaciones para CentOS, seleccione todas las clasificaciones para asegurarse de que se aplican las actualizaciones. En SUSE, al seleccionar *solo* "Otras actualizaciones" como clasificación, puede dar lugar a que se instalen también algunas actualizaciones de seguridad si primero se requieren actualizaciones de seguridad relacionadas con zypper (administrador de paquetes) o sus dependencias. Este comportamiento es una limitación de zypper. En algunos casos, puede que se le pida volver a ejecutar la implementación de actualizaciones. Para ello, compruebe el registro de actualizaciones.

## <a name="remove-a-vm-for-update-management"></a>Eliminación de una VM de Update Management

Para quitar una VM de Update Management:

* En el área de trabajo de Log Analytics, quite la VM de la búsqueda guardada con la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates`. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.
* Quite [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) o el [agente de Log Analytics para Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

Continúe con el tutorial para aprender a administrar actualizaciones de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Administración de actualizaciones y revisiones para las máquinas virtuales Windows de Azure](automation-tutorial-update-management.md)

* Usar búsquedas de registros en [registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md) para ver datos detallados sobre la actualización.
* [Cree alertas](automation-tutorial-update-management.md#configure-alerts) para el estado de implementación de actualizaciones.

* Para información sobre cómo interactuar con Update Management a través de la API de REST, consulte [Software Update Configurations](/rest/api/automation/softwareupdateconfigurations) (Configuraciones de actualización de software).
* Para aprender a solucionar los problemas de Update Management, consulte [Solución de problemas de Update Management](troubleshoot/update-management.md)
