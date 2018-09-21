---
title: Ejecución de Virtual Kubelet en un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a usar Virtual Kubelet con Azure Kubernetes Service (AKS) para ejecutar contenedores de Linux y Windows en Azure Container Instances.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/14/2018
ms.author: iainfou
ms.openlocfilehash: 6ff28443dda65e91fa69fececaff95aa8e872603
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604265"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Uso de Virtual Kubelet con Azure Kubernetes Service (AKS)

Azure Container Instances (ACI) proporciona un entorno hospedado para ejecutar contenedores en Azure. Cuando se usa ACI, no es necesario administrar la infraestructura de proceso subyacente, ya que Azure controla esta administración automáticamente. Al ejecutar contenedores en ACI, se le cobrará por segundo para cada contenedor en ejecución.

Al usar el proveedor de Virtual Kubelet para Azure Container Instances, los contenedores de Linux y Windows se pueden programar en una instancia de contenedor como si fueran un nodo de Kubernetes estándar. Esta configuración le permite aprovechar las funcionalidades de Kubernetes y la ventaja de valor y costo de administración de las instancias de contenedor.

> [!NOTE]
> Virtual Kubelet es un proyecto de código abierto experimental y debe usarse como tal. Para contribuir, registrar problemas y leer más información acerca de Virtual Kubelet, consulte el [proyecto Virtual Kubelet de GitHub][vk-github].

Este documento describe la configuración de Virtual Kubelet para las instancias de contenedor en un AKS.

## <a name="prerequisite"></a>Requisito previo

En este documento se supone que tiene un clúster de AKS. Si necesita un clúster de AKS, consulte la [guía de inicio rápido de Azure Kubernetes Service (AKS)][aks-quick-start].

También es necesaria la versión **2.0.33** de la CLI de Azure u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Para instalar Virtual Kubelet, se requiere también [Helm](https://docs.helm.sh/using_helm/#installing-helm).

### <a name="for-rbac-enabled-clusters"></a>Para los clústeres habilitados para RBAC

Si el clúster de AKS está habilitado para RBAC, debe crear una cuenta de servicio y el enlace de rol para usarse con Tiller. Para más información, consulte [Control de acceso basado en rol de Helm][helm-rbac]. Para crear una cuenta de servicio y un enlace de rol, cree un archivo denominado *rbac-virtualkubelet.yaml* y pegue la siguiente definición:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Aplique la cuenta de servicio y el enlace con [kubectl apply][kubectl-apply] y especifique su archivo *rbac-virtualkubelet.yaml*, como se muestra en el ejemplo siguiente:

```
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

Configuración de Helm para usar la cuenta de servicio de Tiller:

```console
helm init --service-account tiller
```

Ahora puede seguir instalando Virtual Kubelet en el clúster de AKS.

## <a name="installation"></a>Instalación

Use el comando [az aks install-connector][aks-install-connector] para instalar Virtual Kubelet. En el ejemplo siguiente se implementan ambos conectores de Linux y Windows.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Estos argumentos están disponibles para el comando `aks install-connector`.

| Argumento: | DESCRIPCIÓN | Obligatorio |
|---|---|:---:|
| `--connector-name` | Nombre del conector de ACI.| SÍ |
| `--name``-n` | Nombre del clúster administrado. | SÍ |
| `--resource-group``-g` | Nombre del grupo de recursos. | SÍ |
| `--os-type` | Tipo de sistema operativo de Container Instances. Valores permitidos: Both, Linux, Windows. Valor predeterminado: Linux. | Sin  |
| `--aci-resource-group` | Grupo de recursos en el que se crearán los grupos de contenedores de ACI. | Sin  |
| `--location``-l` | Ubicación donde se crearán los grupos de contenedores de ACI. | Sin  |
| `--service-principal` | Entidad de servicio utilizada para la autenticación en las API de Azure. | Sin  |
| `--client-secret` | Secreto asociado a la entidad de servicio. | Sin  |
| `--chart-url` | Dirección URL de un gráfico de Helm que instala ACI Connector. | Sin  |
| `--image-tag` | Etiqueta de imagen de la imagen de contenedor de Virtual Kubelet. | Sin  |

## <a name="validate-virtual-kubelet"></a>Validación de Virtual Kubelet

Para comprobar que Virtual Kubelet esté instalado, devuelva una lista de nodos de Kubernetes con el comando [kubectl get nodes][kubectl-get].

```
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
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Ejecute la aplicación con el comando [kubectl create][kubectl-create].

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Use el comando [kubectl get pods][kubectl-get] con el argumento `-o wide` para generar una lista de pods con el nodo programado. Observe que el pod `aci-helloworld` se ha programado en el nodo `virtual-kubelet-virtual-kubelet-linux`.

```
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
        image: microsoft/iis:nanoserver
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Ejecute la aplicación con el comando [kubectl create][kubectl-create].

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Use el comando [kubectl get pods][kubectl-get] con el argumento `-o wide` para generar una lista de pods con el nodo programado. Observe que el pod `nanoserver-iis` se ha programado en el nodo `virtual-kubelet-virtual-kubelet-win`.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Eliminación de Virtual Kubelet

Use el comando [az aks remove-connector][aks-remove-connector] para quitar Virtual Kubelet. Reemplace los valores de argumento por el nombre del conector, el clúster de AKS y el grupo de recursos del clúster de AKS.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

> [!NOTE]
> Si encuentra errores al quitar ambos conectores del sistema operativo, o quiere quitar simplemente el conector del sistema operativo Windows o Linux, puede especificar manualmente el tipo de sistema operativo. Agregue el parámetro `--os-type` al comando `az aks remove-connector` anterior y especifique `Windows` o `Linux`.

## <a name="next-steps"></a>Pasos siguientes

Para posibles problemas con Virtual Kubelet, consulte [Known quirks and workarounds][vk-troubleshooting] (Peculiaridades conocidas y soluciones alternativas). Para informar de problemas con Virtual Kubelet, [abra un problema de GitHub][vk-issues].

Obtenga más información sobre Virtual Kubelet en el [proyecto Virtual Kubelet de Github][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
