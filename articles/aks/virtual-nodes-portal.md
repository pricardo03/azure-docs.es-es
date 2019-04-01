---
title: Creación de nodos virtuales mediante el portal en Azure Kubernetes Service (AKS)
description: Aprenda a usar el Azure Portal para crear un clúster de Azure Kubernetes Service (AKS) que usa los nodos virtuales para ejecutar pods.
services: container-service
author: iainfoulds
ms.topic: conceptual
ms.service: container-service
ms.date: 12/03/2018
ms.author: iainfou
ms.openlocfilehash: fd538ce6821b35dc6e3932256090afdf70b4b232
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755256"
---
# <a name="preview---create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Obtener una vista previa: crear y configurar un clúster de Azure Kubernetes Service (AKS) para usar los nodos virtuales en Azure portal

Para implementar rápidamente las cargas de trabajo en un clúster de Azure Kubernetes Service (AKS), puede usar nodos virtuales. Con los nodos virtuales, tiene un aprovisionamiento rápido de pods y solo paga por segundo para el tiempo de ejecución. En un escenario de escalado, no es necesario que espere a que el escalador automático del clúster de Kubernetes implemente nodos de proceso de máquina virtual para ejecutar los pods adicionales. En este artículo se muestra cómo crear y configurar los recursos de red virtual y un clúster de AKS con nodos virtuales habilitados.

> [!IMPORTANT]
> Características de versión preliminar AKS son autoservicio y participación. Las versiones preliminares se proporcionan para recopilar comentarios y los errores de nuestra comunidad. Sin embargo, no se admiten por soporte técnico de Azure. Si crea un clúster, o agregar estas características para clústeres existentes, ese clúster no se admite hasta que la característica ya no está en versión preliminar y se aprueba para disponibilidad general (GA).
>
> Si tiene problemas con las características de vista previa, [abra una incidencia en el repositorio de GitHub de AKS] [ aks-github] con el nombre de la característica de vista previa en el título del error.

## <a name="regional-availability"></a>Disponibilidad regional

Para las implementaciones de nodo virtual, se admiten las siguientes regiones:

* Este de Australia (australiaeast)
* Este de EE. UU. (eastus)
* Centro occidental de Ee.uu. (oeste)
* Europa Occidental (westeurope)
* Oeste de EE. UU. (westus)

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

En la esquina superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Kubernetes Service**.

En la página **Datos básicos**, configure las siguientes opciones:

- *DETALLES DEL PROYECTO*: seleccione una suscripción de Azure y, a continuación, seleccione o cree un grupo de recursos de Azure, como *myResourceGroup*. Escriba un **Nombre del clúster de Kubernetes**, como *myAKSCluster*.
- *DETALLES DEL CLÚSTER*: seleccione la región, la versión de Kubernetes y el prefijo del nombre DNS para el clúster de AKS.
- *ESCALA*: seleccione un tamaño de máquina virtual para los nodos de AKS. El tamaño de VM **no** puede cambiarse una vez que se ha implementado un clúster de AKS.
    - Seleccione el número de nodos que se van a implementar en el clúster. Para este artículo, establezca **Número de nodos** en *1*. El número de nodos **puede** ajustarse después de implementar el clúster.
    - En **Nodos virtuales**, seleccione *Habilitado*.

![Creación del clúster de AKS y habilitación de los nodos virtuales](media/virtual-nodes-portal/enable-virtual-nodes.png)

De forma predeterminada, se crea una entidad de servicio de Azure Active Directory. Esta entidad de servicio se usa para la comunicación del clúster y la integración con otros servicios de Azure.

El clúster también se configurado para redes avanzadas. Los nodos virtuales están configurados para usar su propia subred de red virtual de Azure. Esta subred tiene permisos delegados para conectarse a recursos de Azure entre el clúster de AKS. Si aún no tiene una subred delegada, Azure Portal crea y configura la red virtual de Azure y la subred para su uso con los nodos virtuales.

Seleccione **Revisar + crear**. Una vez completada la validación, seleccione **Crear**.

Se tardan unos minutos en crear el clúster de AKS y en prepararlo para usarlo.

## <a name="connect-to-the-cluster"></a>Conexión al clúster

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. Para administrar un clúster de Kubernetes, use [kubectl][kubectl], el cliente de línea de comandos de Kubernetes. El cliente `kubectl` viene preinstalado en Azure Cloud Shell.

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/bash](https://shell.azure.com/bash) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

Use el comando [az aks get-credentials][az-aks-get-credentials] para configurar `kubectl` para conectarse a su clúster de Kubernetes. En el ejemplo siguiente se obtienen credenciales para el nombre de clúster *myAKSCluster* del grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Para comprobar la conexión con el clúster, use el comando [kubectl get][kubectl-get] para que devuelva una lista de los nodos del clúster.

```azurecli-interactive
kubectl get nodes
```

La salida de ejemplo siguiente muestra el nodo de máquina virtual único creado y luego el nodo virtual para Linux, *virtual-node-aci-linux*:

```
$ kubectl get nodes

NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Implementación de una aplicación de ejemplo

En Azure Cloud Shell, cree un archivo denominado `virtual-node.yaml` y cópielo en el siguiente código YAML. Para programar el contenedor en el nodo, se definen [nodeSelector][node-selector] y [toleration][toleration]. Estas configuraciones permiten que el pod se programe en el nodo virtual y confirmen que la característica está habilitada correctamente.

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
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

Ejecute la aplicación con el comando [kubectl apply][kubectl-apply].

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

Use el comando [kubectl get pods][kubectl-get] con el argumento `-o wide` para generar una lista de pods y el nodo programado. Observe que el pod `virtual-node-helloworld` se ha programado en el nodo `virtual-node-linux`.

```
$ kubectl get pods -o wide

NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Se asigna una dirección IP interna al pod de la subred de red virtual de Azure delegada para su uso con los nodos virtuales.

> [!NOTE]
> Si usa imágenes almacenadas en Azure Container Registry, [configure y use un secreto de Kubernetes][acr-aks-secrets]. Una limitación actual de la versión preliminar de los nodos virtuales es que no se puede usar la autenticación de entidad de servicio de Azure AD integrada. Si no usa un secreto, los pods programados en los nodos virtuales no se pueden iniciar y se notifica el error `HTTP response status code 400 error code "InaccessibleImage"`.

## <a name="test-the-virtual-node-pod"></a>Prueba del pod del nodo virtual

Para probar el pod que se ejecuta en el nodo virtual, vaya a la aplicación de demostración con un cliente web. Como se asigna una dirección IP interna al pod, puede probar rápidamente esta conectividad desde otro pod en el clúster de AKS. Cree un pod de prueba y asóciele una sesión de terminal:

```azurecli-interactive
kubectl run -it --rm virtual-node-test --image=debian
```

Instale `curl` en el pod mediante `apt-get`:

```azurecli-interactive
apt-get update && apt-get install -y curl
```

Ahora acceda a la dirección de su pod mediante `curl`, como *http://10.241.0.4*. Proporcione su propia dirección IP interna mostrada en el comando `kubectl get pods` anterior:

```azurecli-interactive
curl -L http://10.241.0.4
```

Se muestra la aplicación de demostración, tal como se muestra en la siguiente salida de ejemplo reducida:

```
$ curl -L 10.241.0.4

<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Cierre la sesión de terminal en su pod de prueba con `exit`. Cuando la sesión finaliza, el pod se elimina.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, se programó un pod en el nodo virtual y se le asignó una dirección IP privada o interna. En su lugar, podría crear una implementación de servicio y enrutar el tráfico a su pod a través de un equilibrador de carga o controlador de entrada. Para más información, vea [Create a basic ingress controller in AKS][aks-basic-ingress] (Creación de un controlador de entrada básico en AKS).

Los nodos virtuales son un componente de una solución de escalado en AKS. Para más información sobre soluciones de escalado, consulte los siguientes artículos:

- [Tutorial: Escalado de aplicaciones en Azure Kubernetes Service (AKS)][aks-hpa]
- [Escalador automático en Azure Kubernetes Service (AKS): Versión preliminar][aks-cluster-autoscaler]
- [Consulte el ejemplo de escalado automático para los nodos virtuales][virtual-node-autoscale]
- [Obtenga más información acerca de la biblioteca de código abierto Virtual Kubelet][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[acr-aks-secrets]: ../container-registry/container-registry-auth-aks.md#access-with-kubernetes-secret
