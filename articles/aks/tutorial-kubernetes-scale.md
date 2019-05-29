---
title: 'Tutorial de Kubernetes en Azure: escalado de una aplicación'
description: En este tutorial de Azure Kubernetes Service (AKS), obtendrá información sobre cómo escalar los nodos y los pods de Kubernetes e implementar el escalado horizontal automático de pods.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: ed82c214e2fbb2a1efc5917eb74ffe9b29abb115
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560322"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Tutorial: Escalado de aplicaciones en Azure Kubernetes Service (AKS)

Si ha seguido los tutoriales, tiene un clúster de Kubernetes en funcionamiento en AKS y ha implementado la aplicación Azure Voting de ejemplo. En este tutorial, la parte cinco de siete, se escalan horizontalmente los pods en la aplicación y se prueba el escalado automático de pods. También se aprende a escalar el número de nodos de VM de Azure para cambiar la capacidad del clúster de hospedar cargas de trabajo. Aprenderá a:

> [!div class="checklist"]
> * Escalar los nodos de Kubernetes
> * Escalar de forma manual pods de Kubernetes que ejecutan la aplicación
> * Configurar el escalado automático de pods que ejecutan el front-end de la aplicación

En otros tutoriales, la aplicación Azure Vote se actualiza a una nueva versión.

## <a name="before-you-begin"></a>Antes de empezar

En los tutoriales anteriores se empaquetó una aplicación en una imagen de contenedor. Esta imagen se cargó en Azure Container Registry y el usuario creó un clúster de AKS. Luego la aplicación se implementó en el clúster de AKS. Si no ha realizado estos pasos, pero quiere continuar, comience con el [Tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

Para realizar este tutorial es necesario ejecutar la versión 2.0.53, o superior, de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="manually-scale-pods"></a>Escalado manual de pods

Cuando el front-end de Azure Vote y la instancia de Redis se implementaron en los tutoriales anteriores, se creó una única réplica. Para ver el número y el estado de los pods del clúster, use el comando [kubectl get][kubectl-get] como se indica a continuación:

```console
kubectl get pods
```

La salida del ejemplo siguiente muestra un pod de front-end y un pod de back-end:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Para cambiar manualmente el número de pods en la implementación *azure-vote-front*, use el comando [kubectl scale][kubectl-scale]. El ejemplo siguiente aumenta el número de pods de front-end a *5*:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Ejecute de nuevo [kubectl get pods][kubectl-get] para comprobar que AKS crea los pods adicionales. Tras un minuto aproximadamente, los pods adicionales están disponibles en el clúster:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Escalado automático de pods

Kubernetes admite el [escalado automático horizontal de pods][kubernetes-hpa] para ajustar el número de pods en una implementación en función del uso de la CPU o de otras métricas de selección. El [servidor de medición][metrics-server] se usa para proporcionar utilización de recursos a Kubernetes y se implementa automáticamente en clústeres de AKS de la versión 1.10 o posteriores. Para ver la versión del clúster de AKS, use el comando [az aks show][az-aks-show], tal como se muestra en el ejemplo siguiente:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query kubernetesVersion
```

Si el clúster de AKS es de una versión inferior a la *1.10*, instale el servidor de medición. En caso contrario, omita este paso. Para realizar la instalación, clone el repositorio de GitHub `metrics-server` e instale las definiciones de recursos de ejemplo. Para ver el contenido de estas definiciones de YAML, consulte [Servidor de métricas para Kuberenetes 1.8 +][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Para usar la autoescala, todos los contenedores de los pod y los pods deben tener definidos solicitudes y límites de CPU. En la implementación de `azure-vote-front`, el contenedor del front-end ya solicita 0,25 CPU, con un límite de 0,5 CPU. Estas solicitudes de recursos y los límites se definen tal y como se muestra en el siguiente fragmento de código de ejemplo:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

En el ejemplo siguiente se usa el comando [kubectl autoscale][kubectl-autoscale] para escalar automáticamente el número de pods en la implementación *azure-vote-front*. Si el uso de la CPU supera el 50 %, el escalador automático aumenta el número de pods hasta un máximo de *10* instancias. Luego se define un mínimo de *3* instancias para la implementación:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Para ver el estado del escalador automático, use el comando `kubectl get hpa` como se indica a continuación:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Después de unos minutos con carga mínima en la aplicación Azure Vote, el número de réplicas del pod se reduce automáticamente a tres. Puede usar nuevamente `kubectl get pods` para ver los pods innecesarios que se han eliminado.

## <a name="manually-scale-aks-nodes"></a>Escalado manual de nodos de AKS

Si creó el clúster de Kubernetes mediante los comandos en el tutorial anterior, este tiene un nodo. Puede ajustar el número de nodos manualmente si tiene previsto usar más o menos cargas de trabajo de contenedor en el clúster.

En el ejemplo siguiente, el número de nodos aumenta a tres en el clúster de Kubernetes denominado *myAKSCluster*. El comando tarda unos minutos en completarse.

```azurecli
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
```

Cuando el clúster se escaló correctamente, el resultado es similar al ejemplo siguiente:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se han usado distintas características de escalado en el clúster de Kubernetes. Ha aprendido a:

> [!div class="checklist"]
> * Escalar de forma manual pods de Kubernetes que ejecutan la aplicación
> * Configurar el escalado automático de pods que ejecutan el front-end de la aplicación
> * Escalado manual de los nodos de Kubernetes

Vaya al siguiente tutorial para aprender a actualizar la aplicación en Kubernetes.

> [!div class="nextstepaction"]
> [Actualización de una aplicación en Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://v1-12.docs.kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
