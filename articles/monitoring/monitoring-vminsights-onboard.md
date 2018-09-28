---
title: Incorporación de Azure Monitor para VM | Microsoft Docs
description: En este artículo se describe cómo incorporar y configurar Azure Monitor para VM para poder empezar a comprender el rendimiento de la aplicación distribuida y los problemas de estado identificados.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: e5421ca791ae9d0059639000f33b77be57f4d891
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968040"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms"></a>Cómo incorporar Azure Monitor para VM 
En este artículo se describe cómo configurar Azure Monitor para VM para supervisar el estado del sistema operativo de las máquinas virtuales de Azure, y detectar y asignar las dependencias de aplicación que pueden hospedar.  

La habilitación de Azure Monitor para VM se logra mediante uno de los métodos siguientes y se proporcionan detalles sobre el uso de cada método más adelante en el artículo.  

* Una sola máquina virtual de Azure, seleccionando **Insights (versión preliminar)** directamente desde la VM.
* Varias VM de Azure con Azure Policy para asegurarse de que las VM nuevas y existentes que se evalúan tienen las dependencias necesarias instaladas y se han configurado correctamente.  Las VM no compatibles se notifican para que pueda decidir a partir de lo que no cumple los requisitos y cómo quiere corregirlo.  
* Varias VM o conjuntos de escalado de máquinas virtuales de Azure en una suscripción o grupo de recursos concreto mediante PowerShell.

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar, asegúrese de disponer de lo siguiente, como se describe en las subsecciones siguientes.

### <a name="log-analytics"></a>Log Analytics 

Actualmente, se admite un área de trabajo de Log Analytics en las siguientes regiones:

  - Centro occidental de EE.UU.  
  - Este de EE. UU  
  - Europa occidental  
  - Sudeste Asiático<sup>1</sup>  

<sup>1</sup> Actualmente, esta región no admite la característica de estado de Azure Monitor para VM   

Si no tiene ninguna área de trabajo, puede crearla desde [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md).  

Para habilitar la solución, debe ser miembro del rol de colaborador de Log Analytics. Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

### <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Las siguientes versiones de los sistemas operativos Windows y Linux son compatibles oficialmente con Azure Monitor para VM:

|Versión del SO |Rendimiento |Mapas |Health |  
|-----------|------------|-----|-------|  
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X| X |  
|Cent OS Linux 7, 6 | X | X| X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> La característica de rendimiento de Azure Monitor para VM sólo está disponible en Azure Monitor; no está disponible cuando se accede a esta desde el panel izquierdo de la VM de Azure directamente.  

### <a name="hybrid-environment-connected-sources"></a>Orígenes conectados del entorno híbrido
La asignación de Azure Monitor para VM obtiene sus datos de Microsoft Dependency Agent. Dependency Agent depende del agente de Log Analytics en lo que respecta a sus conexiones a Log Analytics. Es decir, un sistema debe tener instalado y configurado el agente de Log Analytics con Dependency Agent.  En la tabla siguiente se describen los orígenes conectados que son compatibles con la característica Asignación en un entorno híbrido.

| Origen conectado | Compatible | DESCRIPCIÓN |
|:--|:--|:--|
| Agentes de Windows | SÍ | Además del [agente de Log Analytics para Windows](../log-analytics/log-analytics-concept-hybrid.md), los agentes de Windows requieren Microsoft Dependency Agent. Consulte los [sistemas operativos compatibles](#supported-operating-systems) para obtener una lista completa de las versiones de sistema operativo. |
| Agentes de Linux | SÍ | Además del [agente de Log Analytics para Linux](../log-analytics/log-analytics-concept-hybrid.md), los agentes de Linux requieren Microsoft Dependency Agent. Consulte los [sistemas operativos compatibles](#supported-operating-systems) para obtener una lista completa de las versiones de sistema operativo. |
| Grupo de administración de System Center Operations Manager | Sin  | |  

En Windows, tanto System Center Operations Manager como Log Analytics usan Microsoft Monitoring Agent (MMA) para recopilar y enviar los datos de supervisión. System Center Operations Manager y Log Analytics proporcionan versiones integradas diferentes del agente. Cada una de estas versiones puede informar a System Center Operations Manager, a Log Analytics o a ambos.  

En Linux, el agente de Log Analytics para Linux recopila y envía datos de supervisión a Log Analytics.   

Si los equipos Windows o Linux no pueden conectarse directamente al servicio, deberá configurar el agente de Log Analytics para conectarse a Log Analytics mediante la puerta de enlace de OMS. Para obtener más información sobre cómo implementar y configurar la puerta de enlace de OMS, vea [Conexión de equipos sin acceso a Internet mediante OMS Gateway](../log-analytics/log-analytics-oms-gateway.md).  

Dependency Agent se puede descargar desde la ubicación siguiente.

| Archivo | SO | Versión | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.5.0 | 8B8FE0F6B0A9F589C4B7B52945C2C25DF008058EB4D4866DC45EE2485062C9D7 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.5.1 | 09D56EF43703A350FF586B774900E1F48E72FE3671144B5C99BB1A494C201E9E |

## <a name="diagnostic-and-usage-data"></a>Datos de diagnóstico y uso
Microsoft recopila automáticamente datos de uso y rendimiento a través del servicio Azure Monitor. Microsoft usa estos datos para proporcionar y mejorar la calidad, la seguridad y la integridad del servicio. Con el fin de proporcionar funcionalidades de solución de problemas precisas y eficientes, los datos de la característica de asignación incluyen información sobre la configuración del software, como sistema operativo y versión, dirección IP, nombre DNS y nombre de la estación de trabajo. Microsoft no recopila nombres, direcciones ni otra información de contacto.

Para más información sobre el uso y la recopilación de datos, vea la [Declaración de privacidad de Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="performance-counters-enabled"></a>Contadores de rendimiento habilitados
Azure Monitor para VM configura un área de trabajo de Log Analytics para recopilar contadores de rendimiento que usa la solución.  En la tabla siguiente se enumeran los objetos y contadores que configura la solución que se recopilan cada 60 segundos.

### <a name="windows-performance-counters"></a>Contadores de rendimiento de Windows

|Nombre del objeto |Nombre del contador |  
|------------|-------------|  
|LogicalDisk |% de espacio libre |  
|LogicalDisk |Prom. Segundos de disco/lecturas |  
|LogicalDisk |Prom. Segundos de disco/transferencias |  
|LogicalDisk |Prom. Segundos de disco/escrituras |  
|LogicalDisk |Bytes de disco/s |  
|LogicalDisk |Bytes de lectura de disco/s  |  
|LogicalDisk |Lecturas de disco/s  |  
|LogicalDisk |Transferencias de disco/s |  
|LogicalDisk | Bytes de escritura en disco/s |  
|LogicalDisk |Escrituras en disco/s |  
|LogicalDisk |Megabytes libres |  
|Memoria |MB disponibles |  
|Adaptador de red |Bytes recibidos por segundo |  
|Adaptador de red |Bytes enviados por segundo |  
|Procesador |% de tiempo de procesador |  

### <a name="linux-performance-counters"></a>Contadores de rendimiento de Linux

|Nombre del objeto |Nombre del contador |  
|------------|-------------|  
|Disco lógico |% espacio usado |  
|Disco lógico |Bytes de lectura de disco/s  |  
|Disco lógico |Lecturas de disco/s  |  
|Disco lógico |Transferencias de disco/s |  
|Disco lógico | Bytes de escritura en disco/s |  
|Disco lógico |Escrituras en disco/s |  
|Disco lógico |Megabytes libres |  
|Disco lógico |Bytes de disco lógico/s |  
|Memoria |MB de memoria disponibles |  
|Red |Número total de bytes recibidos |  
|Red |Número total de bytes transmitidos |  
|Procesador |% de tiempo de procesador |  

## <a name="enable-from-the-azure-portal"></a>Habilitar desde Azure Portal
Para habilitar la supervisión de la VM de Azure en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione **Virtual Machines**. 
2. En la lista, seleccione una máquina virtual. 
3. En la página de la VM, en la sección **Supervisión**, seleccione **Insights (versión preliminar)**.
4. En la página **Insights (versión preliminar)**, seleccione **Probar ahora**.

    ![Habilitar Azure Monitor para VM para una máquina virtual](./media/monitoring-vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. En la página **Azure Monitor Insights Onboarding** (Incorporación a Insights de Azure Monitor), si tiene un área de trabajo de Log Analytics existente en la misma suscripción, selecciónela en la lista desplegable.  La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa la máquina virtual en la suscripción. 

    >[!NOTE]
    >Si quiere crear una área de trabajo de Log Analytics nueva para almacenar los datos de supervisión de la VM, siga las instrucciones de [Creación de una área de trabajo de Log Analytics en Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md). Asegúrese de crear el área de trabajo en la misma suscripción en la que está implementada la VM. 

Después de habilitar la supervisión, pueden pasar unos 10 minutos hasta que pueda ver la métrica de estado de la máquina virtual. 

![Habilitar el procesamiento de implementación de supervisión de Azure Monitor para VM](./media/monitoring-vminsights-onboard/onboard-vminsights-vm-portal-status.png)

## <a name="enable-using-azure-policy"></a>Habilitar el uso de Azure Policy
Para habilitar una solución para varias VM de Azure que garantice el cumplimiento coherente y la habilitación automática de nuevas VM aprovisionadas, se recomienda [Azure Policy](../azure-policy/azure-policy-introduction.md).  El uso de Azure Policy con las directivas proporcionadas proporciona los beneficios siguientes para VM nuevas:

* Habilitar Azure Monitor para VM para cada máquina virtual del ámbito definido
* Implementar el agente de Log Analytics 
* Implementar Dependency Agent para detectar dependencias de aplicación y mostrarlas en la asignación
* Auditar si la imagen del sistema operativo de la VM de Azure se encuentra en una lista predefinida en la definición de la directiva  
* Auditar si la VM de Azure se registra en una área de trabajo distinta de la especificada
* Informar sobre los resultados de cumplimiento 
* Admitir correcciones en caso de VM no compatibles

Para activarlo para el inquilino, este proceso requiere:

- Configurar una área de trabajo de Log Analytics mediante los pasos que se indican aquí
- Importar la definición de iniciativa a su inquilino (en el nivel de suscripción o grupo de administración)
- Asignar la directiva al ámbito deseado
- Revisar los resultados de cumplimiento

### <a name="add-the-policies-and-initiative-to-your-subscription"></a>Agregar las directivas y la iniciativa a la suscripción
Para usar las directivas, puede usar un script de PowerShell proporcionado ([agregar VMInsightsPolicy.ps1](https://www.powershellgallery.com/packages/Add-VMInsightsPolicy/1.2)) disponible desde la Galería de Azure PowerShell para completar esta tarea. El script agrega las directivas y una iniciativa a la suscripción.  Realice los pasos siguientes para configurar Azure Policy en la suscripción. 

1. Descargue el script de PowerShell en el sistema de archivos local.

2. Use el siguiente comando de PowerShell en la carpeta para agregar directivas. El script admite los siguientes parámetros opcionales: 

    ```powershell
    -UseLocalPolicies [<SwitchParameter>]
      <Optional> Load the policies from a local folder instead of https://raw.githubusercontent.com/dougbrad/OnBoardVMInsights/Policy/Policy/

    -SubscriptionId <String>
      <Optional> SubscriptionId to add the Policies/Initiatives to
    -ManagementGroupId <String>
      <Optional> Management Group Id to add the Policies/Initiatives to

    -Approve [<SwitchParameter>]
      <Optional> Gives the approval to add the Policies/Initiatives without any prompt
    ```  

    >[!NOTE]
    >Nota: Si tiene previsto asignar la iniciativa/directiva a varias suscripciones, las definiciones se deben almacenar en el grupo de almacenamiento que contiene las suscripciones a las que asignará la directiva. Por lo tanto, debe usar el parámetro ManagementGroupID.
    >
   
    Ejemplo sin parámetros:  `.\Add-VMInsightsPolicy.ps1`

### <a name="create-a-policy-assignment"></a>Creación de una asignación de directiva
Después de ejecutar el script `Add-VMInsightsPolicy.ps1` de PowerShell, se agregan la iniciativa y las directivas siguientes:

* **Deploy Log Analytics Agent for Windows VMs - Preview** (Implementar el agente de Log Analytics para VM Windows [versión preliminar])
* **Deploy Log Analytics Agent for Linux VMs - Preview** (Implementar el agente de Log Analytics para VM Linux [versión preliminar])
* **Deploy Dependency Agent for Windows VMs - Preview** (Implementar Dependency Agent para VM Windows [versión preliminar])
* **Deploy Dependency Agent for Linux VMs - Preview** (Implementar Dependency Agent para VM Linux [versión preliminar])
* **Implementación del agente de Log Analytics de auditoría: imagen de la VM (SO) no mostrada (versión preliminar)**
* **Implementación de Dependency Agent de auditoría: imagen de la VM (SO) no mostrada (versión preliminar)**

Se agrega el parámetro de iniciativa siguiente:

- **Área de trabajo de Log Analytics** (tiene que proporcionar el valor de ResourceID del área de trabajo si aplica una asignación con PowerShell o CLI)

    En el caso de las VM declaradas no compatibles con las directivas de auditoría de **VM fuera del ámbito del sistema operativo**, los criterios de la directiva de implementación solo incluyen las VM implementadas a partir de imágenes de VM conocidas. Consulte en la documentación si el sistema operativo de VM es compatible o no.  Si no lo es, necesitará duplicar la directiva de implementación, y actualizarla o modificarla para crear la imagen en el ámbito.

Se agrega la siguiente directiva opcional independiente:

- **VM is configured for mismatched Log Analytics Workspace - Preview (VM configurada para área de trabajo de Log Analytics no coincidente [versión preliminar])**

    Esto se puede usar para identificar VM que ya se han configurado con la [extensión de VM de Log Analytics](../virtual-machines/extensions/oms-windows.md), pero se configuran con un área de trabajo distinta a la esperada (tal y como indica la asignación de directiva). Esto toma un parámetro para WorkspaceID.

Con esta versión inicial, solo se puede crear la asignación de directiva desde Azure Portal. Para entender cómo completar estos pasos, consulte [Create a policy assignment from the Azure portal](../azure-policy/assign-policy-definition.md) (Crear una asignación de directiva desde Azure Portal).

## <a name="enable-with-powershell"></a>Habilitar con PowerShell
Para habilitar Azure Monitor para VM para varias máquinas virtuales o conjuntos de escalado de VM, puede usar un script de PowerShell proporcionado: [Install VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) disponible desde la Galería de Azure PowerShell para completar esta tarea.  Este script se iterará en cada máquina virtual y conjunto de escalado de VM de la suscripción, en el grupo de recursos con ámbito que especifica *ResourceGroup*, o en una sola VM o conjunto de escalado que especifique *Name*.  Para cada VM o conjunto de escalado de máquinas virtuales, el script comprueba si la extensión de VM ya está instalada y, si no, intenta reinstalarla.  En caso contrario, procede a instalar las extensiones de VM de Log Analytics y Dependency Agent.   

Este script requiere la versión 5.7.0 o posterior del módulo de Azure PowerShell. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

Para obtener ayuda acerca del script, puede ejecutar `Get-Help` para obtener una lista de detalles del argumento y un ejemplo de uso.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

En el ejemplo siguiente se muestra cómo utilizar los comandos de PowerShell en la carpeta para habilitar Azure Monitor para VM y comprender el resultado esperado:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-hybrid-environment"></a>Habilitar para el entorno híbrido
En esta sección se explica cómo incorporar máquinas virtuales o equipos físicos hospedados en el centro de datos o en otro entorno en la nube para que los supervise Azure Monitor para VM.  

Dependency Agent de asignación de Azure Monitor para VM no transmite ningún dato y no requiere ningún cambio en firewalls o puertos. Los datos de la asignación siempre se transmiten mediante el agente de Log Analytics al servicio de Azure Monitor, ya sea directamente o a través de la [Puerta de enlace de OMS](../log-analytics/log-analytics-oms-gateway.md) si las directivas de seguridad de TI no permiten que los equipos de la red se conecten a Internet.

Revise los requisitos y los métodos de implementación para el [Agente Linux y Windows de Log Analytics](../log-analytics/log-analytics-concept-hybrid.md).

Pasos resumidos:

1. Instalar el agente de Log Analytics para Windows o Linux
2. Instalar Dependency Agent de asignación de Azure Monitor para VM
3. Habilitar la recopilación de contadores de rendimiento
4. Incorporar Azure Monitor para VM

### <a name="install-the-dependency-agent-on-windows"></a>Instalar Dependency Agent en Windows 
Dependency Agent puede instalarse manualmente en equipos Windows ejecutando `InstallDependencyAgent-Windows.exe`. Si ejecuta este archivo ejecutable sin opciones, se inicia un asistente para instalación que puede seguir para realizar la instalación de forma interactiva.  

>[!NOTE]
>Se requieren privilegios de administrador para instalar o desinstalar al agente.

En la tabla siguiente se destacan los parámetros específicos que admite el programa de instalación para el agente desde la línea de comandos.  

| Parámetro | DESCRIPCIÓN |
|:--|:--|
| /? | Devuelve una lista de las opciones de la línea de comandos. |
| /S | Realice una instalación silenciosa sin interacción del usuario. |

Por ejemplo, para ejecutar el programa de instalación con el parámetro `/?`, escriba `InstallDependencyAgent-Windows.exe /?`

De forma predeterminada, los archivos de Windows Dependency Agent se instalan en `C:\Program Files\Microsoft Dependency Agent`.  Si Dependency Agent no se inicia una vez completada la instalación, compruebe los registros para obtener información detallada del error. El directorio de registro es `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>Instalación de Dependency Agent en Linux
Dependency Agent se instala en servidores Linux desde `InstallDependencyAgent-Linux64.bin`, un script de shell con un archivo binario autoextraíble. Puede ejecutar el archivo mediante `sh` o agregar permisos de ejecución al propio archivo.

>[!NOTE]
> Es necesario el acceso raíz para instalar o configurar el agente.
> 

| Parámetro | DESCRIPCIÓN |
|:--|:--|
| -help | Obtenga una lista de las opciones de la línea de comandos. |
| -s | Realice una instalación silenciosa sin preguntas. |
| --check | Compruebe los permisos y el sistema operativo, pero no instale el agente. |

Por ejemplo, para ejecutar el programa de instalación con el parámetro `-help`, escriba `InstallDependencyAgent-Linux64.bin -help`

Instale Dependency Agent de Linux como raíz ejecutando el comando siguiente: `sh InstallDependencyAgent-Linux64.bin`
    
Si Dependency Agent no se inicia, compruebe los registros para obtener información detallada del error. En los agentes de Linux, el directorio de registro es `/var/opt/microsoft/dependency-agent/log`.

Los archivos de Dependency Agent se colocan en los directorios siguientes:

| Archivos | Ubicación |
|:--|:--|
| Archivos principales | /opt/microsoft/dependency-agent |
| Archivos de registro | /var/opt/microsoft/dependency-agent/log |
| Archivos de configuración | /etc/opt/microsoft/dependency-agent/config |
| Archivos ejecutables de servicio | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Archivos binarios de almacenamiento | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Activar los contadores de rendimiento
Si el área de trabajo de Log Analytics a la que hace referencia la solución no está configurada para recopilar los contadores de rendimiento que requiere la solución, deben habilitarse. Se puede hacer manualmente, como se describe [aquí](../log-analytics/log-analytics-data-sources-performance-counters.md), o mediante la descarga y ejecución de un script de PowerShell disponible en la [galería de Powershell de Azure](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1).
 
### <a name="onboard-azure-monitor-for-vms"></a>Incorporar Azure Monitor para VM
Este método incluye una plantilla JSON que especifica la configuración para habilitar los componentes de la solución en el área de trabajo de Log Analytics.  

Si no conoce el concepto de implementación de recursos mediante una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) 

Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Creación y ejecución de una plantilla

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Guarde este archivo como **installsolutionsforvminsights.json** en una carpeta local.
3. Edite los valores de **WorkspaceName**, **ResourceGroupName** y **WorkspaceLocation**.  El valor de **WorkspaceName** es el identificador de recurso completo de su área de trabajo de Log Analytics, que incluye el nombre del área de trabajo y el valor de **WorkspaceLocation** es la región en que se define el área de trabajo.
4. Puede implementar una plantilla mediante el siguiente comando de PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

    ```powershell
    provisioningState       : Succeeded
    ```
Después de habilitar la supervisión, pueden pasar unos 10 minutos hasta que pueda ver la métrica y el estado del equipo híbrido. 

## <a name="next-steps"></a>Pasos siguientes

Con la supervisión habilitada para la máquina virtual, esta información está disponible para su análisis con Azure Monitor para VM.  Para obtener información sobre cómo usar la característica de mantenimiento, consulte [Ver el estado de Azure Monitor para VM](monitoring-vminsights-health.md) o, para ver las dependencias de aplicaciones detectadas, consulte [Ver la asignación de Azure Monitor para VM](monitoring-vminsights-maps.md).  