---
title: Usar el escalado automático de clústeres en Azure Kubernetes Service (AKS)
description: Aprenda a usar el escalado automático de clústeres para escalar automáticamente el clúster con el fin de satisfacer las necesidades de su aplicación en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/18/2019
ms.author: mlearned
ms.openlocfilehash: 033cf88e29ba4a9f7ce9397fe216f7380e70be07
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76713403"
---
# <a name="automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Escalar automáticamente un clúster para satisfacer las necesidades de la aplicación en Azure Kubernetes Service (AKS)

Para satisfacer las necesidades de la aplicación en Azure Kubernetes Service (AKS), es posible que deba ajustar el número de nodos que ejecutan las cargas de trabajo. El componente de escalado automático de clústeres puede supervisar los pods del clúster que no pueden programarse debido a las restricciones de los recursos. Cuando se detectan problemas, la cantidad de nodos de un grupo de nodos aumenta para satisfacer las necesidades de la aplicación. Asimismo, los nodos también se comprueban regularmente para detectar la falta de pods en ejecución y, en consecuencia, la cantidad de nodos se reduce según sea necesario. Esta capacidad de ampliar o reducir automáticamente la cantidad de nodos en su clúster de AKS le permite ejecutar un clúster de forma eficaz y rentable.

En este artículo se muestra cómo habilitar y administrar el escalado automático de clústeres en un clúster de AKS. 

## <a name="before-you-begin"></a>Antes de empezar

Para este artículo es preciso usar la versión 2.0.76 de la CLI de Azure, o cualquier versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="limitations"></a>Limitaciones

Se aplican las siguientes limitaciones cuando crea y administra clústeres de AKS que usan la escalabilidad automática de clúster:

* El complemento de enrutamiento de aplicación HTTP no se puede utilizar.

## <a name="about-the-cluster-autoscaler"></a>Acerca del escalado automático de clústeres

Para adaptarse a las cambiantes necesidades de las aplicaciones, como detallar los horarios entre la jornada laboral y la noche o un fin de semana, los clústeres a menudo necesitan una forma de realizar automáticamente el escalado. Los clústeres de AKS pueden escalarse mediante una de dos maneras:

* El **escalado automático de clústeres** busca pods que no puedan programarse en nodos debido a las restricciones de los recursos. Luego, el clúster aumenta automáticamente la cantidad de nodos.
* El **escalado automático horizontal de pods** usa el servidor de métricas en un clúster de Kubernetes para supervisar la demanda de recursos de los pods. Si una aplicación necesita más recursos, el número de pods aumenta automáticamente para satisfacer la demanda.

![El escalado automático de clústeres y el escalado automático horizontal de pods colaboran a menudo para responder a las exigencias de la aplicación.](media/autoscaler/cluster-autoscaler.png)

Tanto Horizontal Pod Autoscaler como Cluster Autoscaler pueden reducir el número de pods y nodos según sea necesario. El escalado automático de clústeres reduce el número de nodos cuando no se usa toda la capacidad durante un período de tiempo. Los pods de un nodo que quitará el escalado automático de clústeres se programan con seguridad en otra parte del clúster. El escalado automático de clústeres no se puede reducir verticalmente si no se pueden mover los pods, tal como se detalla en las situaciones siguientes:

* Un pod se crea directamente y no lo respalda un objeto de controlador, como una implementación o un conjunto de réplicas.
* Un presupuesto de interrupciones de pods (PDB) es demasiado restrictivo y no permite que la cantidad de pods sea menor que el umbral definido.
* Un pod usa selectores de nodo o antiafinidad que no se pueden respetar si se programan en un nodo diferente.

Para obtener más información acerca de cómo el escalado automático de clústeres no puede realizar reducciones verticales, consulte [What types of pods can prevent the cluster autoscaler from removing a node?][autoscaler-scaledown] (¿Qué tipos de pods pueden evitar que el escalado automático de clústeres elimine un nodo?)

El escalado automático de clústeres usa parámetros de inicio para cosas como intervalos de tiempo entre eventos de escalado y umbrales de recursos. Estos parámetros se definen mediante la plataforma de Azure y actualmente no se pueden ajustar. Para obtener más información sobre los parámetros que usa el escalado automático de clústeres, consulte [What are the cluster autoscaler parameters?][autoscaler-parameters] (¿Cuáles son los parámetros de clúster del escalado automático de clústeres?).

Horizontal Pod Autoscaler y Cluster Autoscaler pueden funcionar juntos y a menudo se implementan en un clúster. Cuando se combinan, el escalado automático horizontal de pods se centra en ejecutar el número de pods necesario para satisfacer las exigencias de la aplicación. El escalado automático de clústeres se centra en ejecutar el número de nodos necesario para admitir los pods programados.

> [!NOTE]
> El escalado manual está deshabilitado cuando se usa el escalado automático de clústeres. Permita que el escalado automático de clústeres determine el número de nodos. Si quiere escalar manualmente el clúster, [deshabilite el escalado automático de clústeres](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Crear un clúster de AKS y habilitar el escalado automático de clústeres

Si necesita crear un clúster de AKS, use el comando [az aks create][az-aks-create]. Para habilitar y configurar el escalado automático de clústeres en el grupo de nodos del clúster, use el parámetro *--enable-cluster-autoscaler* y especifique un nodo *--min-count* y *--max-count*.

> [!IMPORTANT]
> El escalador automático del clúster es un componente de Kubernetes. Aunque el clúster de AKS usa un conjunto de escalado de máquinas virtuales para los nodos, no habilite ni edite manualmente la configuración de escalado automático del conjunto de escalado en Azure Portal o mediante la CLI de Azure. Permita que el escalador automático del clúster de Kubernetes administre la configuración del escalado necesaria. Para más información, consulte [¿Puedo modificar los recursos de AKS en el grupo de recursos del nodo?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group)

En el ejemplo siguiente se crea un clúster de AKS con un único grupo de nodos respaldado por un conjunto de escalado de máquinas virtuales. También habilita la escalabilidad automática del clúster en el grupo de nodos para el clúster y establece un mínimo de *1* y un máximo de *3* nodos:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Tardará unos minutos en crear el clúster y configurar las opciones del escalado automático de clústeres.

## <a name="change-the-cluster-autoscaler-settings"></a>Cambiar la configuración del escalado automático de clústeres

> [!IMPORTANT]
> Si tiene varios grupos de nodos en el clúster de AKS, vaya a la sección de [escalado automático con varios grupos de agentes](#use-the-cluster-autoscaler-with-multiple-node-pools-enabled). Los clústeres con varios grupos de agentes habilitados requieren el uso del conjunto de comandos `az aks nodepool` para cambiar las propiedades específicas del grupo de nodos en lugar de `az aks`.

En el paso anterior para crear un clúster de AKS o actualizar un grupo de nodos existente, el número mínimo de nodos del escalado automático de clústeres se estableció en *1*, y el número máximo de nodos se estableció en *3*. Como las necesidades de la aplicación van cambiando, es posible que deba ajustar el número de nodos del escalado automático de clústeres.

Para cambiar el número de nodos, use el comando [az aks update][az-aks-update].

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

En el ejemplo anterior se actualiza el escalado automático del clúster en el único grupo de nodos de *myAKSCluster* a un mínimo de *1* y un máximo de *5* nodos.

> [!NOTE]
> No se puede establecer un número mínimo de nodos que sea mayor que el establecido para el grupo de nodos. Por ejemplo, si actualmente tiene el mínimo establecido en *1*, no se puede actualizar a *3*.

Supervise el rendimiento de las aplicaciones y los servicios y ajuste la cantidad de nodos del escalado automático de clústeres para que coincida con el rendimiento necesario.

## <a name="disable-the-cluster-autoscaler"></a>Deshabilitar el escalado automático de clústeres

Si ya no quiere usar el escalado automático de clústeres, puede deshabilitarlo mediante el comando [az aks update][az-aks-update], especificando el parámetro *--disable-cluster-autoscaler*. Los nodos no se quitan cuando se deshabilita el escalado automático de clústeres.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Puede escalar manualmente el clúster después de deshabilitar el escalado automático del clúster mediante el comando [az aks scale][az-aks-scale]. Si usa el escalado automático horizontal de pods, esa característica continúa ejecutándose con el escalado automático de clústeres deshabilitado, pero es posible que los pods no puedan programarse si todos los recursos de nodo están en uso.

## <a name="re-enable-a-disabled-cluster-autoscaler"></a>Volver a habilitar un escalado automático de clústeres deshabilitado

Si quiere volver a habilitar el escalado automático de clústeres en un clúster existente, puede volver a habilitarlo mediante el comando [az aks update][az-aks-update], especificando los parámetros *--enable-cluster-autoscaler*, *--min-count* y *--max-count*.

## <a name="retrieve-cluster-autoscaler-logs-and-status"></a>Recuperación de registros y estado del escalador automático del clúster

Para diagnosticar y depurar los eventos del escalador automático, los registros y el estado se pueden recuperar desde el complemento del escalador automático.

AKS administra el escalador automático del clúster en su nombre y lo ejecuta en el plano de control administrado. Los registros del nodo principal deben estar configurados para que se puedan ver como resultado.

Para configurar los registros que se van a insertar desde el escalador automático del clúster en Log Analytics, siga estos pasos.

1. Configure una regla para que los registros de diagnóstico inserten registros del escalador automático del clúster en Log Analytics. [Las instrucciones se detallan aquí](https://docs.microsoft.com/azure/aks/view-master-logs#enable-diagnostics-logs), asegúrese de activar la casilla correspondiente a `cluster-autoscaler` al seleccionar las opciones para "Registros".
1. Haga clic en la sección "Registros" en el clúster mediante Azure Portal.
1. Escriba la consulta de ejemplo siguiente en Log Analytics:

```
AzureDiagnostics
| where Category == "cluster-autoscaler"
```

Debería ver devueltos registros similares a los siguientes, siempre y cuando haya registros para recuperar.

![Registros de Log Analytics](media/autoscaler/autoscaler-logs.png)

El escalador automático del clúster también escribirá el estado de mantenimiento en un archivo ConfigMap llamado `cluster-autoscaler-status`. Para recuperar estos registros, ejecute el siguiente comando `kubectl`. Se informará del estado de mantenimiento para cada grupo de nodos configurado con el escalador automático del clúster.

```
kubectl get configmap -n kube-system cluster-autoscaler-status -o yaml
```

Para más información sobre lo que se registra del escalador automático, consulte las preguntas más frecuentes sobre el [proyecto de GitHub Kubernetes/autoscaler](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#ca-doesnt-work-but-it-used-to-work-yesterday-why).

## <a name="use-the-cluster-autoscaler-with-multiple-node-pools-enabled"></a>Uso del escalado automático de clústeres con varios grupos de nodos habilitados

El escalado automático de clústeres también se puede usar con [varios grupos de nodos](use-multiple-node-pools.md) habilitados. Siga este documento para aprender a habilitar varios grupos de nodos y agregar grupos de nodos adicionales a un clúster existente. Cuando se utilizan ambas características juntas, se activa el escalado automático de clústeres en cada grupo de nodos individuales del clúster y se pueden pasar reglas de escalado automático únicas a cada uno de ellos.

En el comando siguiente se supone que ha seguido las [instrucciones iniciales](#create-an-aks-cluster-and-enable-the-cluster-autoscaler) anteriores de este documento y desea actualizar el número máximo de grupos de nodos existentes de *3* a *5*. Use el comando [az aks nodepool update][az-aks-nodepool-update] para actualizar la configuración de un grupo de nodos existente.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name nodepool1 \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

El escalado automático de clústeres se puede deshabilitar con [az aks nodepool update][az-aks-nodepool-update] y al pasar el parámetro `--disable-cluster-autoscaler`.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name nodepool1 \
  --disable-cluster-autoscaler
```

Si quiere volver a habilitar el escalado automático de clústeres en un clúster existente, puede volver a habilitarlo mediante el comando [az aks nodepool update][az-aks-nodepool-update], especificando los parámetros *--enable-cluster-autoscaler*, *--min-count* y *--max-count*.

## <a name="next-steps"></a>Pasos siguientes

En este artículo le mostramos cómo escalar automáticamente el número de nodos de AKS. Asimismo, también puede usar el escalado automático horizontal de pods para ajustar automáticamente el número de pods ejecutan la aplicación. Para obtener instrucciones sobre cómo usar el escalado automático horizontal de pods, consulte [Escalado de aplicaciones en AKS][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[az-aks-nodepool-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview#enable-cluster-auto-scaler-for-a-node-pool
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
