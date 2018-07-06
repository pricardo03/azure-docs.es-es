---
title: Ejecución de Virtual Kubelet en un clúster de Azure Kubernetes Service (AKS)
description: Utilice Virtual Kubelet para ejecutar contenedores de Kubernetes en Azure Container Instances.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: nepeters
ms.openlocfilehash: 0d046970b40f5253b07005ab578035c7c179e7df
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304016"
---
# <a name="virtual-kubelet-with-aks"></a>Virtual Kubelet con AKS

Azure Container Instances (ACI) proporciona un entorno hospedado para ejecutar contenedores en Azure. Cuando se usa ACI, no es necesario administrar la infraestructura de proceso subyacente, ya que Azure controla esta administración automáticamente. Al ejecutar contenedores en ACI, se le cobrará por segundo para cada contenedor en ejecución.

Al usar el proveedor de Virtual Kubelet para Azure Container Instances, los contenedores de Linux y Windows se pueden programar en una instancia de contenedor como si fueran un nodo de Kubernetes estándar. Esta configuración le permite aprovechar las funcionalidades de Kubernetes y la ventaja de valor y costo de administración de las instancias de contenedor.

> [!NOTE]
> Virtual Kubelet es un proyecto de código abierto experimental y debe usarse como tal. Para contribuir, registrar problemas y leer más información acerca de Virtual Kubelet, consulte el [proyecto Virtual Kubelet de GitHub][vk-github].

Este documento describe la configuración de Virtual Kubelet para las instancias de contenedor en un AKS.

## <a name="prerequisite"></a>Requisito previo

En este documento se supone que tiene un clúster de AKS. Si necesita un clúster de AKS, consulte la [guía de inicio rápido de Azure Kubernetes Service (AKS)][aks-quick-start].

También es necesaria la versión **2.0.33** de la CLI de Azure u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

[Helm](https://docs.helm.sh/using_helm/#installing-helm) también es necesario para poder instalar Virtual Kubelet.

## <a name="installation"></a>Instalación

Use el comando [az aks install-connector][aks-install-connector] para instalar Virtual Kubelet. En el ejemplo siguiente se implementan ambos conectores de Linux y Windows.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Estos argumentos están disponibles para el comando `aks install-connector`.

| Argumento: | DESCRIPCIÓN | Obligatorio |
|---|---|:---:|
| `--connector-name` | Nombre del conector de ACI.| Sí |
| `--name``-n` | Nombre del clúster administrado. | Sí |
| `--resource-group``-g` | Nombre del grupo de recursos. | Sí |
| `--os-type` | Tipo de sistema operativo de Container Instances. Valores permitidos: Both, Linux, Windows. Valor predeterminado: Linux. | Sin  |
| `--aci-resource-group` | Grupo de recursos en el que se crearán los grupos de contenedores de ACI. | Sin  |
| `--location``-l` | Ubicación donde se crearán los grupos de contenedores de ACI. | Sin  |
| `--service-principal` | Entidad de servicio utilizada para la autenticación en las API de Azure. | Sin  |
| `--client-secret` | Secreto asociado a la entidad de servicio. | Sin  |
| `--chart-url` | Dirección URL de un gráfico de Helm que instala ACI Connector. | Sin  |
| `--image-tag` | Etiqueta de imagen de la imagen de contenedor de Virtual Kubelet. | Sin  |

## <a name="validate-virtual-kubelet"></a>Validación de Virtual Kubelet

Para comprobar que Virtual Kubelet esté instalado, devuelva una lista de nodos de Kubernetes con el comando [kubectl get nodes][kubectl-get].

```console
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Ejecución del contenedor de Linux

Cree un archivo denominado `virtual-kubelet-linux.yaml` y cópielo en el siguiente código YAML. Reemplace el valor `kubernetes.io/hostname` por el nombre del nodo de Virtual Kubelet de Linux. Observe que se usa un campo [nodeSelector][node-selector] y un campo [toleration][toleration] para programar el contenedor en el nodo.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-linux
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Ejecute la aplicación con el comando [kubectl create][kubectl-create].

```azurecli-interactive
kubectl create -f virtual-kubelet-linux.yaml
```

Use el comando [kubectl get pods][kubectl-get] con el argumento `-o wide` para generar una lista de pods con el nodo programado. Observe que el pod `aci-helloworld` se ha programado en el nodo `virtual-kubelet-virtual-kubelet-linux`.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Ejecución del contenedor de Windows

Cree un archivo denominado `virtual-kubelet-windows.yaml` y cópielo en el siguiente código YAML. Reemplace el valor `kubernetes.io/hostname` por el nombre del nodo de Virtual Kubelet de Windows. Observe que se usa un campo [nodeSelector][node-selector] y un campo [toleration][toleration] para programar el contenedor en el nodo.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: nanoserver/iis
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Ejecute la aplicación con el comando [kubectl create][kubectl-create].

```azurecli-interactive
kubectl create -f virtual-kubelet-windows.yaml
```

Use el comando [kubectl get pods][kubectl-get] con el argumento `-o wide` para generar una lista de pods con el nodo programado. Observe que el pod `nanoserver-iis` se ha programado en el nodo `virtual-kubelet-virtual-kubelet-win`.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Eliminación de Virtual Kubelet

Use el comando [az aks remove-connector][aks-remove-connector] para quitar Virtual Kubelet. Reemplace los valores de argumento por el nombre del conector, el clúster de AKS y el grupo de recursos del clúster de AKS.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el kubelet virtual en el [proyecto del kubelet virtual en Github][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az_aks_list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
