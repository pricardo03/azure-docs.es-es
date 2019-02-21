---
title: Implementación de Azure Monitor para VM (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo implementar y configurar Azure Monitor para VM de modo que pueda empezar a comprender el rendimiento de la aplicación distribuida y los problemas de estado identificados.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2019
ms.author: magoedte
ms.openlocfilehash: 46df2d6828cd60aee3c64128197579eb6f51a11a
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340338"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Implementación de Azure Monitor para VM (versión preliminar)
En este artículo se describe cómo configurar Azure Monitor para VM. El servicio supervisa el estado del sistema operativo de las máquinas virtuales (VM) de Azure, los conjuntos de escalado de máquinas virtuales y las máquinas virtuales en el entorno. Esta supervisión incluye la detección y asignación de dependencias de aplicaciones que podrían hospedarse en ellos. 

Para habilitar Azure Monitor para VM, puede seguir alguno de los métodos siguientes:

* Habilitar una sola máquina virtual de Azure, seleccionando **Insights (versión preliminar)** directamente desde la misma.
* Habilitar dos o más máquinas virtuales de Azure mediante Azure Policy. Mediante este método, las dependencias necesarias de las máquinas virtuales nuevas y existentes se instalan y configuran correctamente. Las máquinas virtuales no compatibles se notifican para que pueda decidir si habilitarlas y cómo desea corregir esa incompatibilidad.
* Use PowerShell para habilitar dos o más máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure en una suscripción o grupo de recursos concreto.

Más adelante en este artículo, se proporciona información adicional sobre cada método.

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar, asegúrese de conocer la información de los apartados siguientes.

### <a name="log-analytics"></a>Log Analytics

Actualmente, se admite un área de trabajo de Log Analytics en las siguientes regiones:

- Centro occidental de EE.UU.
- Este de EE. UU
- Europa occidental
- Sudeste Asiático<sup>1</sup>

<sup>1</sup> Actualmente, esta región no admite la característica de estado de Azure Monitor para VM.

>[!NOTE]
>Se pueden implementar máquinas virtuales de Azure desde cualquier región, sin limitarse a las admitidas en el área de trabajo de Log Analytics.
>

Si no tiene un área de trabajo, puede crear una con alguno de los métodos siguientes:
* [La CLI de Azure](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Portal de Azure](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Si va a habilitar la supervisión en una sola máquina virtual de Azure desde Azure Portal, puede crear un área de trabajo durante el proceso.

Para habilitar la solución del escenario a escala, en primer lugar, configure lo siguiente en el área de trabajo de Log Analytics:

* Instale las soluciones ServiceMap e InfrastructureInsights. Solo puede completar esta instalación mediante una plantilla de Azure Resource Manager que se proporciona en este artículo.
* Configure el área de trabajo de Log Analytics para que recopile contadores de rendimiento.

Para configurar el área de trabajo para el escenario a escala, consulte Configuración del área de trabajo de Log Analytics para la implementación a escala.

### <a name="supported-operating-systems"></a>Sistemas operativos compatibles

La siguiente es una lista de los sistemas operativos Windows y Linux que son compatibles oficialmente con Azure Monitor para máquinas virtuales. Más adelante en esta sección, encontrará una lista completa que detalla las versiones de kernel admitidas y las versiones de sistema operativo Linux principales y secundarias.

|Versión del SO |Rendimiento |Mapas |Health |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X |  |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 7, 6| X | X| X |
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |
|CentOS Linux 7, 6 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |
|Oracle Linux 7 | X<sup>1</sup> | | X |
|Oracle Linux 6 | X | X | X |
|Debian 9.4, 8 | X<sup>1</sup> | | X |

<sup>1</sup> La característica Rendimiento de Azure Monitor para VM solo está disponible desde Azure Monitor. No está disponible cuando tiene acceso a ella directamente desde el panel izquierdo de la máquina virtual de Azure.

>[!NOTE]
>La siguiente información se aplica a la compatibilidad del sistema operativo Linux:
> - Se admiten solo versiones de kernel SMP Linux y predeterminados.
> - Las versiones de kernel no estándar, como Physical Address Extension (PAE) y Xen, no son compatibles con ninguna distribución de Linux. Por ejemplo, un sistema con la cadena de versión *2.6.16.21-0.8-xen* no es compatible.
> - No se admiten los kernel personalizados, incluidas las recompilaciones de kernels estándar.
> - No se admite el kernel de CentOSPlus.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versión del SO | Versión del kernel |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7,2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versión del SO | Versión del kernel |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4. | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6,8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Versión del SO | Versión del kernel |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15.* |
| Ubuntu 16.04.3 | kernel 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 con Unbreakable Enterprise Kernel
| Versión del SO | Versión del kernel
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4. | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 con Unbreakable Enterprise Kernel

| Versión del SO | Versión del kernel
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versión del SO | Versión del kernel
|:--|:--|
|12 SP2 | 4.4.* |
|12 SP3 | 4.4.* |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency Agent
La característica Service Map de Azure Monitor para VM obtiene sus datos de Microsoft Dependency Agent. Dependency Agent depende del agente de Log Analytics en lo que respecta a sus conexiones a Log Analytics. Por tanto, el sistema debe tener instalado y configurado el agente de Log Analytics con Dependency Agent.

Cuando se habilita Azure Monitor para VM para una sola máquina virtual de Azure o cuando se usa el método de implementación a escala, es necesario usar la extensión Dependency Agent de Azure VM para instalar el agente como parte de la experiencia.

En un entorno híbrido, puede descargar e instalar Dependency Agent de cualquiera de estas dos maneras: Manualmente o mediante un método de implementación automatizada para las máquinas virtuales que se hospedan fuera de Azure.

En la tabla siguiente se describen los orígenes conectados que son compatibles con la característica Asignación en un entorno híbrido.

| Origen conectado | Compatible | DESCRIPCIÓN |
|:--|:--|:--|
| Agentes de Windows | Sí | Además del [agente de Log Analytics para Windows](../../azure-monitor/platform/log-analytics-agent.md), los agentes de Windows requieren Microsoft Dependency Agent. Para obtener una lista completa de las versiones del sistema operativo, consulte los [sistemas operativos compatibles](#supported-operating-systems). |
| Agentes de Linux | Sí | Además del [agente de Log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md), los agentes de Linux requieren Microsoft Dependency Agent. Para obtener una lista completa de las versiones del sistema operativo, consulte los [sistemas operativos compatibles](#supported-operating-systems). |
| Grupo de administración de System Center Operations Manager | Sin  | |

Dependency Agent se puede descargar desde las ubicaciones siguientes:

| Archivo | SO | Versión | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) |  Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>Control de acceso basado en rol
Para habilitar las características de Azure Monitor para VM y acceder a ellas, debe tener asignados los siguientes roles de acceso:

- Para habilitar la solución, debe tener el rol de *colaborador de Log Analytics*.

- Para ver los datos de rendimiento, mantenimiento y el mapa, debe tener el rol de *lector de supervisión* en la máquina virtual de Azure. El área de trabajo de Log Analytics debe configurarse para Azure Monitor para VM.

Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../../azure-monitor/platform/manage-access.md).

## <a name="enable-monitoring-in-the-azure-portal"></a>Habilitación de la supervisión en Azure Portal
Para habilitar la supervisión de la VM de Azure en Azure Portal, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Seleccione **Máquinas virtuales**.

1. En la lista, seleccione una máquina virtual.

1. En la página de la VM, en la sección **Supervisión**, seleccione **Insights (versión preliminar)**.

1. En la página **Insights (versión preliminar)**, seleccione **Probar ahora**.

    ![Habilitar Azure Monitor para VM para una máquina virtual](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)
1. En la página **Azure Monitor Insights Onboarding** (Incorporación a Insights de Azure Monitor), si tiene un área de trabajo de Log Analytics existente en la misma suscripción, selecciónela en la lista desplegable.  
    La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa la máquina virtual en la suscripción. 

    >[!NOTE]
    >Si quiere crear una área de trabajo de Log Analytics nueva para almacenar los datos de supervisión de la VM, siga las instrucciones de [Creación de una área de trabajo de Log Analytics en Azure Portal](../../azure-monitor/learn/quick-create-workspace.md) en una de las regiones admitidas indicadas anteriormente.

Después de habilitar la supervisión, pueden pasar unos 10 minutos hasta que pueda ver la métrica de estado de la máquina virtual.

![Habilitar el procesamiento de implementación de supervisión de Azure Monitor para VM](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>Implementación a escala
En esta sección, se implementa Azure Monitor para VM a escala mediante Azure Policy o Azure PowerShell.

Antes de implementar las máquinas virtuales, configure previamente el área de trabajo de Log Analytics. Para ello, haga lo siguiente:

1. Si aún no tiene un área de trabajo, cree una que pueda admitir Azure Monitor para VM.  
    Antes de continuar, consulte [Administración de áreas de trabajo](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json) para conocer las consideraciones relativas al cumplimiento de normas, a la administración y al costo.

1. Cree una nueva área de trabajo si todavía no existe una que sea compatible con Azure Monitor para VM. Revise cómo [administrar áreas de trabajo](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json) antes de crear una nueva área de trabajo para conocer las consideraciones de costos, administración y cumplimiento antes de continuar.

1. Habilite los contadores de rendimiento en el área de trabajo para la recopilación en máquinas virtuales de Windows y Linux.

1. Instale y habilite las soluciones ServiceMap e InfrastructureInsights en el área de trabajo.

### <a name="set-up-a-log-analytics-workspace"></a>Configure un área de trabajo de Log Analytics
Si no tiene un área de trabajo de Log Analytics, cree una; para ello, revise los métodos que se sugieren en la sección ["Requisitos previos"](#log-analytics).

#### <a name="enable-performance-counters"></a>Activar los contadores de rendimiento
Si el área de trabajo de Log Analytics a la que la solución hace referencia no está todavía configurada para recopilar los contadores de rendimiento que la solución requiere, debe habilitarlos. Puede hacerlo de dos maneras:
* Manualmente, según se describe en [Orígenes de datos de rendimiento de Windows y Linux en Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Descargando y ejecutando un script de PowerShell que está disponible en [Galería de Azure PowerShell](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Instalación de las soluciones ServiceMap e InfrastructureInsights
Este método incluye una plantilla JSON que especifica la configuración para habilitar los componentes de la solución en el área de trabajo de Log Analytics.

Si no sabe cómo implementar los recursos con una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

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
    }
    ```

1. Guarde este archivo como *installsolutionsforvminsights.json* en una carpeta local.

1. Capture los valores de *WorkspaceName*, *ResourceGroupName* y *WorkspaceLocation*. El valor de *WorkspaceName* es el nombre del área de trabajo de Log Analytics. El valor de *WorkspaceLocation* es la región en la que el área de trabajo está definida.

1. Ya está listo para implementar esta plantilla.
 
    * Use los siguientes comandos de PowerShell en la carpeta que contenga la plantilla:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
        ```

        El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Para ejecutar el comando siguiente mediante la CLI de Azure:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --name DeploySolutions --resource-group <ResourceGroupName> --template-file InstallSolutionsForVMInsights.json --parameters WorkspaceName=<workspaceName> WorkspaceLocation=<WorkspaceLocation - example: eastus>
        ```

        El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

        ```azurecli
        provisioningState       : Succeeded

### Enable by using Azure Policy
To enable Azure Monitor for VMs at scale in a way that helps ensure consistent compliance and the automatic enabling of the newly provisioned VMs, we recommend [Azure Policy](../../governance/policy/overview.md). These policies:

* Deploy the Log Analytics agent and the Dependency agent.
* Report on compliance results.
* Remediate for non-compliant VMs.

To enable Azure Monitor for VMs by using Azure Policy in your tenant:

- Assign the initiative to a scope: management group, subscription, or resource group
- Review and remediate compliance results

For more information about assigning Azure Policy, see [Azure Policy overview](../../governance/policy/overview.md#policy-assignment) and review the [overview of management groups](../../governance/management-groups/index.md) before you continue.

The policy definitions are listed in the following table:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Enable Azure Monitor for VMs |Enable Azure Monitor for the Virtual Machines (VMs) in the specified scope (management group, subscription, or resource group). Takes Log Analytics workspace as a parameter. |Initiative |
|[Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Linux VMs |Deploy Dependency Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Windows VMs |Deploy Dependency Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Linux VMs |Deploy Log Analytics Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Windows VMs |Deploy Log Analytics Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |

Standalone policy (not included with the initiative) is described here:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Audit Log Analytics Workspace for VM - Report Mismatch |Report VMs as non-compliant if they aren't logging to the Log Analytics workspace specified in the policy/initiative assignment. |Policy |

#### Assign the Azure Monitor initiative
With this initial release, you can create the policy assignment only in the Azure portal. To understand how to complete these steps, see [Create a policy assignment from the Azure portal](../../governance/policy/assign-policy-portal.md).

1. To launch the Azure Policy service in the Azure portal, select **All services**, and then search for and select **Policy**.

1. In the left pane of the Azure Policy page, select **Assignments**.  
    An assignment is a policy that has been assigned to take place within a specific scope.
    
1. At the top of the **Policy - Assignments** page, select **Assign Initiative**.

1. On the **Assign Initiative** page, select the **Scope** by clicking the ellipsis (...), and select a management group or subscription.  
    In our example, a scope limits the policy assignment to a grouping of virtual machines for enforcement.
    
1. At the bottom of the **Scope** page, save your changes by selecting **Select**.

1. (Optional) To remove one or more resources from the scope, select **Exclusions**.

1. Select the **Initiative definition** ellipsis (...) to display the list of available definitions, select **[Preview] Enable Azure Monitor for VMs**, and then select **Select**.  
    The **Assignment name** box is automatically populated with the initiative name you selected, but you can change it. You can also add an optional description. The **Assigned by** box is automatically populated based on who is logged in, and this value is optional.
    
1. In the **Log Analytics workspace** drop-down list for the supported region, select a workspace.

    >[!NOTE]
    >If the workspace is beyond the scope of the assignment, grant *Log Analytics Contributor* permissions to the policy assignment's Principal ID. If you don't do this, you might see a deployment failure such as: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... `
    >To grant access, review [how to manually configure the managed identity](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
    >  
    The **Managed Identity** check box is selected, because the initiative being assigned includes a policy with the *deployIfNotExists* effect.
    
1. In the **Manage Identity location** drop-down list, select the appropriate region.

1. Select **Assign**.

#### Review and remediate the compliance results

You can learn how to review compliance results by reading [identify non-compliance results](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). In the left pane, select **Compliance**, and then locate the **[Preview] Enable Azure Monitor for VMs** initiative for VMs that aren't compliant according to the assignment you created.

![Policy compliance for Azure VMs](./media/vminsights-onboard/policy-view-compliance-01.png)

Based on the results of the policies included with the initiative, VMs are reported as non-compliant in the following scenarios:

* Log Analytics or the Dependency agent isn't deployed.  
    This scenario is typical for a scope with existing VMs. To mitigate it, deploy the required agents by [creating remediation tasks](../../governance/policy/how-to/remediate-resources.md) on a non-compliant policy.  
    - [Preview]: Deploy Dependency Agent for Linux VMs
    - [Preview]: Deploy Dependency Agent for Windows VMs
    - [Preview]: Deploy Log Analytics Agent for Linux VMs
    - [Preview]: Deploy Log Analytics Agent for Windows VMs

* VM Image (OS) isn't identified in the policy definition.  
    The criteria of the deployment policy include only VMs that are deployed from well-known Azure VM images. Check the documentation to see whether the VM OS is supported. If it isn't supported, duplicate the deployment policy and update or modify it to make the image compliant.  
    - [Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted
    - [Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted

* VMs aren't logging in to the specified Log Analytics workspace.  
    It's possible that some VMs in the initiative scope are logging in to a Log Analytics workspace other than the one that's specified in the policy assignment. This policy is a tool to identify which VMs are reporting to a non-compliant workspace.  
    - [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch

### Enable with PowerShell
To enable Azure Monitor for VMs for multiple VMs or virtual machine scale sets, you can use the PowerShell script [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), available from the Azure PowerShell Gallery. This script iterates through every virtual machine and virtual machine scale set in your subscription, in the scoped resource group that's specified by *ResourceGroup*, or to a single VM or virtual machine scale set that's specified by *Name*. For each VM or virtual machine scale set, the script verifies whether the VM extension is already installed. If the VM extension is not installed, the script tries to reinstall it. If the VM extension is installed, the script installs the Log Analytics and Dependency agent VM extensions.

This script requires Azure PowerShell module version 5.7.0 or later. Run `Get-Module -ListAvailable AzureRM` to find the version. If you need to upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). If you're running PowerShell locally, you also need to run `Connect-AzureRmAccount` to create a connection with Azure.

To get a list of the script's argument details and example usage, run `Get-Help`.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
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
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
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

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

En el ejemplo siguiente se muestra cómo utilizar los comandos de PowerShell en la carpeta para habilitar Azure Monitor para VM y comprender el resultado esperado:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

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

## <a name="enable-for-a-hybrid-environment"></a>Habilitación para el entorno híbrido
En esta sección se explica cómo implementar máquinas virtuales o equipos físicos hospedados en el centro de datos o en otros entornos en la nube de modo que Azure Monitor para VM los supervise.

La extensión Dependency Agent para Service Map de Azure Monitor para VM no transmite ningún dato y no requiere ningún cambio en firewalls o puertos. Los datos de Mapa siempre son transmitidos por el agente de Log Analytics al servicio de Azure Monitor, ya sea directamente o mediante la [Puerta de enlace de OMS](../../azure-monitor/platform/gateway.md), si las directivas de seguridad de TI no permiten que los equipos de la red se conecten a Internet.

Revise los requisitos y los métodos de implementación para el [Agente Linux y Windows de Log Analytics](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

A continuación se resumen los pasos:

1. Instalar el agente de Log Analytics para Windows o Linux.

1. Descargar e instalar Dependency Agent para Service Map de Azure Monitor para VM en [Windows](https://aka.ms/dependencyagentwindows) o [Linux](https://aka.ms/dependencyagentlinux).

1. Habilitar la recopilación de contadores de rendimiento.

1. Implementar Azure Monitor para VM.

### <a name="install-the-dependency-agent-on-windows"></a>Instalar Dependency Agent en Windows
Para instalar Dependency Agent manualmente en equipos Windows, puede ejecutar `InstallDependencyAgent-Windows.exe`. Si ejecuta este archivo ejecutable sin opciones, se inicia un asistente para instalación que puede seguir para realizar la instalación del agente de forma interactiva.

>[!NOTE]
>Se requieren privilegios de *administrador* para instalar o desinstalar al agente.

En la tabla siguiente se destacan los parámetros que admite el programa de instalación para el agente desde la línea de comandos.

| Parámetro | DESCRIPCIÓN |
|:--|:--|
| /? | Devuelve una lista de las opciones de la línea de comandos. |
| /S | Realiza una instalación silenciosa sin interacción del usuario. |

Por ejemplo, para ejecutar el programa de instalación con el parámetro `/?`, escriba **InstallDependencyAgent-Windows.exe /?**.

Los archivos de Dependency Agent para Windows se instalan en *C:\Archivos de programa\Microsoft Dependency Agent* de manera predeterminada. Si Dependency Agent no se inicia una vez completada la instalación, compruebe los registros para obtener información detallada del error. El directorio de registro es *%Programfiles%\Microsoft Dependency Agent\logs*.

### <a name="install-the-dependency-agent-on-linux"></a>Instalación de Dependency Agent en Linux
Dependency Agent se instala en los servidores Linux desde *InstallDependencyAgent-Linux64.bin*, un script de shell con un archivo binario autoextraíble. Puede ejecutar el archivo mediante `sh` o agregar permisos de ejecución al propio archivo.

>[!NOTE]
> Es necesario el acceso raíz para instalar o configurar el agente.
>

| Parámetro | DESCRIPCIÓN |
|:--|:--|
| -help | Obtenga una lista de las opciones de la línea de comandos. |
| -s | Realice una instalación silenciosa sin preguntas. |
| --check | Compruebe los permisos y el sistema operativo, pero no instale el agente. |

Por ejemplo, para ejecutar el programa de instalación con el parámetro `-help`, escriba **InstallDependencyAgent-Linux64.bin -help**.

Para instalar Dependency en Linux como raíz, ejecute el comando `sh InstallDependencyAgent-Linux64.bin`.

Si Dependency Agent no se inicia, compruebe los registros para obtener información detallada del error. En los agentes de Linux, el directorio de registro es */var/opt/microsoft/dependency-agent/log*.

Los archivos de Dependency Agent se colocan en los directorios siguientes:

| Archivos | Ubicación |
|:--|:--|
| Archivos principales | /opt/microsoft/dependency-agent |
| Archivos de registro | /var/opt/microsoft/dependency-agent/log |
| Archivos de configuración | /etc/opt/microsoft/dependency-agent/config |
| Archivos ejecutables de servicio | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Archivos binarios de almacenamiento | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Activar los contadores de rendimiento
Si el área de trabajo de Log Analytics a la que la solución hace referencia no está todavía configurada para recopilar los contadores de rendimiento que la solución requiere, tendrá que habilitarlos. Puede hacerlo de dos maneras:
* Manualmente, según se describe en [Orígenes de datos de rendimiento de Windows y Linux en Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Descargando y ejecutando un script de PowerShell que está disponible en [Galería de Azure PowerShell](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="deploy-azure-monitor-for-vms"></a>Implementar Azure Monitor para VM
Este método incluye una plantilla JSON que especifica la configuración para habilitar los componentes de la solución en el área de trabajo de Log Analytics.

Si no sabe cómo implementar los recursos con una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md)

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
    }
    ```

1. Guarde este archivo como *installsolutionsforvminsights.json* en una carpeta local.

1. Edite los valores de *WorkspaceName*, *ResourceGroupName* y *WorkspaceLocation*. El valor de *WorkspaceName* es el identificador de recurso completo del área de trabajo de Log Analytics e incluye el nombre del área de trabajo. El valor de *WorkspaceLocation* es la región en la que el área de trabajo está definida.

1. Ya puede implementar una plantilla mediante el siguiente comando de PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

    ```powershell
    provisioningState       : Succeeded
    ```
Después de habilitar la supervisión, pueden pasar unos 10 minutos hasta que pueda ver la métrica y el estado del equipo híbrido.

## <a name="performance-counters-enabled"></a>Contadores de rendimiento habilitados
Azure Monitor para VM configura un área de trabajo de Log Analytics para recopilar los contadores de rendimiento que la solución usa. En la tabla siguiente se enumeran los objetos y contadores que configura la solución que se recopilan cada 60 segundos.

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
|LogicalDisk |  Bytes de escritura en disco/s |
|LogicalDisk | Escrituras en disco/s |
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
|Disco lógico |  Bytes de escritura en disco/s |
|Disco lógico | Escrituras en disco/s |
|Disco lógico |Megabytes libres |
|Disco lógico |Bytes de disco lógico/s |
|Memoria |MB de memoria disponibles |
|Red |Número total de bytes recibidos |
|Red |Número total de bytes transmitidos |
|Procesador |% de tiempo de procesador |

## <a name="diagnostic-and-usage-data"></a>Datos de diagnóstico y uso
Microsoft recopila automáticamente datos de uso y rendimiento a través del servicio Azure Monitor. Microsoft usa estos datos para proporcionar y mejorar la calidad, la seguridad y la integridad del servicio. Con el fin de proporcionar funcionalidades de solución de problemas precisas y eficientes, los datos de la característica Mapa incluyen información sobre la configuración del software, como el sistema operativo y la versión, la dirección IP, el nombre DNS y el nombre de la estación de trabajo. Microsoft no recopila nombres, direcciones ni otra información de contacto.

Para más información sobre el uso y la recopilación de datos, vea la [Declaración de privacidad de Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Pasos siguientes

Ahora que la supervisión está habilitada para la máquina virtual, esta información está disponible para analizarse con Azure Monitor para VM. Para obtener información sobre cómo usar la característica de mantenimiento, consulte [Descripción del estado de las máquinas virtuales de Azure con Azure Monitor para VM (versión preliminar)](vminsights-health.md). Para ver las dependencias de las aplicaciones detectadas, consulte [Uso de la asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md).
