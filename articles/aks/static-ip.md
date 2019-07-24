---
title: Use una dirección IP estática con el equilibrador de carga de Azure Kubernetes Service (AKS).
description: Aprenda a crear y usar una dirección IP estática con el equilibrador de carga de Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: mlearned
ms.openlocfilehash: 9e32715766734bcbb150d70aeed2dc5b06a4bcbb
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614462"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Usar una dirección IP pública estática con el equilibrador de carga de Azure Kubernetes Service (AKS)

De forma predeterminada, la dirección IP pública asignada a un recurso de equilibrador de carga creado por un clúster de AKS solo es válida para la duración de ese recurso. Si elimina el servicio de Kubernetes, el equilibrador de carga asociado y la dirección IP también se eliminan. Si quiere asignar una dirección IP específica o conservar una dirección IP para los servicios de Kubernetes reimplementados, puede crear y usar una dirección IP pública estática.

En este artículo se muestra cómo crear una dirección IP pública estática y asignarla al servicio de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

También es preciso que esté instalada y configurada la versión 2.0.59 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

Actualmente, solo se admite la *SKU de IP básica*. Se está trabajando para admitir la SKU *IP estándar* de recurso. Para más información, consulte [Tipos de direcciones IP y métodos de asignación en Azure][ip-sku].

## <a name="create-a-static-ip-address"></a>Crear una dirección IP estática

Cuando se crea una dirección IP pública estática para usarla con AKS, el recurso de dirección IP debe crearse en el grupo de recursos del **nodo**. Si desea separar los recursos, consulte la sección [Uso de una dirección IP estática fuera del grupo de recursos del nodo](#use-a-static-ip-address-outside-of-the-node-resource-group).

Primero, obtenga el nombre del grupo de recursos del nodo con el comando [az aks show][az-aks-show] y agregue el parámetro de consulta `--query nodeResourceGroup`. En este ejemplo se obtiene el grupo de recursos del nodo para el nombre de clúster de AKS *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Cree una dirección IP pública estática con el comando [az network public ip create][az-network-public-ip-create]. Especifique el nombre del grupo de recursos del nodo que obtuvo en el comando anterior y, después, un nombre para el recurso de dirección IP, como *myAKSPublicIP*:

```azurecli-interactive
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
    [...]
  }
}
```

Después puede obtener la dirección IP pública mediante el comando [az network public-ip list][az-network-public-ip-list]. Especifique el nombre del grupo de recursos del nodo y la dirección IP pública creados y envíe una consulta para *ipAddress*, como se muestra en este ejemplo:

```azurecli-interactive
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

## <a name="use-a-static-ip-address-outside-of-the-node-resource-group"></a>Uso de una dirección IP estática fuera del grupo de recursos del nodo

Con Kubernetes 1.10 o versiones posteriores, puede usar una dirección IP estática que se haya creado fuera del grupo de recursos del nodo. La entidad de servicio utilizada por el clúster de AKS debe tener permisos delegados al otro grupo de recursos, como se muestra en el siguiente ejemplo:

```azurecli-interactive
az role assignment create\
    --assignee <SP Client ID> \
    --role "Network Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Para utilizar una dirección IP fuera del grupo de recursos del nodo, agregue una anotación a la definición de servicio. En el ejemplo siguiente se establece el grupo de recursos denominado *myResourceGroup*. Indique su propio nombre del grupo de recursos:

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-resource-group: myResourceGroup
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

## <a name="troubleshoot"></a>Solución de problemas

Si la dirección IP estática definida en la propiedad *loadBalancerIP* del manifiesto de servicio de Kubernetes no existe o no se ha creado en el grupo de recursos del nodo y no se han configurado delegaciones adicionales, se produce un error en la creación del servicio del equilibrador de carga. Para solucionar este problema, revise los eventos de creación del servicio con el comando [kubectl describe][kubectl-describe]. Indique el nombre del servicio tal y como aparece en el manifiesto de YAML, como se muestra en este ejemplo:

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

Para un control adicional sobre el tráfico de red a las aplicaciones, puede que sea mejor [crear un controlador de entrada][aks-ingress-basic]. You can also [create an ingress controller with a static public IP address][aks-static-ingress].

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
[ip-sku]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md#sku
