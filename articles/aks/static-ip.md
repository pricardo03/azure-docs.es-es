---
title: Use una dirección IP estática con el equilibrador de carga de Azure Kubernetes Service (AKS).
description: Aprenda a crear y usar una dirección IP estática con el equilibrador de carga de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: b51da8c5e5e113cdb7e449206f7137386b278be4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025929"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Usar una dirección IP pública estática con el equilibrador de carga de Azure Kubernetes Service (AKS)

De forma predeterminada, la dirección IP pública asignada a un recurso de equilibrador de carga creado por un clúster de AKS solo es válida para la duración de ese recurso. Si elimina el servicio de Kubernetes, el equilibrador de carga asociado y la dirección IP también se eliminan. Si quiere asignar una dirección IP específica o conservar una dirección IP para los servicios de Kubernetes reimplementados, puede crear y usar una dirección IP pública estática.

En este artículo se muestra cómo crear una dirección IP pública estática y asignarla al servicio de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, vea la guía de inicio rápido AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También es preciso que esté instalada y configurada la versión 2.0.46 de la CLI de Azure u otra posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="create-a-static-ip-address"></a>Crear una dirección IP estática

Cuando se crea una dirección IP pública estática para usarla con AKS, el recurso de dirección IP debe crearse en el grupo de recursos del **nodo**. Obtenga el nombre del grupo de recursos con el comando [az aks show][az-aks-show] y agregue el parámetro de consulta `--query nodeResourceGroup`. En este ejemplo se obtiene el grupo de recursos del nodo para el nombre de clúster de AKS *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Cree una dirección IP pública estática con el comando [az network public ip create][az-network-public-ip-create]. Especifique el nombre del grupo de recursos del nodo que obtuvo en el comando anterior y, después, un nombre para el recurso de dirección IP, como *myAKSPublicIP*:

```azurecli
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

Se muestra la dirección IP, como se indica en esta salida de ejemplo reducido:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
````

Después puede obtener la dirección IP pública mediante el comando [az network public-ip list][az-network-public-ip-list]. Especifique el nombre del grupo de recursos del nodo y la dirección IP pública creados y envíe una consulta para *ipAddress*, como se muestra en este ejemplo:

```azurecli
$ az network public-ip show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Crear un servicio mediante la dirección IP estática

Para crear un servicio con la dirección IP pública estática, agregue la propiedad `loadBalancerIP` y el valor de la dirección IP pública estática al manifiesto de YAML. Cree un archivo denominado `load-balancer-service.yaml` y cópielo en el siguiente código YAML. Indique su propia dirección IP pública que creó en el paso anterior.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Cree el servicio y la implementación con el comando `kubectl apply`.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="troubleshoot"></a>Solución de problemas

Si la dirección IP estática definida en la propiedad *loadBalancerIP* del manifiesto de servicio de Kubernetes no existe o no se ha creado en el grupo de recursos del nodo, se produce un error en la creación del servicio del equilibrador de carga. Para solucionar este problema, revise los eventos de creación del servicio con el comando [kubectl describe][kubectl-describe]. Indique el nombre del servicio tal y como aparece en el manifiesto de YAML, como se muestra en este ejemplo:

```console
kubectl describe service azure-load-balancer
```

Se muestra información sobre el recurso de servicio de Kubernetes. Los *Eventos* al final de la salida de este ejemplo indican que *no se encontró la dirección IP proporcionada por el usuario*. En estos casos, compruebe que ha creado la dirección IP pública estática en el grupo de recursos del nodo y que la dirección IP especificada en el manifiesto de servicio de Kubernetes es correcta.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Pasos siguientes

Para un control adicional sobre el tráfico de red a las aplicaciones, puede que sea mejor [crear un controlador de entrada][aks-ingress-basic]. También puede [crear un controlador de entrada con una dirección IP pública estática][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
