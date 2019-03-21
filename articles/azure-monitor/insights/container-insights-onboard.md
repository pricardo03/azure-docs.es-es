---
title: Cómo incorporar Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo incorporar y configurar Azure Monitor para contenedores, de forma que pueda conocer el rendimiento de su contenedor y qué problemas relacionados con su rendimiento se han identificado.
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
ms.date: 02/28/2019
ms.author: magoedte
ms.openlocfilehash: 591624e6bab07bfa06799d8e4817622e7a5c280a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58107651"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Cómo incorporar Azure Monitor para contenedores  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En este artículo se describe cómo configurar la solución para contenedores de Azure Monitor para supervisar el rendimiento de las cargas de trabajo que se implementan en entornos de Kubernetes y se hospedan en [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

Azure Monitor para contenedores puede habilitarse para implementaciones de AKS nuevas, o en una o más existentes, mediante los siguientes métodos compatibles:

* Desde Azure Portal o con la CLI de Azure
* Usando [Terraform y AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

## <a name="prerequisites"></a>Requisitos previos 
Antes de empezar, asegúrese de que dispone de lo siguiente:

- **Un área de trabajo de Log Analytics.** Puede crearla al habilitar la supervisión de su nuevo clúster de AKS o dejar que la experiencia de incorporación cree un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster de AKS. Si opta por crear el área de trabajo usted mismo, puede usar [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), mediante [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md).
- Es un **miembro del rol de colaborador de Log Analytics** para habilitar la supervisión del contenedor. Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../../azure-monitor/platform/manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Componentes 

La capacidad de supervisar el rendimiento se basa en un agente de Log Analytics en contenedores para Linux, desarrollado específicamente para Azure Monitor para contenedores. Este agente especializado recopila datos de rendimiento y de eventos de todos los nodos del clúster, y el agente se implementa y registra automáticamente en el área de trabajo de Log Analytics especificada durante la implementación. La versión del agente es microsoft/oms:ciprod04202018 o posterior y se representa mediante una fecha en el siguiente formato: *mmddaaaa*. 

Cuando se publica una nueva versión del agente, se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS). Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). 

>[!NOTE] 
>Si ya ha implementado un clúster de AKS, puede habilitar la supervisión mediante la CLI de Azure o una plantilla de Azure Resource Manager que se proporciona, como se muestra más adelante en este artículo. No puede usar `kubectl` para actualizar, eliminar, volver a implementar o implementar el agente. La plantilla debe implementarse en el mismo grupo de recursos que el clúster.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en el [Azure Portal](https://portal.azure.com). 

## <a name="enable-monitoring-for-a-new-cluster"></a>Habilitación de la supervisión para un clúster nuevo
Durante la implementación, puede habilitar la supervisión de un nuevo clúster de AKS desde Azure Portal, con la CLI de Azure o con Terraform.  Siga los pasos descritos en el artículo de guía de inicio rápido [Implementación de un clúster de Azure Kubernetes Service (AKS)](../../aks/kubernetes-walkthrough-portal.md), si desea habilitarlo desde el portal. En la página **Supervisión**, en la opción **Habilitar supervisión**, seleccione **Sí** y, después, seleccione un área de trabajo de Log Analytics existente o cree una nueva. 

### <a name="enable-using-azure-cli"></a>Habilitación del uso de la CLI de Azure
Para habilitar la supervisión de un nuevo clúster de AKS creado con la CLI de Azure, siga el paso correspondiente del artículo de la guía de inicio rápido, en la sección [Creación de un clúster de AKS](../../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecutar la versión 2.0.43 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

### <a name="enable-using-terraform"></a>Habilitación con Terraform
Si va a [implementar un nuevo clúster de AKS mediante Terraform](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), especifique los argumentos necesarios en el perfil [para crear un área de trabajo de Log Analytics](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) si no eligió especificar uno existente. 

>[!NOTE]
>Si decide usar Terraform, debe ejecutar el proveedor de Azure RM para Terraform versión 1.17.0 o superior.

Para agregar Azure Monitor para contenedores al área de trabajo, consulte [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) y complete el perfil mediante la inclusión de la [ **addon_profile** ](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) y especifique **oms_agent**. 

Una vez que haya habilitado la supervisión y todas las tareas de configuración se hayan completado correctamente, puede supervisar el rendimiento de su clúster de cualquiera de estas dos maneras:

* Directamente en el clúster de AKS mediante la selección de **Health** (Mantenimiento) en el panel izquierdo.
* Si selecciona el icono de **Monitor Container insights** (Supervisar conclusiones de contenedores) en la página del clúster de AKS correspondiente al clúster seleccionado. En Azure Monitor, en el panel izquierdo, seleccione **Health** (Mantenimiento). 

  ![Opciones para la selección de Azure Monitor para contenedores en AKS](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver la métrica de estado del clúster. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Habilitación de la supervisión para clústeres administrados existentes
Puede habilitar la supervisión de un clúster de AKS que ya se haya implementado bien con la CLI de Azure, desde Azure Portal o con la plantilla de Azure Resource Manager proporcionada mediante el cmdlet `New-AzResourceGroupDeployment` de PowerShell. 

### <a name="enable-monitoring-using-azure-cli"></a>Habilitación de la supervisión mediante la CLI de Azure
El paso siguiente habilita la supervisión del clúster de AKS mediante la CLI de Azure. En este ejemplo, no es necesario que cree o especifique un área de trabajo. Este comando le simplifica el proceso al crear un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster de AKS, si aún no existe en la región.  El formato del área de trabajo predeterminada creada es similar al de *DefaultWorkspace-\<GUID>-\<Region>*.  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

La salida será similar a la siguiente:

```azurecli
provisioningState       : Succeeded
```

Si, en su lugar, prefiere realizar la integración con un área de trabajo existente, use el comando siguiente para especificar esa área de trabajo.

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG --workspace-resource-id <ExistingWorkspaceResourceID> 
```

La salida será similar a la siguiente:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-using-terraform"></a>Habilitación de la supervisión mediante Terraform
1. Agregue el perfil de complemento **oms_agent** al [recurso azurerm_kubernetes_cluster](https://www.terraform.io/docs/providers/azurerm/d/kubernetes_cluster.html#addon_profile) existente.

   ```
   addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
     }
   }
   ```

2. Agregue [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) siguiendo los pasos descritos en la documentación de Terraform.

### <a name="enable-monitoring-from-azure-monitor-in-the-portal"></a>Habilitar la supervisión de Azure Monitor en el portal 
Para habilitar la supervisión de un clúster de AKS en Azure Portal desde Azure Monitor, siga estos pasos:

1. En Azure Portal, seleccione **Monitor**. 
2. Seleccione **Contenedores** en la lista.
3. En la página **Supervisión - Contenedores**, seleccione **Non-monitored clusters** (Clústeres sin supervisión).
4. En la lista de clústeres sin supervisión, busque el contenedor y haga clic en **Habilitar**.   
5. En la página **Incorporación a Azure Monitor para contenedores**, si tiene un área de trabajo de Log Analytics ya existente en la misma suscripción que el clúster, selecciónela de la lista desplegable.  
    La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa el contenedor de AKS en la suscripción. 

    ![Habilitación de la supervisión de conclusiones de contenedores de AKS](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Si desea crear una área de trabajo de Log Analytics para almacenar los datos de supervisión del clúster, siga las instrucciones de [Creación de un área de trabajo de Log Analytics en Azure Portal](../../azure-monitor/learn/quick-create-workspace.md). Asegúrese de crear el área de trabajo en la misma suscripción en la que está implementado el contenedor de AKS. 
 
Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver la métrica de estado del clúster. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>Habilitación de la supervisión desde el clúster AKS en el portal
Para habilitar la supervisión de un contenedor de AKS en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione **Todos los servicios**. 
2. En la lista de recursos, empiece a escribir **Containers**.  
    La lista se filtra en función de lo que escriba. 
3. Seleccione **servicios de Kubernetes**.  

    ![El vínculo de Servicios de Kubernetes](./media/container-insights-onboard/portal-search-containers-01.png)

4. En la lista de contenedores, seleccione uno de ellos.
5. En la página de información general del contenedor, seleccione **Contenedores de Monitor**.  
6. En la página **Incorporación a Azure Monitor para contenedores**, si tiene un área de trabajo de Log Analytics ya existente en la misma suscripción que el clúster, selecciónela de la lista desplegable.  
    La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa el contenedor de AKS en la suscripción. 

    ![Habilitación de la supervisión del mantenimiento de contenedores de AKS](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Si desea crear una área de trabajo de Log Analytics para almacenar los datos de supervisión del clúster, siga las instrucciones de [Creación de un área de trabajo de Log Analytics en Azure Portal](../../azure-monitor/learn/quick-create-workspace.md). Asegúrese de crear el área de trabajo en la misma suscripción en la que está implementado el contenedor de AKS. 
 
Después de habilitar la supervisión pueden pasar unos 15 minutos hasta que pueda ver los datos operativos del clúster. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Habilitación de la supervisión mediante el uso de una plantilla de Azure Resource Manager
Este método incluye dos plantillas JSON. Una plantilla especifica la configuración para habilitar la supervisión y la otra contiene los valores de los parámetros que se configuran para especificar lo siguiente:

* El identificador del recurso del contenedor de AKS. 
* El grupo de recursos en que se implementa el clúster.
* El área de trabajo y la región de Log Analytics en la que se crea el área de trabajo. 

>[!NOTE]
>La plantilla debe implementarse en el mismo grupo de recursos que el clúster.
>

El área de trabajo de Log Analytics se debe crear manualmente. Para crear el área de trabajo, puede configurarla una mediante [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md).

Si no conoce el concepto de implementación de recursos mediante una plantilla, consulte:
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
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. Guarde este archivo como **existingClusterOnboarding.json** en una carpeta local.
3. Pegue la siguiente sintaxis de JSON en el archivo:

    ```json
    {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. Edite los valores de **aksResourceId** y **aksResourceLocation** con los valores que de la página de **introducción a AKS** del clúster de AKS. El valor de **workspaceResourceId** es el identificador de recurso completo del área de trabajo de Log Analytics e incluye el nombre del área de trabajo. Especifique también la ubicación del área de trabajo de **workspaceRegion**. 
5. Guarde este archivo como **existingClusterParam.json** en una carpeta local.
6. Está listo para implementar esta plantilla. 

   * Use los siguientes comandos de PowerShell en la carpeta que contenga la plantilla:

       ```powershell
       New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
       ```
       El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Para ejecutar el comando siguiente mediante la CLI de Azure:
    
       ```azurecli
       az login
       az account set --subscription "Subscription Name"
       az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
       ```

       El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

       ```azurecli
       provisioningState       : Succeeded
       ```
     Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver la métrica de estado del clúster. 

## <a name="verify-agent-and-solution-deployment"></a>Comprobar la implementación del agente y la solución
Con la versión del agente *06072018*, o cualquier versión posterior, puede comprobar que tanto el agente como la solución se han implementado correctamente. Con las versiones anteriores del agente, solo se puede comprobar la implementación del agente.

### <a name="agent-version-06072018-or-later"></a>Versión 06072018 del agente, o posterior
Ejecute el siguiente comando para comprobar que el agente se ha implementado correctamente. 

```
kubectl get ds omsagent --namespace=kube-system
```

Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Para comprobar la implementación de la solución, ejecute el siguiente comando:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Versión del agente anterior a 06072018

Para comprobar que la versión del agente de Log Analytics anterior a *06072018* se ha implementado correctamente, ejecute el comando siguiente:  

```
kubectl get ds omsagent --namespace=kube-system
```

Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Visualización de la configuración con la CLI
Use el comando `aks show` para obtener detalles como si la solución está habilitada o no, cuál es el valor del identificador de recursos del área de trabajo de Log Analytics y detalles de resumen acerca del clúster.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Transcurridos unos minutos, el comando se completa y devuelve información en formato JSON acerca de la solución.  Los resultados del comando deben mostrar el perfil de complemento de supervisión y son similares a la salida del ejemplo siguiente:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```
## <a name="next-steps"></a>Pasos siguientes

* Si experimenta problemas al intentar incorporar la solución, consulte la [guía de solución de problemas](container-insights-troubleshoot.md).

* Con la supervisión habilitada para capturar métricas de estado para los pods y nodos del clúster de AKS, estas métricas de estado están disponibles en Azure Portal. Para obtener información sobre cómo usar Azure Monitor para contenedores, consulte el artículo sobre cómo [ver el estado de Azure Kubernetes Service](container-insights-analyze.md).
