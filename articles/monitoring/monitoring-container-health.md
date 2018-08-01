---
title: Supervisión del mantenimiento de Azure Kubernetes Service (AKS) (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo puede revisar fácilmente el rendimiento del contenedor de AKS para comprender rápidamente el uso de su entorno hospedado de Kubernetes.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2018
ms.author: magoedte
ms.openlocfilehash: 806487ec731a1b7fe02ccdfe6b285f5b2e119787
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39249104"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Supervisión del mantenimiento de contenedores de Azure Kubernetes Service (AKS) (versión preliminar)

En este artículo se describe cómo configurar y usar la solución de mantenimiento de contenedores de Azure Monitor para supervisar el rendimiento de las cargas de trabajo que se implementan en entornos de Kubernetes y se hospedan en Azure Kubernetes Service (AKS). La supervisión de su clúster de Kubernetes y de los contenedores es fundamental, sobre todo cuando se ejecuta un clúster de producción, a escala, con varias aplicaciones.

La solución de mantenimiento de contenedores le brinda posibilidad de supervisar el rendimiento mediante la recopilación de métricas del procesador y de la memoria de los controladores, nodos y contenedores disponibles en Kubernetes mediante Metrics API. Tras habilitar la solución de mantenimiento de contenedores, estas métricas se recopilan automáticamente mediante una versión en contenedor del Agente de Operations Management Suite (OMS) para Linux y se almacenan en su área de trabajo de [Log Analytics](../log-analytics/log-analytics-overview.md). Las vistas predefinidas incluidas muestran las cargas de trabajo residentes en el contenedor y lo que afecta al mantenimiento del rendimiento del clúster Kubernetes, con el fin de que pueda:  

* Identificar los contenedores que se ejecutan en el nodo y su utilización media tanto del procesador como de la memoria. Este conocimiento puede ayudarle a identificar cuellos de botella en los recursos.
* Identificar dónde se encuentra el contenedor en un controlador o un pod. Este conocimiento puede ayudarle a ver el rendimiento general del controlador o del pod. 
* Revisar el uso de recursos de las cargas de trabajo que se ejecutan en el host que no estén relacionadas con los procesos estándar que admite el pod.
* Conocer el comportamiento del clúster tanto con cargas medias como con las más pesadas. Este conocimiento puede ayudarle a identificar los requisitos de capacidad y determinar la carga máxima que el clúster puede admitir. 

Si está interesado en la supervisión y la administración de hosts de contenedor de Docker y Windows y desea más información sobre la configuración, la auditoría y la utilización de recursos, consulte la [solución de contenedor](../log-analytics/log-analytics-containers.md).

## <a name="prerequisites"></a>Requisitos previos 
Antes de empezar, asegúrese de que dispone de lo siguiente:

- Un clúster de AKS nuevo o existente.
- Un Agente de OMS para Linux, versión microsoft / oms:ciprod04202018 o posterior. El número de versión se representa mediante una fecha con el siguiente formato: *mmddaaaa*. El agente se instala automáticamente durante la incorporación de la solución de mantenimiento de contenedores. 
- Un área de trabajo de Log Analytics. Puede crearlo al habilitar la supervisión del nuevo clúster de AKS, o bien mediante [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md).
- El rol de colaborador de Log Analytics para habilitar la supervisión de contenedores. Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Componentes 

La capacidad para supervisar el rendimiento se basa en un Agente de OMS para Linux en contenedor, que recopila los datos de los eventos y del rendimiento de todos los nodos del clúster. El agente se implementa y registra automáticamente con el área de trabajo de Log Analytics especificada después de habilitar la supervisión del contenedor. 

>[!NOTE] 
>Si ya ha implementado un clúster de AKS, puede habilitar la supervisión mediante una plantilla de Azure Resource Manager que se proporciona, como se muestra más adelante en este artículo. No puede usar `kubectl` para actualizar, eliminar, volver a implementar o implementar el agente. 
>

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en el [Azure Portal](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Habilitación de la supervisión del mantenimiento de contenedores para un nuevo clúster
Durante la implementación puede habilitar la supervisión de un nuevo clúster de AKS desde Azure Portal. Siga los pasos descritos en el artículo de inicio rápido [Implementación de un clúster de Azure Container Service (AKS)](../aks/kubernetes-walkthrough-portal.md). En la página **Supervisión**, en la opción **Habilitar supervisión**, seleccione **Sí** y, después, seleccione un área de trabajo de Log Analytics existente o cree una nueva. 

Una vez que haya habilitado la supervisión y todas las tareas de configuración se hayan completado correctamente, puede supervisar el rendimiento de su clúster de cualquiera de estas dos maneras:

* Directamente en el clúster de AKS mediante la selección de **Health** (Mantenimiento) en el panel izquierdo.
* Seleccionando el icono **Monitor container health** (Supervisar mantenimiento de contenedores) en la página del clúster de AKS correspondiente al clúster seleccionado. En Azure Monitor, en el panel izquierdo, seleccione **Health** (Mantenimiento). 

  ![Opciones para seleccionar el mantenimiento de contenedores en AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Después de habilitar la supervisión pueden pasar unos 15 minutos hasta que pueda ver los datos operativos del clúster. 

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Habilitación de la supervisión del mantenimiento de contenedores para clústeres administrados existentes
Puede habilitar la supervisión de un clúster de AKS que ya se haya implementado en Azure Portal o con la plantilla de Azure Resource Manager proporcionada mediante el cmdlet `New-AzureRmResourceGroupDeployment` de PowerShell o la CLI de Azure. 

### <a name="enable-monitoring-in-the-azure-portal"></a>Habilitación de la supervisión en Azure Portal
Para habilitar la supervisión de un contenedor de AKS en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione **Todos los servicios**. 
2. En la lista de recursos, empiece a escribir **Containers**.  
    La lista se filtra en función de lo que escriba. 
3. Seleccione **Servicios de Kubernetes**.  

    ![El vínculo de Servicios de Kubernetes](./media/monitoring-container-health/azure-portal-01.png)

4. En la lista de contenedores, seleccione uno de ellos.
5. En la página de información general del contenedor, seleccione **Estado del contenedor de Monitor**.  
6. En la página **Onboarding to Container Health and Logs** (Incorporación a los registros y al mantenimiento de contenedores), si tiene un área de trabajo de Log Analytics existente en la misma suscripción que el clúster, selecciónela en la lista desplegable.  
    La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa el contenedor de AKS en la suscripción. 

    ![Habilitación de la supervisión del mantenimiento de contenedores de AKS](./media/monitoring-container-health/container-health-enable-brownfield-02.png)

>[!NOTE]
>Si desea crear una área de trabajo de Log Analytics para almacenar los datos de supervisión del clúster, siga las instrucciones de [Creación de un área de trabajo de Log Analytics en Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md). Asegúrese de crear el área de trabajo en la misma suscripción en la que está implementado el contenedor de AKS. 
 
Después de habilitar la supervisión pueden pasar unos 15 minutos hasta que pueda ver los datos operativos del clúster. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Habilitación de la supervisión mediante el uso de una plantilla de Azure Resource Manager
Este método incluye dos plantillas JSON. Una plantilla especifica la configuración para habilitar la supervisión y la otra contiene los valores de los parámetros que se configuran para especificar lo siguiente:

* El identificador del recurso del contenedor de AKS. 
* El grupo de recursos en que se implementa el clúster.
* El área de trabajo y la región de Log Analytics en la que se crea el área de trabajo. 

El área de trabajo de Log Analytics se debe crear manualmente. Para crear el área de trabajo, puede configurarla una mediante [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md).

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
        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
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
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscroptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "East US"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "eastus"
       }
     }
    }
    ```

4. Edite los valores de **aksResourceId** y **aksResourceLocation** con los valores que de la página de **introducción a AKS** del clúster de AKS. El valor de **workspaceResourceId** es el identificador de recurso completo del área de trabajo de Log Analytics e incluye el nombre del área de trabajo. Especifique también la ubicación del área de trabajo de **workspaceRegion**. 
5. Guarde este archivo como **existingClusterParam.json** en una carpeta local.
6. Está listo para implementar esta plantilla. 

    * Use los siguientes comandos de PowerShell en la carpeta que contenga la plantilla:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Para ejecutar el comando siguientes mediante la CLI de Azure en Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        El cambio de configuración puede tardar unos minutos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

        ```azurecli
        provisioningState       : Succeeded
        ```
Después de habilitar la supervisión pueden pasar unos 15 minutos hasta que pueda ver los datos operativos del clúster. 

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

Para comprobar que la versión del agente de OMS anterior a *06072018* se ha implementado correctamente, ejecute el comando siguiente:  

```
kubectl get ds omsagent --namespace=kube-system
```

Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Ver el uso del rendimiento
Cuando se abre el mantenimiento de contenedores, la página presenta inmediatamente el uso del rendimiento de todo el clúster. La vista de información sobre el clúster de AKS está organizada en cuatro perspectivas:

- Clúster
- Nodos 
- Controladores  
- Contenedores

En la pestaña **Clúster**, los cuatro gráficos de rendimiento de línea muestran las métricas de rendimiento clave del clúster. 

![Gráficos de rendimiento de ejemplo de la pestaña Clúster](./media/monitoring-container-health/container-health-cluster-perfview.png)

El gráfico del rendimiento muestra cuatro métricas de rendimiento:

- **Node CPU Utilization&nbsp;%** (Porcentaje de uso de CPU del nodo): una perspectiva agregada del uso de la CPU para todo el clúster. Puede filtrar los resultados para el intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles situado encima del gráfico, ya sea individualmente o de forma combinada. 
- **Node memory utilization&nbsp;%** (Porcentaje de uso de la memoria del nodo): una perspectiva agregada del uso de la memoria para todo el clúster. Puede filtrar los resultados para el intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles situado encima del gráfico, ya sea individualmente o de forma combinada. 
- **Node count** (Número de nodos): número y estado de los nodos de Kubernetes. Los estados de los nodos del clúster representados son *Todo*, *Listo* y  *No está listo*, y se pueden filtrar de forma individual o combinada en el selector situado encima del gráfico. 
- **Activity pod count** (Número de pods de actividad): recuento y el estado de los pods de Kubernetes. Los estados de los pods representados son *Todo*, *Pendientes*, *En ejecución* y *Desconocidos*, y se pueden filtrar de forma individual o combinada en el selector situado encima del gráfico. 

Si cambia a la pestaña **Nodos**, la jerarquía de filas sigue el modelo de objetos de Kubernetes a partir de un nodo del clúster. Expanda el nodo y podrá ver uno, o varios, pods que se ejecutan en el nodo. Si hay más de un contenedor agrupado en un pod, se muestran como la última fila de la jerarquía. También puede ver cuántas cargas de trabajo no relacionadas con pods se ejecutan en el host si el host tiene presión de memoria o procesador.

![Ejemplo de jerarquía de nodos de Kubernetes en la vista de rendimiento](./media/monitoring-container-health/container-health-nodes-view.png)

Puede seleccionar los controladores o los contenedores en la parte superior de la página y revisar el estado y el uso de recursos de dichos objetos. Use los cuadros de lista desplegable de la parte superior para filtrar por espacio de nombres, servicio y nodo. Si prefiere revisar el uso de memoria, en la lista desplegable **Metric** (Métrica), seleccione **Memory RSS** (RSS de memoria) o **Memory working set** (Espacio de trabajo de memoria). **RSS de memoria** solo se admite en Kubernetes versión 1.8, y en las versiones posteriores. En caso contrario, verá los valores de **Min&nbsp;%** (Porcentaje mínimo) como *NaN&nbsp;%* (Porcentaje de NaN), que es un valor de tipo de datos numérico que representa un valor no definido o no representable. 

![Vista de rendimiento de los nodos del contenedor](./media/monitoring-container-health/container-health-node-metric-dropdown.png)

De forma predeterminada, los datos de rendimiento se basan en las últimas seis horas, pero puede cambiar la ventana con la lista desplegable **Time Range** (Intervalo de tiempo) de la parte superior derecha. En este momento, la página no se actualiza automáticamente, sino que debe hacerlo manualmente. También puede filtrar los resultados del intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles. 

![Selección de percentil para el filtrado de datos](./media/monitoring-container-health/container-health-metric-percentile-filter.png)

En el ejemplo siguiente, observa en el nodo *aks-nodepool-3977305* que el valor de **Containers** (Contenedores) es 5, que es un resumen del número total de contenedores implementados.

![Ejemplo de resumen de contenedores por nodo](./media/monitoring-container-health/container-health-nodes-containerstotal.png)

Este resumen puede ayudarle a identificar rápidamente si tiene el equilibrio adecuado de contenedores entre los nodos del clúster. 

La información que se presenta al ver los nodos se describe en la siguiente tabla:

| Columna | Descripción | 
|--------|-------------|
| Nombre | El nombre del host. |
| Status | Vista de Kubernetes del estado del nodo. |
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | Porcentaje medio de nodos basado en el percentil durante la duración seleccionada. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90) | Valor real de promedio de nodos basado en el percentil para la duración seleccionada. El valor medio se mide desde el límite de CPU/memoria establecido para un nodo; en el caso de los pods y contenedores, es el valor medio notificado por el host. |
| Contenedores | Número de contenedores. |
| Tiempo de actividad | Representa la hora desde un nodo iniciado o que se reinició. |
| Controladores | Solo para los contenedores y pods. Muestra el controlador en que reside. No todos los pods están en un controlador, así que algunos pueden mostrar **N/D**. | 
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia | Tendencia de gráfico de barras que presenta el porcentaje de métricas de percentil del controlador. |


En el selector, elija **Controllers** (Controladores).

![Selección de la vista de controladores](./media/monitoring-container-health/container-health-controllers-tab.png)

Aquí puede ver el mantenimiento del rendimiento de los controladores.

![Vista de rendimiento de los controladores <nombre>](./media/monitoring-container-health/container-health-controllers-view.png)

La jerarquía de filas comienza con un controlador y expande el controlador. Se ven uno o varios contenedores. Expanda un pod, en la última fila se muestra el contenedor agrupado al pod. 

La información que se muestra al ver los controladores se describe en la siguiente tabla:

| Columna | Descripción | 
|--------|-------------|
| Nombre | El nombre del controlador.|
| Status | El estado del resumen de los contenedores cuando ha terminado de ejecutarse con un estado, como *OK (Correcto)*, *Terminated (Finalizado)*, *Failed (Error)* *Stopped (Detenido)* o *Paused (En pausa)*. Si el contenedor se está ejecutando, pero el estado no se mostró correctamente o el agente no lo seleccionó, y no ha respondido durante más de 30 minutos, el estado es *Unknown* (Desconocido). En la tabla siguiente se proporcionan detalles adicionales del icono de estado.|
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | Promedio resumen del porcentaje medio de cada entidad para la métrica y el percentil seleccionados. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90)  | Resumen del rendimiento medio de memoria o CPU de millares de núcleos del contenedor para el percentil seleccionado. El valor medio se mide a partir del límite de CPU o memoria establecido para un pod. |
| Contenedores | Número total de contenedores para el controlador o pod. |
| Reinicios | Resumen del número de reinicios de los contenedores. |
| Tiempo de actividad | Representa el tiempo desde que se inició un contenedor. |
| Nodo | Solo para los contenedores y pods. Muestra el controlador en el que reside. | 
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia| Tendencia de gráfico de barras que representa las métricas de percentil del controlador. |

Los iconos del campo de estado indican el estado en línea de los contenedores:
 
| Icono | Status | 
|--------|-------------|
| ![Icono de estado de ejecución listo](./media/monitoring-container-health/container-health-ready-icon.png) | En ejecución (Listo)|
| ![Icono de estado de espera o en pausa](./media/monitoring-container-health/container-health-waiting-icon.png) | En pausa o en espera|
| ![Icono de último estado de ejecución notificado](./media/monitoring-container-health/container-health-grey-icon.png) | Última notificación: en ejecución (pero no ha respondido en más de 30 minutos)|
| ![Icono de estado correcto](./media/monitoring-container-health/container-health-green-icon.png) | Se detuvo correctamente o no se pudo detener|

El icono de estado muestra un número en función de lo que proporciona el pod. Muestra los dos peores estados y, al pasar el ratón sobre el estado, muestra un estado de resumen de todos los pods del contenedor. Si no hay un estado listo, el valor de estado muestra **(0)**. 

En el selector, elija **Containers** (Contenedores).

![Selección de la vista de contenedores](./media/monitoring-container-health/container-health-containers-tab.png)

Aquí puede ver el mantenimiento del rendimiento de los contenedores.

![Vista de rendimiento de los controladores <nombre>](./media/monitoring-container-health/container-health-containers-view.png)

La información que se muestra al ver los contenedores se describe en la siguiente tabla:

| Columna | Descripción | 
|--------|-------------|
| Nombre | El nombre del controlador.|
| Status | Estado de los contenedores, si lo hay. En la tabla siguiente se proporcionan detalles adicionales del icono de estado.|
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | El resumen del porcentaje medio de cada entidad para la métrica y el percentil seleccionados. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90)  | El resumen del rendimiento medio de memoria o CPU de millares de núcleos del contenedor para el percentil seleccionado. El valor medio se mide a partir del límite de CPU o memoria establecido para un pod. |
| Pod | Contenedor en el que reside el pod.| 
| Nodo |  Nodo donde reside el contenedor. | 
| Reinicios | Representa el tiempo desde que se inició un contenedor. |
| Tiempo de actividad | Representa el tiempo desde que se inicia o reinicia un contenedor. |
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia | Una tendencia del gráfico de barras que representa el porcentaje medio de métrica del contenedor. |

Los iconos en el campo de estado indican los estados en línea de los pods, tal como se describen en la tabla siguiente:
 
| Icono | Status | 
|--------|-------------|
| ![Icono de estado de ejecución listo](./media/monitoring-container-health/container-health-ready-icon.png) | En ejecución (Listo)|
| ![Icono de estado de espera o en pausa](./media/monitoring-container-health/container-health-waiting-icon.png) | En pausa o en espera|
| ![Icono de último estado de ejecución notificado](./media/monitoring-container-health/container-health-grey-icon.png) | Última notificación: en ejecución, pero no ha respondido en más de 30 minutos|
| ![Icono de estado finalizado](./media/monitoring-container-health/container-health-terminated-icon.png) | Se detuvo correctamente o no se pudo detener|
| ![Icono de estado de error](./media/monitoring-container-health/container-health-failed-icon.png) | Estado de error |

## <a name="container-data-collection-details"></a>Información detallada sobre la recopilación de datos del contenedor
La solución de mantenimiento de contenedores recopila diversas métricas de rendimiento y datos de registro de los hosts de contenedor y los contenedores. Los datos se recopilan cada tres minutos.

### <a name="container-records"></a>Registros de contenedor

En la siguiente tabla se muestran ejemplos de registros recopilados por la solución de mantenimiento de contenedores y los tipos de datos que aparecen en los resultados de las búsquedas de registros:

| Tipo de datos | Tipo de datos en la búsqueda de registros | Fields |
| --- | --- | --- |
| Rendimiento de hosts y contenedores | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventario de contenedor | `ContainerInventory` | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventario de imágenes de contenedor | `ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Registro de contenedor | `ContainerLog` | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Registro del servicio de contenedores | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |
| Inventario de nodo de contenedor | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Proceso del contenedor | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventario de pods en un clúster de Kubernetes | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventario de la parte de nodos de un clúster de Kubernetes | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, SourceSystem | 
| Servicios en el clúster de Kubernetes | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Métricas de rendimiento de la parte de nodos del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;cpuUsageNanoCores, , memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;,CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de rendimiento de la parte de contenedores del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SContainer” | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;,CounterValue, TimeGenerated, CounterPath, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Búsqueda de registros para analizar datos
Log Analytics puede ayudarle a buscar tendencias, diagnosticar cuellos de botellas, realizar previsiones o correlacionar datos, que pueden servirle para determinar si la configuración actual del clúster funciona óptimamente. Las búsquedas de registros predefinidas se proporcionan para que comience a usarlas inmediatamente o para que las personalice con el fin de devolver la información de la forma deseada. 

Para realizar una análisis interactivo de los datos del área de trabajo, seleccione la opción **View Log** (Ver registro), disponible en el extremo derecho al expandir un controlador o contenedor. La página **Búsqueda de registros** aparece encima de la página de Azure Portal en que se encontraba.

![Análisis de los datos en Log Analytics](./media/monitoring-container-health/container-health-view-logs.png)   

La salida de los registros del contenedor que se reenvía a Log Analytics es STDOUT y STDERR. Como la solución de mantenimiento de contenedores supervisa Azure Kubernetes Service (AKS), el sistema de Kubernetes no se recopila hoy debido al gran volumen de datos generados. 

### <a name="example-log-search-queries"></a>Ejemplos de consultas de búsqueda de registros
A menudo resulta útil crear consultas que comiencen con un ejemplo o dos y luego modificarlas para ajustarlas a sus requisitos. Para ayudarle a crear consultas más avanzadas, puede experimentar con las siguientes consultas de ejemplo:

| Consultar | Descripción | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Se muestra toda la información del ciclo de vida de un contenedor| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Inventario de imágenes | 
| **En Análisis avanzado, seleccione gráficos de líneas**:<br> Perf<br> &#124; where ObjectName == "Container" and CounterName == "% Processor Time"<br> &#124; summarize AvgCPUPercent = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | CPU de contenedor | 
| **En Análisis avanzado, seleccione gráficos de líneas**:<br> Perf &#124; where ObjectName == "Container" and CounterName == "Memory Usage MB"<br> &#124; summarize AvgUsedMemory = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Memoria de contenedor |

## <a name="how-to-stop-monitoring-with-container-health"></a>Cómo detener la supervisión con la solución de mantenimiento de contenedor
Si después de habilitar la supervisión del contenedor de AKS decide que ya no desea supervisarlo, puede *descartarla* mediante las plantillas de Azure Resource Manager que proporcionan con el cmdlet **New-AzureRmResourceGroupDeployment** de PowerShell o la CLI de Azure. Una plantilla JSON especifica la configuración que se *descarta*. La otro contiene los valores de parámetro que se configuran para especificar el grupo de recursos y el identificador de recursos de AKS en que se implementa el clúster. 

Si no conoce el concepto de implementación de recursos mediante una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md)

Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-template"></a>Creación y ejecución de la plantilla

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
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Guarde este archivo como **OptOutTemplate.json** en una carpeta local.
3. Pegue la siguiente sintaxis de JSON en el archivo:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Edite los valores de **aksResourceId** y **aksResourceLocation** usando los valores del clúster de AKS, que puede encontrar en la página **Propiedades** del clúster seleccionado.

    ![Página de propiedades del contenedor](./media/monitoring-container-health/container-properties-page.png)

    Mientras está en la página de **propiedades**, copie también el **identificador de recurso del área de trabajo**. Este valor es necesario si decide que desea eliminar el área de trabajo de Log Analytics más adelante. La eliminación del área de trabajo de Log Analytics no se realiza como parte de este proceso. 

5. Guarde este archivo como **OptOutParam.json** en una carpeta local.
6. Está listo para implementar esta plantilla. 

    * Para usar los siguientes comandos de PowerShell en la carpeta que contiene la plantilla:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        El cambio de configuración puede tardar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Para ejecutar el comando siguiente con la CLI de Azure en Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        El cambio de configuración puede tardar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Si el área de trabajo se creó solamente para admitir la supervisión del clúster y ya no es necesario, tiene que eliminarlo manualmente. Si no está familiarizado con la eliminación de un área de trabajo, consulte [Eliminación de una área de trabajo de Azure Log Analytics con Azure Portal](../log-analytics/log-analytics-manage-del-workspace.md). No se olvide del **identificador de recurso del área de trabajo** que copió anteriormente en el paso 4, lo necesitará. 

## <a name="troubleshooting"></a>solución de problemas
En esta sección se proporciona información para ayudarle a solucionar problemas con el mantenimiento de contenedores.

Si el mantenimiento de contenedores se ha habilitado y configurado correctamente, pero no ve información acerca del estado ni resultados en Log Analytics al realizar una búsqueda de registros, la realización de las siguientes acciones puede ayudarle a diagnosticar el problema: 

1. Compruebe el estado del agente ejecutando el comando siguiente: 

    `kubectl get ds omsagent --namespace=kube-system`

    Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Compruebe el estado de implementación de la solución con la versión *06072018* del agente, o cualquier versión posterior, mediante la ejecución del comando siguiente:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Compruebe que se ejecuta el pod, para lo que se ejecuta el comando siguiente: `kubectl get pods --namespace=kube-system`

    La salida debe recordar a la siguiente con un estado de *Running* (En ejecución) en omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Compruebe los registros del agente. Cuando se implementa el agente en contenedor, se ejecuta una comprobación rápida mediante la ejecución de comandos de OMI y se muestra la versión del agente y el proveedor. 

5. Para comprobar que el agente se ha incorporado correctamente, ejecute el siguiente comando: `kubectl logs omsagent-484hw --namespace=kube-system`

    La salida debe ser similar a la siguiente:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Pasos siguientes

Para ver información detallada acerca del rendimiento de la aplicación y del mantenimiento del contenedor, consulte [Registros de búsqueda](../log-analytics/log-analytics-log-search.md). 
