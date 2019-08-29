---
title: Supervisión de un clúster de Azure Kubernetes Service (AKS) implementado | Microsoft Docs
description: Obtenga información sobre cómo habilitar la supervisión de un clúster de Azure Kubernetes Service (AKS) con Azure Monitor para contenedores ya implementados en la suscripción.
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
ms.date: 08/19/2019
ms.author: magoedte
ms.openlocfilehash: 650729269370bfcd6608b82fc14c3306da1ed222
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624429"
---
# <a name="enable-monitoring-of-azure-kubernetes-service-aks-cluster-already-deployed"></a>Habilitar la supervisión de un clúster de Azure Kubernetes Service (AKS) ya implementado

En este artículo se describe cómo configurar Azure Monitor para contenedores para supervisar un clúster de Kubernetes administrado, hospedado en [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/), que ya se han implementado en la suscripción.

Puede habilitar la supervisión de un clúster de AKS que ya se haya implementado mediante uno de los métodos admitidos:

* CLI de Azure
* Terraform
* [Desde Azure Monitor](#enable-from-azure-monitor-in-the-portal) o [directamente desde un clúster de AKS](#enable-directly-from-aks-cluster-in-the-portal) en Azure Portal. 
* Con la [plantilla de Azure Resource Manager proporcionada](#enable-using-an-azure-resource-manager-template) con el cmdlet `New-AzResourceGroupDeployment` de Azure PowerShell o con la CLI de Azure. 

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com). 

## <a name="enable-using-azure-cli"></a>Habilitación del uso de la CLI de Azure

El paso siguiente habilita la supervisión del clúster de AKS mediante la CLI de Azure. En este ejemplo, no es necesario que cree o especifique un área de trabajo. Este comando le simplifica el proceso al crear un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster de AKS, si aún no existe en la región.  El formato del área de trabajo predeterminada creada es similar al de *DefaultWorkspace-\<GUID>-\<Region>* .  

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

## <a name="enable-using-terraform"></a>Habilitación con Terraform

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

## <a name="enable-from-azure-monitor-in-the-portal"></a>Habilitación desde Azure Monitor en el portal 

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

## <a name="enable-directly-from-aks-cluster-in-the-portal"></a>Habilitación directamente desde un clúster de AKS en el portal

Para habilitar la supervisión directamente desde un clúster de AKS en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione **Todos los servicios**. 

2. En la lista de recursos, empiece a escribir **Containers**.  La lista se filtra en función de lo que escriba. 

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

## <a name="enable-using-an-azure-resource-manager-template"></a>Habilitación mediante una plantilla de Azure Resource Manager

Este método incluye dos plantillas JSON. Una plantilla especifica la configuración para habilitar la supervisión y la otra contiene los valores de los parámetros que se configuran para especificar lo siguiente:

* El identificador del recurso del contenedor de AKS. 
* El grupo de recursos en que se implementa el clúster.

>[!NOTE]
>La plantilla debe implementarse en el mismo grupo de recursos que el clúster.
>

El área de trabajo de Log Analytics debe crearse antes de habilitar la supervisión con Azure PowerShell o la CLI. Para crear el área de trabajo, puede configurarla una mediante [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md).

Si no conoce el concepto de implementación de recursos mediante una plantilla, consulte:

* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)

* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecutar la versión 2.0.59 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-a-template"></a>Creación y ejecución de una plantilla

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
        "aksResourceTagValues": {
          "type": "object",
          "metadata": {
            "description": "Existing all tags on AKS Cluster Resource"
          }
        },
        "workspaceResourceId": {
          "type": "string",
          "metadata": {
            "description": "Azure Monitor Log Analytics Resource ID"
          }
        }
      },
      "resources": [
        {
          "name": "[split(parameters('aksResourceId'),'/')[8]]",
          "type": "Microsoft.ContainerService/managedClusters",
          "location": "[parameters('aksResourceLocation')]",
          "tags": "[parameters('aksResourceTagValues')]",
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
        "aksResourceTagValues": {
          "value": {
            "<existing-tag-name1>": "<existing-tag-value1>",
            "<existing-tag-name2>": "<existing-tag-value2>",
            "<existing-tag-nameN>": "<existing-tag-valueN>"
          }
        }
      }
    }
    ```

4. Edite los valores de **aksResourceId** y **aksResourceLocation** con los valores de la página **AKS Overview** (Introducción a AKS) del clúster de AKS. El valor de **workspaceResourceId** es el identificador de recurso completo del área de trabajo de Log Analytics e incluye el nombre del área de trabajo. 

    Edite los valores de **aksResourceTagValues** para que coincidan con los valores de etiqueta existentes especificados para el clúster de AKS.

5. Guarde este archivo como **existingClusterParam.json** en una carpeta local.

6. Está listo para implementar esta plantilla. 

   * Para implementar con Azure PowerShell, use los siguientes comandos en la carpeta que contiene la plantilla:

       ```powershell
       New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
       ```
       
       El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Para implementar con la CLI de Azure, ejecute los siguientes comandos:
    
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
