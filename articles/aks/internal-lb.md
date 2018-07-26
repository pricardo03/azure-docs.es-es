---
title: Creación de un equilibrador de carga interno de Azure Kubernetes Service (AKS)
description: Aprenda a crear y usar un equilibrador de carga interno para exponer los servicios con Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/12/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 123fc08995416e0ff9c7e12a526deadc34b3a4a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001402"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Uso de un equilibrador de carga interno con Azure Kubernetes Service (AKS)

El equilibrio de carga interno permite que un servicio de Kubernetes sea accesible para las aplicaciones que se ejecutan en la misma red virtual que el clúster de Kubernetes. En este artículo se muestra cómo crear y usar un equilibrador de carga interno con Azure Kubernetes Service (AKS). Azure Load Balancer está disponible en dos SKU: Básico y Estándar. AKS usa el SKU Básico.

## <a name="create-an-internal-load-balancer"></a>Creación de un conjunto de equilibrador de carga interno

Para crear un equilibrador de carga interno, cree un manifiesto de servicio con el tipo de servicio *LoadBalancer* y la anotación *azure-load-balancer-internal*, tal como se muestra en el siguiente ejemplo:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Una vez implementado con `kubetctl apply`, se crea un equilibrador de carga de Azure y se pone a disposición en la misma red virtual que el clúster de AKS.

![Imagen del equilibrador de carga interno de AKS](media/internal-lb/internal-lb.png)

Cuando vea los detalles del servicio, la dirección IP en la columna *EXTERNAL-IP* es la dirección IP del equilibrador de carga interno, tal como se muestra en el siguiente ejemplo:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Especificación de una dirección IP

Si quiere usar una dirección IP específica con el equilibrador de carga interno, agregue la propiedad *loadBalancerIP* a la especificación del equilibrador de carga. La dirección IP especificada debe encontrarse en la misma subred que el clúster de AKS y no se debe haber asignado ya a un recurso.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Al ver los detalles del servicio, la dirección IP de *EXTERNAL-IP* debe reflejar la dirección IP especificada:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Uso de redes privadas

Al crear su clúster de AKS, puede especificar la configuración de red avanzada. Este enfoque le permite implementar el clúster en una red y subredes virtuales de Azure existentes. Un escenario es implementar el clúster de AKS en una red privada conectada a su entorno local y ejecutar servicios solo accesibles internamente. Para más información, consulte [Configuración de red en Azure Kubernetes Service (AKS)][advanced-networking].

No se necesitan cambios en los pasos anteriores para implementar un equilibrador de carga interno en un clúster de AKS que utilice una red privada. El equilibrador de carga se crea en el mismo grupo de recursos que el clúster de AKS pero está conectado a la red virtual privada y subred, como se muestra en el siguiente ejemplo:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Es posible que tenga que conceder a la entidad de servicio del clúster de AKS la función *Colaborador de red* al grupo de recursos en el que están implementados los recursos de la red virtual de Azure. Consulte la entidad de servicio con [az aks show][az-aks-show], como `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Para crear una asignación de roles, use el comando [az role assignment create][az-role-assignment-create].

## <a name="specify-a-different-subnet"></a>Especificación de una subred diferente

Para especificar una subred para el equilibrador de carga, agregue la anotación *azure-load-balancer-internal-subnet* al servicio. La subred especificada debe estar en la misma red virtual que el clúster de AKS. Cuando se implementa, la dirección *EXTERNAL-IP* del equilibrador de carga forma parte de la subred especificada.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="delete-the-load-balancer"></a>Eliminación del equilibrador de carga

Cuando se han eliminado todos los servicios que usan el equilibrador de carga interno, también se elimina el propio equilibrador de carga.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los servicios de Kubernetes en la [documentación de servicios de Kubernetes][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[advanced-networking]: networking-overview.md
[deploy-advanced-networking]: networking-overview.md#configure-networking---cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create