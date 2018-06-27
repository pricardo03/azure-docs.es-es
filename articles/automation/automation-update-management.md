---
title: Solución Update Management de Azure
description: Este artículo está pensado para ayudarle a entender cómo usar la solución Azure Update Management para administrar las actualizaciones de los equipos Windows y Linux.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8013a221f818b2aca8163e5f5195cae17c8a6a95
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232148"
---
# <a name="update-management-solution-in-azure"></a>Solución Update Management de Azure

Puede usar la solución Update Management de Azure Automation para administrar las actualizaciones del sistema operativo de los equipos Windows y Linux implementados en Azure, en entornos locales o en otros proveedores de nube. Puede evaluar rápidamente el estado de las actualizaciones disponibles en todos los equipos agente y administrar el proceso de instalación de las actualizaciones necesarias para los servidores.

Update Management se puede habilitar en máquinas virtuales directamente desde una cuenta de Azure Automation. Para aprender a habilitar Update Management en máquinas virtuales desde una cuenta de Automation, consulte [Administración de actualizaciones para varias máquinas virtuales](manage-update-multi.md). También puede habilitar Update Management para una única máquina virtual desde el panel de la máquina virtual en Azure Portal. Este escenario está disponible para máquinas virtuales [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) y [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management).

## <a name="solution-overview"></a>Información general de la solución

Los equipos administrados por Update Management usan las siguientes configuraciones para evaluar e implementar actualizaciones:

* Microsoft Monitoring Agent (MMA) para Windows o Linux
* Extensión DSC (configuración de estado deseado) de PowerShell para Linux
* Hybrid Runbook Worker de Automation
* Microsoft Update o Windows Server Update Services (WSUS) para equipos Windows

El siguiente diagrama muestra una vista conceptual del comportamiento y un flujo de datos con el modo en que la solución evalúa y aplica las actualizaciones de seguridad a todos los equipos Windows Server y Linux conectados en un área de trabajo:

![Flujo del proceso de Update Management](media/automation-update-management/update-mgmt-updateworkflow.png)

Después de que un equipo realiza un examen de cumplimiento de las actualizaciones, el agente reenvía la información de forma masiva a Azure Log Analytics. En un equipo Windows, el examen de cumplimiento se realiza cada 12 horas de forma predeterminada. 

Además del examen programado, el examen de cumplimiento de las actualizaciones se inicia a los 15 minutos si se reinicia MMA, antes y después de la instalación de actualizaciones. 

En un equipo Linux, el examen de cumplimiento se realiza cada 3 horas de forma predeterminada. Si se reinicia el agente MMA, se inicia un examen de cumplimiento al cabo de 15 minutos.

La solución informa del grado de actualización del equipo en función del origen configurado para la sincronización. Si el equipo Windows está configurado para informar a WSUS, en función de cuándo WSUS se sincronizó por última vez con Microsoft Update, los resultados pueden diferir de lo que se muestra en Microsoft Updates. Esto mismo ocurre en los equipos Linux que están configurados para informar a un repositorio local en lugar de a uno público.

> [!NOTE]
> Para informar correctamente al servicio, Update Management requiere la habilitación de ciertas direcciones URL y puertos. Para más información sobre estos requisitos, consulte [Planeamiento de red para Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Puede implementar e instalar las actualizaciones de software en equipos que requieren las actualizaciones mediante la creación de una implementación programada. Las actualizaciones clasificadas como *Opcional* no se incluyen en el ámbito de implementación en equipos Windows. Solo se incluyen las actualizaciones necesarias. 

La implementación programada define qué equipos de destino reciben las actualizaciones aplicables, ya sea mediante la especificación explícita de los equipos o por medio de la selección de un [grupo de equipos](../log-analytics/log-analytics-computer-groups.md) que se basa en las búsquedas en registros de un conjunto determinado de equipos. También se especifica una programación para aprobar y designar un período de tiempo durante el que se pueden instalar actualizaciones. 

Los Runbooks instalan las actualizaciones en Azure Automation. No puede ver estos runbooks, y los runbooks no requieren ninguna configuración. Cuando se crea una implementación de actualizaciones, esta crea una programación que inicia un runbook de actualización maestro a la hora especificada para los equipos incluidos. El runbook maestro inicia un runbook secundario en cada agente para realizar la instalación de las actualizaciones necesarias.

En la fecha y hora especificadas en la implementación de actualizaciones, los equipos de destino ejecutan la implementación en paralelo. Antes de la instalación, se realiza un examen para comprobar que las actualizaciones siguen siendo necesarias. En los equipos cliente WSUS, si no se aprueban las actualizaciones en WSUS, se produce un error en la implementación de actualizaciones.

## <a name="clients"></a>Clientes

### <a name="supported-client-types"></a>Tipos de cliente admitidos

En la tabla siguiente se muestra una lista de sistemas operativos compatibles:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Solo admite evaluaciones de actualización.         |
|Windows Server 2008 R2 SP1 y posterior     |Se requiere .NET Framework 4.5 o posterior. ([Descargar .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Se requiere Windows PowerShell 4.0 o posterior. ([Descargar WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Se recomienda Windows PowerShell 5.1 para aumentar la confiabilidad.  ([Descargar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) y 7 (x64)      | Los agentes de Linux deben tener acceso a un repositorio de actualización. La aplicación de revisiones basada en la clasificación requiere "yum" para devolver los datos de seguridad que CentOS no tiene de forma nativa.         |
|Red Hat Enterprise (x86/x64) 6 y 7 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|SUSE Linux Enterprise Server 11 (x86/x64) y 12 (x64)     | Los agentes de Linux deben tener acceso a un repositorio de actualización.        |
|Ubuntu 14.04 LTS y 16.04 LTS (x86/x64)      |Los agentes de Linux deben tener acceso a un repositorio de actualización.         |

### <a name="unsupported-client-types"></a>Tipos de cliente no admitidos

En la tabla siguiente se enumeran los sistemas operativos no admitidos:

|Sistema operativo  |Notas  |
|---------|---------|
|Cliente Windows     | No se admiten sistemas operativos cliente (por ejemplo, Windows 7 y Windows 10).        |
|Windows Server 2016 Nano Server     | No compatible.       |

### <a name="client-requirements"></a>Requisitos del cliente

#### <a name="windows"></a>Windows

Los agentes de Windows deben estar configurados para comunicarse con un servidor de WSUS o tener acceso a Microsoft Update. Puede usar Update Management con System Center Configuration Manager. Para más información sobre escenarios de integración, consulte [Integración de System Center Configuration Manager con Update Management](oms-solution-updatemgmt-sccmintegration.md#configuration). El [agente de Windows](../log-analytics/log-analytics-agent-windows.md) es necesario. Este agente se instala automáticamente si va a incorporar una máquina virtual de Azure.

#### <a name="linux"></a>Linux

En Linux, la máquina debe tener acceso a un repositorio de actualización. El repositorio de actualización puede ser público o privado. No se admite con esta solución un agente de Operations Management Suite (OMS) para Linux que esté configurado para informar a varias áreas de trabajo de Log Analytics.

Para más información sobre cómo instalar el agente de OMS para Linux y descargar la versión más reciente, consulte [Agente de Operations Management Suite para Linux](https://github.com/microsoft/oms-agent-for-linux). Para más información sobre cómo instalar el agente de OMS para Windows, consulte [Agente de Operations Management Suite para Windows](../log-analytics/log-analytics-windows-agent.md).

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

Para más información sobre cómo se actualizan los módulos de administración de soluciones, consulte [Conexión de Operations Manager con Log Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Confirmación de que las máquinas que no son de Azure están incorporadas

Para confirmar que las máquinas conectadas directamente se comunican con Log Analytics, después de unos minutos, puede ejecutar una de las siguientes búsquedas de registros:

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

En un equipo Windows, puede revisar la siguiente información para comprobar la conectividad del agente con Log Analytics:

1. En el Panel de Control, abra **Microsoft Monitoring Agent**. En la pestaña **Azure Log Analytics**, el agente muestra un mensaje que indica que **Microsoft Monitoring Agent se ha conectado correctamente a Log Analytics**.
2. Abra el registro de eventos de Windows. Vaya a **Application and Services Logs\Operations Manager** y busque los identificadores de evento 3000 y 5002 desde el **Conector de servicio** de origen. Estos eventos indican que el equipo se ha registrado con el área de trabajo de Log Analytics y está recibiendo la configuración.

Si el agente no puede comunicarse con Log Analytics y está configurado para comunicarse con Internet mediante un firewall o un servidor proxy, confirme que estos están configurados correctamente. Para aprender a comprobar que un firewall o un servidor proxy estén correctamente configurados, consulte [Configuración de red para el agente Windows](../log-analytics/log-analytics-agent-windows.md) o [Configuración de red para el agente de Linux](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Si los sistemas Linux están configurados para comunicarse con un servidor proxy o una puerta de enlace de OMS y va a incorporar esta solución, actualice los permisos *proxy.conf* para conceder al grupo omiuser permiso de lectura sobre este archivo mediante la ejecución de los siguientes comandos:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Los agentes de Linux recién agregados muestran un estado **Actualizado** después de haber realizado una evaluación. Este proceso puede tardar hasta 6 horas.

Para confirmar que un grupo de administración de Operations Manager se comunica con Log Analytics, consulte [Validación de la integración de Operations Manager con Log Analytics](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics).

## <a name="data-collection"></a>Colección de datos

### <a name="supported-agents"></a>Agentes admitidos

En la tabla siguiente se describen los orígenes conectados que son compatibles con esta solución:

| Origen conectado | Compatible | DESCRIPCIÓN |
| --- | --- | --- |
| Agentes de Windows |Sí |La solución recopila información sobre las actualizaciones del sistema de los agentes de Windows e inicia a continuación la instalación de las actualizaciones necesarias. |
| Agentes de Linux |Sí |La solución recopila información sobre las actualizaciones del sistema de los agentes de Linux e inicia a continuación la instalación de las actualizaciones necesarias en las distribuciones admitidas. |
| Grupo de administración de Operations Manager |Sí |La solución recopila información acerca de las actualizaciones del sistema de agentes en un grupo de administración conectado.<br/>No se requiere ninguna conexión directa entre el agente de Operations Manager y Log Analytics. Los datos se reenvían desde el grupo de administración al área de trabajo de Log Analytics. |

### <a name="collection-frequency"></a>Frecuencia de recopilación

Para cada equipo Windows administrado, se realiza un examen dos veces al día. Cada 15 minutos, se llama a la API de Windows para consultar la hora de la última actualización y determinar si ha cambiado el estado. Si ha cambiado el estado, se inicia un examen de cumplimiento. 

Para cada equipo Linux administrado, se realiza un examen cada 3 horas.

Puede que transcurran entre 30 minutos y 6 horas antes de que se muestren los datos actualizados de los equipos administrados.

## <a name="viewing-update-assessments"></a>Visualización de la evaluación de actualización

En la cuenta de Automation, seleccione **Update Management** para ver el estado de sus máquinas.

Esta vista proporciona información sobre las máquinas, las actualizaciones que faltan, las implementaciones de actualizaciones y las implementaciones de actualizaciones programadas. En la columna **Cumplimiento**, puede ver la última vez que se evaluó la máquina. En la columna **Preparación de actualizaciones del agente**, puede ver el estado del agente de actualización. Si hay un problema, seleccione el vínculo para dirigirse a la documentación de solución de problemas, que le ayudará a conocer los pasos que debe realizar para corregirlo.

Para ejecutar una búsqueda de registros que devuelva información sobre la máquina, las actualizaciones o las implementaciones, seleccione el elemento de la lista. Se abre el panel **Búsqueda de registros** con una consulta para el elemento seleccionado:

![Vista predeterminada de Update Management](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Instalación de actualizaciones

Una vez que se han evaluado las actualizaciones de todos los equipos Linux y Windows del área de trabajo, puede instalar las actualizaciones necesarias mediante la creación de una *implementación de actualizaciones*. Una implementación de actualizaciones es una instalación programada de actualizaciones necesarias en uno o más equipos. Se especifica la fecha y la hora de la implementación y el equipo o el grupo de equipos que se incluirán en el ámbito de esta. Para más información sobre grupos de equipos, consulte [Grupos de equipos de Log Analytics](../log-analytics/log-analytics-computer-groups.md).

 Al incluir grupos de equipos en la implementación de actualizaciones, la pertenencia al grupo se evalúa solo en el momento de la creación de la programación. Los cambios posteriores en un grupo no se reflejan. Para resolver este problema, elimine la implementación de actualizaciones programada y vuelva a crearla.

> [!NOTE]
> Las máquinas virtuales Windows implementadas desde Azure Marketplace se establecen de forma predeterminada para recibir actualizaciones automáticas del servicio Windows Update. Este comportamiento no cambia cuando se agrega esta solución o se agregan máquinas virtuales Windows al área de trabajo. Si no administra activamente las actualizaciones mediante esta solución, se aplica el comportamiento predeterminado (las actualizaciones se aplican automáticamente).

Para evitar que las actualizaciones se apliquen fuera de una ventana de mantenimiento en Ubuntu, vuelva a configurar el paquete de actualizaciones desatendidas para deshabilitar las actualizaciones automáticas. Para más información sobre cómo configurar el paquete, consulte el tema [Actualizaciones automáticas](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) en la Guía de Ubuntu Server.

Las máquinas virtuales que se crearon a partir de las imágenes a petición de Red Hat Enterprise Linux (RHEL) que están disponibles en Azure Marketplace están registradas para acceder a la instancia de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) implementada en Azure. Cualquier otra distribución de Linux se debe actualizar desde el repositorio de archivos en línea de la distribución en cuestión según los métodos admitidos por esta.

## <a name="view-missing-updates"></a>Visualización de actualizaciones que faltan

Haga clic en **Actualizaciones que faltan** para ver la lista de actualizaciones que faltan en las máquinas. Se muestran todas las actualizaciones, y se pueden seleccionar. Aparece información sobre el número de máquinas que requieren la actualización, el sistema operativo y un vínculo para más información. El panel **Búsqueda de registros** muestra más información sobre las actualizaciones.

## <a name="view-update-deployments"></a>Visualización de implementaciones de actualizaciones

Seleccione la pestaña **Implementaciones de actualizaciones** para ver la lista de implementaciones de actualizaciones existentes. Seleccione cualquiera de las implementaciones de actualizaciones de la tabla para abrir el panel **Actualizar la ejecución de implementación** para esa implementación de actualizaciones.

![Información general de los resultados de la implementación de actualizaciones](./media/automation-update-management/update-deployment-run.png)

## <a name="create-or-edit-an-update-deployment"></a>Creación o edición de una implementación de actualizaciones

Para crear una nueva implementación de actualizaciones, seleccione **Programar implementación de actualizaciones**. Se abre el panel **Nueva implementación de actualización**. Escriba valores para las propiedades descritas en la tabla siguiente:

| Propiedad | DESCRIPCIÓN |
| --- | --- |
|NOMBRE |Nombre único para identificar la implementación de actualizaciones. |
|Sistema operativo| Seleccione **Linux** o **Windows**.|
|Máquinas para actualizar |Seleccione una búsqueda guardada o elija **Máquina** en la lista desplegable y seleccione máquinas individuales. |
|Clasificaciones de actualizaciones|Seleccione todas las clasificaciones de actualizaciones que necesite. CentOS no admite estas clasificaciones de forma nativa.|
|Actualizaciones para excluir|Escriba las actualizaciones que desea excluir. En Windows, escriba el artículo de la KB sin el prefijo **KB**. En Linux, escriba el nombre del paquete o use un carácter comodín.  |
|Configuración de programación|Seleccione la hora de inicio y seleccione **Una vez** o **Periódico** para la periodicidad.|| Ventana de mantenimiento |Número de minutos establecido para las actualizaciones. El valor no puede ser inferior a 30 minutos ni superior a 6 horas. |

## <a name="update-classifications"></a>Clasificaciones de actualizaciones

En las siguientes tablas se muestran las clasificaciones de actualizaciones en Update Management, junto con una definición de cada clasificación.

### <a name="windows"></a>Windows

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

Actualmente no hay ningún método admitido para habilitar la disponibilidad de datos de clasificación nativos en CentOS. En este momento, solo se proporciona soporte técnico a clientes que pueden haber habilitado esto por su cuenta.

## <a name="ports"></a>Puertos

Las direcciones siguientes se requieren específicamente para Update Management. La comunicación con estas direcciones se realiza a través del puerto 443.

|Azure Public  |Azure Government  |
|---------|---------|
|* .ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|* .oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|* .blob.core.windows.net|*.blob.core.usgovcloudapi.net|

Para más información sobre los puertos que necesita Hybrid Runbook Worker, consulte los [puertos de roles de Hybrid Worker](automation-hybrid-runbook-worker.md#hybrid-worker-role).

## <a name="search-logs"></a>Búsqueda de registros

Además de los detalles que se proporcionan en Azure Portal, se pueden realizar búsquedas en los registros. En el panel **Change Tracking**, seleccione **Log Analytics**. Se abre el panel **Búsqueda de registros**.

### <a name="sample-queries"></a>Consultas de ejemplo

En la tabla siguiente se proporcionan ejemplos de búsquedas de registros para los registros de actualizaciones recopilados por esta solución:

| Consultar | DESCRIPCIÓN |
| --- | --- |
|Actualizar</br>&#124; where UpdateState == "Needed" and Optional == false</br>&#124; project Computer, Title, KBID, Classification, PublishedDate |Todos los equipos con actualizaciones pendientes</br>Agregue uno de los siguientes para limitar el sistema operativo:</br>OSType != "Linux"</br>OSType == "Linux" |
| Actualizar</br>&#124; where UpdateState == "Needed" and Optional == false</br>&#124; where Computer == "ContosoVM1.contoso.com"</br>&#124; project Computer, Title, KBID, Product, PublishedDate |Actualizaciones pendientes para un equipo específico (reemplace el valor por el nombre del equipo)|
| Evento</br>&#124; where EventLevelName == "error" and Computer in ((Update &#124; where (Classification == "Security Updates" or Classification == "Critical Updates")</br>&#124; where UpdateState == "Needed" and Optional == false </br>&#124; distinct Computer)) |Eventos de error para las máquinas que tienen pendientes actualizaciones de seguridad o críticas necesarias |
| Actualizar</br>&#124; where UpdateState == "Needed" and Optional == false</br>&#124; distinct Title |Actualizaciones distintivas pendientes en todos los equipos |
| UpdateRunProgress</br>&#124; where InstallationStatus == "failed" </br>&#124; summarize AggregatedValue = count() by Computer, Title, UpdateRunName |Equipos con actualizaciones con error en la ejecución de la actualización</br>Agregue uno de los siguientes para limitar el sistema operativo:</br>OSType = "Windows"</br>OSType == "Linux" |
| Actualizar</br>&#124; where OSType == "Linux"</br>&#124; where UpdateState != "Not needed" and (Classification == "Critical Updates" or Classification == "Security Updates")</br>&#124; summarize AggregatedValue = count() by Computer |Lista de todas las máquinas Linux que tienen un paquete de actualización disponible que soluciona puntos vulnerables de seguridad o críticos |
| UpdateRunProgress</br>&#124; where UpdateRunName == "DeploymentName"</br>&#124; summarize AggregatedValue = count() by Computer|Equipos que se actualizaron en esta ejecución de la actualización (reemplace el valor por el nombre de la implementación de actualizaciones) |

## <a name="integrate-with-system-center-configuration-manager"></a>Integración con System Center Configuration Manager

Los clientes que han invertido en System Center Configuration Manager para administrar equipos, servidores y dispositivos móviles también dependen de la fuerza y la madurez de Configuration Manager para ayudarles a administrar actualizaciones de software. Configuration Manager es parte de su ciclo de administración de actualizaciones de software (SUM).

Para aprender a integrar la solución de administración con System Center Configuration Manager, consulte [Integración de System Center Configuration Manager con Update Management](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patch-linux-machines"></a>Aplicación de revisiones en máquinas Linux

Las siguientes secciones explican posibles problemas con la aplicación de revisiones de Linux.

### <a name="unexpected-os-level-upgrades"></a>Actualizaciones de nivel de sistema operativo inesperadas

En algunas variantes de Linux, como Red Hat Enterprise Linux, las actualizaciones de nivel de sistema operativo pueden producirse mediante paquetes. Esto puede llevar a ejecuciones de Update Management en las que cambia el número de versión del sistema operativo. Puesto que Update Management usa los mismos métodos para actualizar paquetes que un administrador utilizaría localmente en el equipo Linux, este comportamiento es deliberado.

Para evitar la actualización de la versión del sistema operativo con las ejecuciones de Update Management, use la característica **Exclusión**.

En Red Hat Enterprise Linux, el nombre del paquete para excluir sería: redhat-release-server.x86_64.

![Paquetes que se van a excluir para Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>No se aplican las revisiones críticas y de seguridad

Al implementar actualizaciones en una máquina Linux, puede seleccionar clasificaciones de actualizaciones. Esto filtra las actualizaciones que se aplican a aquellas que cumplen los criterios especificados. Este filtro se aplica localmente en el equipo cuando se implementa la actualización.

Dado que Update Management realiza enriquecimiento de actualizaciones en la nube, algunas actualizaciones pueden marcarse en Update Management como de impacto para la seguridad, aunque la máquina local no tenga esa información. Como resultado, si aplica actualizaciones críticas a una máquina Linux, puede haber actualizaciones que no estén marcadas como de impacto para la seguridad en esa máquina y no se apliquen.

Sin embargo, es posible que Update Management todavía notifique que esa máquina no es compatible porque tiene información adicional acerca de la actualización pertinente.

La implementación de actualizaciones mediante la clasificación de actualizaciones no funciona en CentOS de forma nativa. En SUSE, al seleccionar *solo* "Otras actualizaciones" como clasificación, puede dar lugar a que se instalen también algunas actualizaciones de seguridad si primero se requieren actualizaciones de seguridad relacionadas con zypper (administrador de paquetes) o sus dependencias. Esta es una limitación de zypper. En algunos casos, puede que se le pida volver a ejecutar la implementación de actualizaciones para comprobar el registro de actualizaciones.

## <a name="troubleshooting"></a>solución de problemas

En esta sección se proporciona información para resolver problemas con la solución Update Management.

### <a name="windows"></a>Windows

Si se producen problemas al intentar incorporar la solución o una máquina virtual, compruebe en los registros de eventos **Application and Services Logs/Operations Manager** de la máquina local los eventos con el identificador 4502 y los mensajes de eventos que contengan **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**. En la tabla siguiente se destacan los mensajes de error específicos y una posible solución para cada uno:

| Message | Motivo | Solución |
|----------|----------|----------|
| No se pudo registrar la máquina para la administración de revisiones,<br/>error en el registro con la excepción<br/>System.InvalidOperationException: {"Message":"La máquina ya<br/>está registrada en una cuenta diferente. "} | La máquina ya está incorporada a otra área de trabajo para Update Management. | Realice una limpieza de los artefactos antiguos y [elimine el grupo Hybrid Runbook](automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group).|
| No se puede registrar la máquina para la administración de revisiones; no se pudo registrar con la excepción<br/>System.Net.Http.HttpRequestException: error al enviar la solicitud. ---><br/>System.Net.WebException: la conexión subyacente<br/>se cerró: error inesperado<br/>en una operación de recepción. ---> System.ComponentModel.Win32Exception:<br/>el cliente y el servidor no pueden comunicarse,<br/>dado que no poseen un algoritmo común | El proxy, la puerta de enlace o el firewall bloquean la comunicación. | [Revise los requisitos de red](automation-hybrid-runbook-worker.md#network-planning).|
| No se pudo registrar la máquina para la administración de revisiones,<br/>error en el registro con la excepción<br/>Newtonsoft.Json.JsonReaderException: error al analizar el valor de infinito positivo. | El proxy, la puerta de enlace o el firewall bloquean la comunicación. | [Revise los requisitos de red](automation-hybrid-runbook-worker.md#network-planning).|
| El certificado presentado por el servicio \<wsid\>.oms.opinsights.azure.com<br/>no fue emitido por una entidad de certificación<br/>utilizada para los servicios de Microsoft. Contacto<br/>el administrador de red para comprobar si están ejecutando un proxy que intercepte<br/>la comunicación TLS/SSL. |El proxy, la puerta de enlace o el firewall bloquean la comunicación. | [Revise los requisitos de red](automation-hybrid-runbook-worker.md#network-planning).|
| No se pudo registrar la máquina para la administración de revisiones,<br/>error en el registro con la excepción<br/>AgentService.HybridRegistration.<br/>PowerShell.Certificates.CertificateCreationException:<br/>error al crear un certificado autofirmado. ---><br/>System.UnauthorizedAccessException: se denegó el acceso. | Error al generar un certificado autofirmado. | Compruebe que la cuenta del sistema tiene<br/>acceso de lectura a la carpeta:<br/>**C:\ProgramData\Microsoft\**<br/>** Crypto\RSA**|

### <a name="linux"></a>Linux

Si las ejecuciones de actualizaciones no se pueden iniciar en una máquina Linux, realice una copia del archivo de registro siguiente y consérvelo para la solución de problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

Si se producen errores durante una ejecución de actualizaciones después de que se ha iniciado correctamente en Linux, compruebe el trabajo de salida desde la máquina afectada en la ejecución. Puede encontrar mensajes de error específicos procedentes del Administrador de paquetes de su máquina que puede investigar e intentar solucionar. Update Management requiere que el administrador de paquetes tenga un estado correcto para que las implementaciones de actualizaciones se realicen con éxito.

En algunos casos, las actualizaciones de paquetes pueden interferir con Update Management e impedir que finalice una implementación de actualizaciones. Si ese fuera el caso, deberá excluir estos paquetes de las futuras ejecuciones de actualizaciones o instalarlos manualmente por su cuenta.

Si no puede resolver un problema de aplicación de revisiones, realice una copia del archivo de registro siguiente y consérvela **antes** de que se inicie la siguiente implementación de actualizaciones para solucionar los problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Pasos siguientes

Continúe con el tutorial para aprender a administrar actualizaciones de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Administración de actualizaciones y revisiones para las máquinas virtuales Windows de Azure](automation-tutorial-update-management.md)

* Use las búsquedas de registros de [Log Analytics](../log-analytics/log-analytics-log-searches.md) para ver datos de actualización detallados.
* [Cree alertas](../log-analytics/log-analytics-alerts.md) cuando se detecten actualizaciones críticas pendientes en equipos, o bien cuando un equipo tenga deshabilitadas las actualizaciones automáticas.
