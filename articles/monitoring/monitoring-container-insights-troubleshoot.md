---
title: Cómo solucionar problemas de Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo se pueden detectar y solucionar problemas con Azure Monitor para contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: de7ae5788224b83105e4dc9a24aea35c8b841c88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986734"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Solución de problemas de Azure Monitor para contenedores

Al configurar la supervisión del clúster de Azure Kubernetes Service (AKS) con Azure Monitor para contenedores, puede encontrar un problema que impida la recopilación de datos o el informe del estado. En este artículo se detallan algunos problemas comunes y los pasos para solucionarlos.

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Azure Monitor para contenedores está habilitado, pero no proporciona ninguna información
Si Azure Monitor para contenedores está habilitado y configurado correctamente, pero no puede ver la información de estado o no se devuelve ningún resultado de una consulta de registros de Log Analytics, siga estos pasos para diagnosticar el problema: 

1. Ejecute este comando para comprobar el estado del agente: 

    `kubectl get ds omsagent --namespace=kube-system`

    Si la salida se parece a la siguiente, la implementación se ha realizado correctamente:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Compruebe el estado de implementación con la versión del agente *06072018* o posterior con el comando:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Si la salida se parece al ejemplo siguiente, la implementación se ha realizado correctamente:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Con el comando siguiente, consulte el estado del pod para comprobar que se está ejecutando: `kubectl get pods --namespace=kube-system`

    La salida debe parecerse al ejemplo siguiente con un estado de *Running* (En ejecución) en omsagent:

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

5. Para comprobar que el agente se ha incorporado correctamente, ejecute el comando: `kubectl logs omsagent-484hw --namespace=kube-system`

    El estado debe ser similar al ejemplo siguiente:

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
Con la supervisión habilitada para capturar métricas de estado para los pods y nodos del clúster de AKS, estas métricas de mantenimiento están disponibles en Azure Portal. Para obtener información sobre cómo usar Azure Monitor para contenedores, consulte el artículo sobre cómo [ver el estado de Azure Kubernetes Service](monitoring-container-insights-analyze.md).