---
title: Ejecución de Virtual Kubelet en un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a usar Virtual Kubelet con Azure Kubernetes Service (AKS) para ejecutar contenedores de Linux y Windows en Azure Container Instances.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: cc0c3becf21cb54b97a88e9ba35b38308af81a85
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475429"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Uso de Virtual Kubelet con Azure Kubernetes Service (AKS)

Azure Container Instances (ACI) proporciona un entorno hospedado para ejecutar contenedores en Azure. Cuando se usa ACI, no es necesario administrar la infraestructura de proceso subyacente, ya que Azure controla esta administración automáticamente. Al ejecutar contenedores en ACI, se le cobrará por segundo para cada contenedor en ejecución.

Al usar el proveedor de Virtual Kubelet para Azure Container Instances, los contenedores de Linux y Windows se pueden programar en una instancia de contenedor como si fueran un nodo de Kubernetes estándar. Esta configuración le permite aprovechar las funcionalidades de Kubernetes y la ventaja de valor y costo de administración de las instancias de contenedor.

> [!NOTE]
> AKS ahora tiene compatibilidad integrada para programar los contenedores en ACI, que se conocen como *nodos virtuales*. Actualmente, estos nodos virtuales admiten instancias de contenedor de Linux. Si tiene que programar instancias de contenedor de Windows, puede seguir usando Virtual Kubelet. En caso contrario, debe usar los nodos virtuales en lugar de las instrucciones manuales de Virtual Kubelet que se han indicado en este artículo. Puede empezar a trabajar con los nodos virtuales mediante la [CLI de Azure] [virtual-nodes-cli] o [Azure Portal][virtual-nodes-portal].
>
> Virtual Kubelet es un proyecto de código abierto experimental y debe usarse como tal. Para contribuir, registrar problemas y leer más información acerca de Virtual Kubelet, consulte el [proyecto Virtual Kubelet de GitHub][vk-github].

## <a name="before-you-begin"></a>Antes de empezar

En este documento se supone que tiene un clúster de AKS. Si necesita un clúster de AKS, consulte la [guía de inicio rápido de Azure Kubernetes Service (AKS)][aks-quick-start].

También necesitará la versión de CLI de Azure **2.0.65** o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Para instalar Virtual Kubelet, instalar y configurar [Helm] [ aks-helm] en el clúster de AKS. Asegúrese de que es su Tiller [configurado para su uso con Kubernetes RBAC](#for-rbac-enabled-clusters), si es necesario.

### <a name="register-container-instances-feature-provider"></a>Registrar proveedor de características de Container Instances

Si no ha usado anteriormente el servicio de instancia de contenedor de Azure (ACI), registrar el proveedor de servicios con su suscripción. Puede comprobar el estado de registro del proveedor de ACI mediante el comando de [lista de proveedores de az][az-provider-list], tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

El proveedor *Microsoft.ContainerInstance* debería notificar como *Registrado*, tal como se muestra en el siguiente ejemplo de salida:

```console
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Si el proveedor se muestra como *NotRegistered*, registre el proveedor mediante el [registro de proveedor de az] [ az-provider-register] tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

### <a name="for-rbac-enabled-clusters"></a>Para los clústeres habilitados para RBAC

Si el clúster de AKS está habilitado para RBAC, debe crear una cuenta de servicio y el enlace de rol para usarse con Tiller. Para más información, consulte [Control de acceso basado en rol de Helm][helm-rbac]. Para crear una cuenta de servicio y un enlace de rol, cree un archivo denominado *rbac-virtualkubelet.yaml* y pegue la siguiente definición:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
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

Aplique la cuenta de servicio y el enlace con [kubectl apply][kubectl-apply] y especifique el archivo *rbac-virtualkubelet.yaml*, tal y como se muestra en el ejemplo siguiente:

```console
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
az aks install-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

Estos argumentos están disponibles para el [az aks install-connector] [ aks-install-connector] comando.

| Argumento: | DESCRIPCIÓN | Obligatorio |
|---|---|:---:|
| `--connector-name` | Nombre del conector de ACI.| Sí |
| `--name``-n` | Nombre del clúster administrado. | Sí |
| `--resource-group``-g` | Nombre del grupo de recursos. | Sí |
| `--os-type` | Tipo de sistema operativo de Container Instances. Valores permitidos: Ambos, Linux, Windows. Valor predeterminado: Linux. | Sin |
| `--aci-resource-group` | Grupo de recursos en el que se crearán los grupos de contenedores de ACI. | Sin |
| `--location``-l` | Ubicación donde se crearán los grupos de contenedores de ACI. | No |
| `--service-principal` | Entidad de servicio utilizada para la autenticación en las API de Azure. | Sin |
| `--client-secret` | Secreto asociado a la entidad de servicio. | Sin |
| `--chart-url` | Dirección URL de un gráfico de Helm que instala ACI Connector. | No |
| `--image-tag` | Etiqueta de imagen de la imagen de contenedor de Virtual Kubelet. | Sin |

## <a name="validate-virtual-kubelet"></a>Validación de Virtual Kubelet

Para validar que se ha instalado Virtual Kubelet, devolver una lista de los nodos de Kubernetes mediante la [kubectl obtener nodos] [ kubectl-get] comando:

```console
$ kubectl get nodes

NAME                                             STATUS   ROLES   AGE   VERSION
aks-nodepool1-56577038-0                         Ready    agent   11m   v1.12.8
virtual-kubelet-virtual-kubelet-linux-eastus     Ready    agent   39s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
virtual-kubelet-virtual-kubelet-windows-eastus   Ready    agent   37s   v1.13.1-vk-v0.9.0-1-g7b92d1ee-dev
```

## <a name="run-linux-container"></a>Ejecución del contenedor de Linux

Cree un archivo denominado `virtual-kubelet-linux.yaml` y cópielo en el siguiente código YAML. Observe que se usa un campo [nodeSelector][node-selector] y un campo [toleration][toleration] para programar el contenedor en el nodo.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
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
        beta.kubernetes.io/os: linux
        kubernetes.io/role: agent
        type: virtual-kubelet
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

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
aci-helloworld-7b9ffbf946-rx87g   1/1     Running   0          22s     52.224.147.210   virtual-kubelet-virtual-kubelet-linux-eastus
```

## <a name="run-windows-container"></a>Ejecución del contenedor de Windows

Cree un archivo denominado `virtual-kubelet-windows.yaml` y cópielo en el siguiente código YAML. Observe que se usa un campo [nodeSelector][node-selector] y un campo [toleration][toleration] para programar el contenedor en el nodo.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nanoserver-iis
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
        beta.kubernetes.io/os: windows
        kubernetes.io/role: agent
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Equal
        value: azure
        effect: NoSchedule
```

Ejecute la aplicación con el comando [kubectl create][kubectl-create].

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Use el comando [kubectl get pods][kubectl-get] con el argumento `-o wide` para generar una lista de pods con el nodo programado. Observe que el pod `nanoserver-iis` se ha programado en el nodo `virtual-kubelet-virtual-kubelet-windows`.

```console
$ kubectl get pods -o wide

NAME                              READY   STATUS    RESTARTS   AGE     IP               NODE
nanoserver-iis-5d999b87d7-6h8s9   1/1     Running   0          47s     52.224.143.39    virtual-kubelet-virtual-kubelet-windows-eastus
```

## <a name="remove-virtual-kubelet"></a>Eliminación de Virtual Kubelet

Use el comando [az aks remove-connector][aks-remove-connector] para quitar Virtual Kubelet. Reemplace los valores de argumento por el nombre del conector, el clúster de AKS y el grupo de recursos del clúster de AKS.

```azurecli-interactive
az aks remove-connector \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --connector-name virtual-kubelet \
    --os-type Both
```

> [!NOTE]
> Si encuentra errores al quitar ambos conectores del sistema operativo, o quiere quitar simplemente el conector del sistema operativo Windows o Linux, puede especificar manualmente el tipo de sistema operativo. Agregue el parámetro `--os-type` al comando `az aks remove-connector` anterior y especifique `Windows` o `Linux`.

## <a name="next-steps"></a>Pasos siguientes

Para posibles problemas con Virtual Kubelet, consulte [Known quirks and workarounds][vk-troubleshooting] (Peculiaridades conocidas y soluciones alternativas). Para informar de problemas con Virtual Kubelet, [abra un problema de GitHub][vk-issues].

Obtenga más información sobre Virtual Kubelet en el [proyecto Virtual Kubelet de GitHub][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az-aks-list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector
[virtual-nodes-cli]: virtual-nodes-cli.md
[virtual-nodes-portal]: virtual-nodes-portal.md
[aks-helm]: kubernetes-helm.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[vk-troubleshooting]: https://github.com/virtual-kubelet/virtual-kubelet#known-quirks-and-workarounds
[vk-issues]: https://github.com/virtual-kubelet/virtual-kubelet/issues
