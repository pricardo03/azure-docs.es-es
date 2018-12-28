---
title: Administración de Azure Monitor para el agente de contenedores | Microsoft Docs
description: En este artículo se describe cómo administrar las tareas de mantenimiento más comunes con el agente de Log Analytics en contenedores que usa Azure Monitor para contenedores.
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
ms.date: 12/06/2018
ms.author: magoedte
ms.openlocfilehash: 566ab8d14ebce04a2cba208dd72efc3782d5ad41
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256305"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Administración de Azure Monitor para el agente de contenedores
Azure Monitor para contenedores usa una versión con contenedores del agente de Log Analytics para Linux. Después de la implementación inicial, hay tareas rutinarias u opcionales que debe hacer durante su ciclo de vida. En este artículo se detallan los procedimientos para actualizar el agente manualmente y deshabilitar la recopilación de variables de entorno desde un contenedor determinado. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Actualización de Azure Monitor para el agente de contenedores
Azure Monitor para contenedores usa una versión con contenedores del agente de Log Analytics para Linux. Cuando se publica una nueva versión del agente, este se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS).  

Si se produce un error en la actualización del agente, en este artículo se describe el proceso para actualizar manualmente el agente. Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

### <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Actualización del agente en un clúster de Kubernetes supervisado
El proceso para actualizar al agente consta de dos pasos sencillos. El primer paso es deshabilitar la supervisión con Azure Monitor para contenedores mediante la CLI de Azure.  Siga los pasos descritos en el artículo [Deshabilitar la supervisión](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli). El uso de la CLI de Azure nos permite quitar el agente de los nodos del clúster sin afectar a la solución ni a los datos correspondientes que se almacenan en el área de trabajo. 

>[!NOTE]
>Mientras está realizando esta actividad de mantenimiento, los nodos del clúster no reenviarán los datos recopilados y las vistas de rendimiento no mostrarán datos en el período comprendido entre la eliminación del agente y la instalación de la nueva versión. 
>

Para instalar la nueva versión del agente, siga los pasos descritos en el artículo [Incorporación de la supervisión](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) usando la CLI de Azure para completar este proceso.  

Después de volver a habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver actualizada la métrica de estado del clúster. Para verificar que el agente se ha actualizado correctamente, ejecute el comando: `kubectl logs omsagent-484hw --namespace=kube-system`

El estado debe parecerse al ejemplo siguiente, donde los valores de *omi* y *omsagent* deben coincidir con la versión más reciente especificada en el [historial de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

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
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Cómo deshabilitar la recopilación de variables de entorno en un contenedor
Azure Monitor para contenedores recopila las variables de entorno de los contenedores que se ejecutan en un pod y las presenta en el panel de propiedades del contenedor seleccionado en la vista **Contenedores**. Puede controlar este comportamiento si deshabilita la recopilación de un contenedor específico, ya sea durante la implementación del clúster de AKS, o después, al establecer la variable de entorno *AZMON_COLLECT_ENV*. Esta característica está disponible en la versión del agente: ciprod11292018 y posterior.  

Para deshabilitar la recopilación de variables de entorno en un contenedor nuevo o existente, establezca la variable **AZMON_COLLECT_ENV** con un valor de **False** en el archivo de configuración yaml de implementación de Kubernetes.   

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

Ejecute el siguiente comando para aplicar el cambio en el contenedor de AKS: `kubectl apply -f  <path to yaml file>`.

Para verificar que el cambio de configuración surta efecto, seleccione un contenedor en la vista **Contenedores** en Azure Monitor para contenedores y, en el panel de propiedades, expanda **Variables de entorno**.  La sección debe mostrar solo la variable que creó anteriormente: **AZMON_COLLECT_EN=FALSE**. Para todos los otros contenedores, la sección Variables de entorno debe enumerar todas las variables de entorno detectadas.   

Para volver a habilitar la detección de las variables de entorno, aplique el mismo proceso anterior y cambie el valor de **False** a **True** y, luego, vuelva a ejecutar el comando `kubectl` para actualizar el contenedor.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Pasos siguientes
Si experimenta problemas al actualizar el agente, consulte la [guía de solución de problemas](container-insights-troubleshoot.md) para obtener ayuda.