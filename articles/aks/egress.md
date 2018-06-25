---
title: Tráfico de salida de lista blanca desde el clúster de Azure Kubernetes Service (AKS)
description: Tráfico de salida de lista blanca desde un clúster de Azure Kubernetes Service (AKS)
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: nepeters
ms.openlocfilehash: 0bafb62fcdc8a24084cf7170a0e0f72bfd7824b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659815"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Salida de Azure Kubernetes Service (AKS)

De forma predeterminada, la dirección de salida desde un clúster de Azure Kubernetes Service (AKS) se asigna aleatoriamente. Esta configuración no es la más idónea cuando es necesario identificar una dirección IP para que tenga acceso a servicios externos. Este documento describe cómo crear y mantener una dirección IP de salida asignada estáticamente en un clúster de AKS.

## <a name="egress-overview"></a>Información general de salida

El tráfico saliente de un clúster de AKS sigue las convenciones de Azure Load Balancer, que se documentan [aquí][outbound-connections]. Antes de que se cree el primer servicio Kubernetes de tipo `LoadBalancer`, los nodos de agente no forman parte de ningún grupo de Azure Load Balancer. En esta configuración, los nodos están sin una dirección IP pública de nivel de instancia. Azure traduce el flujo de salida a una dirección IP de origen pública que no es configurable o determinista.

Una vez que se ha creado un servicio Kubernetes de tipo `LoadBalancer`, los nodos de agente se agregan a un grupo Azure Load Balancer. Para el flujo de salida, Azure lo convierte a la primera dirección IP pública configurada en el equilibrador de carga.

## <a name="create-a-static-public-ip"></a>Creación de una IP pública estática

Para impedir que se usen direcciones IP aleatorias, cree una dirección IP estática y asegúrese de que el equilibrador de carga usa esta dirección. La dirección IP debe crearse en el grupo de recursos del **nodo** de AKS.

Obtenga el nombre del grupo de recursos con el comando [az resource show][az-resource-show]. Actualice el nombre del grupo de recursos y el nombre del clúster para que coincida con su entorno.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Después, use el comando [az network public-ip create][public-ip-create] para crear una dirección IP pública estática. Actualice el nombre del grupo de recursos para que coincida con el nombre recogido en el último paso.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Crear un servicio con la dirección IP estática

Ahora que tiene una dirección IP, cree un servicio Kubernetes con el tipo `LoadBalancer` y asigne la dirección IP al servicio.

Cree un archivo denominado `egress-service.yaml` y cópielo en el siguiente código YAML. Actualice la dirección IP para que coincida con su entorno.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-egress
spec:
  loadBalancerIP: 23.101.128.81
  type: LoadBalancer
  ports:
  - port: 8080
```

Cree el servicio y la implementación con el comando `kubectl apply`.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Al crear este servicio se configura una nueva dirección IP de front-end en Azure Load Balancer. Si no tiene más direcciones IP configuradas, entonces ahora **todo** el tráfico de salida debería usar esta dirección. Cuando hay varias direcciones configuradas en Azure Load Balancer, la salida usa la primera dirección IP en el equilibrador de carga.

## <a name="verify-egress-address"></a>Comprobar la dirección de salida

Para comprobar que se está usando la dirección IP pública, utilice un servicio como `checkip.dyndns.org`.

Iniciar y conectarse a un pod:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Si es necesario, instale CURL en el contenedor:

```console
$ apt-get update && apt-get install curl -y
```

Curl `checkip.dyndns.org`, que devuelve la dirección IP de salida:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

Debería ver que la dirección IP coincide con la dirección IP estática adjuntada a Azure Load Balancer.

## <a name="ingress-controller"></a>Controlador de entrada

Para evitar mantener varias direcciones IP públicas en Azure Load Balancer, considere la posibilidad de usar un controlador de entrada. Los controladores de entrada proporcionan ventajas como equilibrio de carga, terminación SSL/TLS, compatibilidad con reescritura de URI y cifrado SSL/TLS ascendente. Para obtener más información sobre los controladores de entrada en AKS, consulte la guía [Entrada HTTPS en Azure Kubernetes Service (AKS)][ingress-aks-cluster].

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el software que se muestra en este documento.

- [CLI de Helm][helm-cli-install]
- [Controlador de entrada NGINX][nginx-ingress]
- [Conexiones salientes de Azure Load Balancer][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
