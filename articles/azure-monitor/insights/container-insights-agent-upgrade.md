---
title: Actualización de Azure Monitor para el agente de contenedores (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo actualizar al agente de Log Analytics que usa Azure Monitor para los contenedores.
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
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 08206b9de4fca3b422c5b076d7e0c1c180f82fbd
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184095"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Actualización de Azure Monitor para el agente de contenedores (versión preliminar)
Azure Monitor para contenedores usa una versión con contenedores del agente de Log Analytics para Linux. Cuando se publica una nueva versión del agente, este se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS).  

Si se produce un error en la actualización del agente, en este artículo se describe el proceso para actualizar manualmente el agente. Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Actualización del agente en un clúster de Kubernetes supervisado
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

## <a name="next-steps"></a>Pasos siguientes
Si experimenta problemas al actualizar el agente, consulte la [guía de solución de problemas](container-insights-troubleshoot.md) para obtener ayuda.