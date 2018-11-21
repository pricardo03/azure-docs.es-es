---
title: Actualización y reinicio de nodos con Kured en Azure Kubernetes Service (AKS)
description: Aprenda a actualizar los nodos y a reiniciarlos automáticamente con Kured en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 11/06/2018
ms.author: iainfou
ms.openlocfilehash: 0bcc49df6540b73b8feb5bb1ec4312e680572797
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617813"
---
# <a name="apply-security-and-kernel-updates-to-nodes-in-azure-kubernetes-service-aks"></a>Aplicación de actualizaciones de kernel y seguridad a los nodos en Azure Kubernetes Service (AKS)

Para proteger los clústeres, las actualizaciones de seguridad se aplican automáticamente a los nodos en AKS. Estas actualizaciones incluyen las revisiones de seguridad del sistema operativo o las actualizaciones del kernel. Algunas de estas actualizaciones requieren un reinicio del nodo para completar el proceso. AKS no reinicia automáticamente los nodos para completar el proceso de actualización.

Este artículo muestra cómo usar el código abierto [Kured (demonio de reinicio de Kubernetes)][kured] para buscar nodos que requieran un reinicio, y a continuación administrar automáticamente la reprogramación de pods en ejecución y el proceso de reinicio de nodos.

> [!NOTE]
> `Kured` es un proyecto de código abierto de Weaveworks. La asistencia para este proyecto en AKS se proporciona dentro de lo posible. Se puede encontrar soporte técnico adicional en el canal de Slack #weave-community,

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, vea la guía de inicio rápido AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También es preciso que esté instalada y configurada la versión 2.0.49 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>Información sobre la experiencia de actualización del nodo AKS

En un clúster de AKS, los nodos de Kubernetes se ejecutan como máquinas virtuales de Azure. Estas máquinas virtuales basadas en Linux, usan una imagen de Ubuntu, con el sistema operativo configurado para buscar automáticamente actualizaciones cada noche. Si hay disponibles actualizaciones de kernel o de seguridad, estas se descargan e instalan automáticamente.

![Actualización de nodo AKS y proceso de reinicio con Kured](media/node-updates-kured/node-reboot-process.png)

Algunas actualizaciones de seguridad, como las actualizaciones del kernel, requieren un reinicio del nodo para finalizar el proceso. Un nodo que requiere un reinicio crea un archivo denominado */var/run/reboot-required*. Este proceso de reinicio no se realiza automáticamente.

Puede usar sus propios flujos de trabajo y procesos para controlar los reinicios de nodo, o usar `kured` para coordinar el proceso. Con `kured`, se implementa un [DaemonSet][DaemonSet] que ejecuta un pod en cada nodo del clúster. Estos pods en el DaemonSet comprueban si existe el archivo */var/run/reboot-required* y, a continuación, inician un proceso para reiniciar los nodos.

### <a name="node-upgrades"></a>Actualizaciones de nodo

Hay un proceso adicional en AKS que le permite *actualizar* un clúster. Una actualización consiste generalmente en el cambio a una versión más reciente de Kubernetes, no solo en aplicar las actualizaciones de seguridad de nodo. Una actualización de AKS realiza las acciones siguientes:

* Se implementa un nuevo nodo con las últimas actualizaciones de seguridad y se aplica la versión de Kubernetes.
* Se acordona y vacía un nodo anterior.
* Los pods se programan en el nuevo nodo.
* El nodo anterior se elimina.

No puede permanecer en la misma versión de Kubernetes durante un evento de actualización. Tiene que especificar una versión más reciente de Kubernetes. Para actualizar a la versión más reciente de Kubernetes, puede [actualizar el clúster de AKS][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Implementación de Kured en un clúster de AKS

Para implementar el DaemonSet de `kured`, aplique el siguiente manifiesto YAML de ejemplo desde su página de proyecto de GitHub. Este manifiesto crea un rol y un rol de clúster, enlaces y una cuenta de servicio y, después, implementa el DaemonSet mediante `kured` versión 1.1.0, que es compatible con clústeres AKS 1.9 o posteriores.

```console
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.1.0/kured-1.1.0.yaml
```

También puede configurar parámetros adicionales para `kured`, como la integración con Prometheus o Slack. Para más información acerca de los parámetros de configuración adicionales, consulte los [documentos de instalación de Kured][kured-install].

## <a name="update-cluster-nodes"></a>Actualización de nodos de clúster

De forma predeterminada, los nodos de AKS buscan actualizaciones cada noche. Si no desea esperar, puede realizar manualmente una actualización para comprobar que `kured` se ejecuta correctamente. En primer lugar, siga los pasos para [conectar con SSH a uno de los nodos AKS][aks-ssh]. Una vez que tenga una conexión SSH al nodo, compruebe si hay actualizaciones y aplíquelas como sigue:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Si se aplicaron actualizaciones que requieren un reinicio del nodo, se escribe un archivo en */var/run/reboot-required*. `Kured` comprueba si hay nodos que requieran un reinicio cada 60 minutos de forma predeterminada.

## <a name="monitor-and-review-reboot-process"></a>Supervisión y revisión del proceso de reinicio

Cuando una de las réplicas en el DaemonSet ha detectado que se requiere el reinicio de un nodo, se coloca un bloqueo en el nodo mediante la API de Kubernetes. Este bloqueo impide que se programen pods adicionales en el nodo. El bloqueo también indica que solo un nodo debe reiniciarse cada vez. Con el nodo acordonado, los pods en ejecución se purgan desde el nodo y este se reinicia.

Puede supervisar el estado de los nodos mediante el comando [kubectl get nodes][kubectl-get-nodes]. La salida del ejemplo siguiente muestra un nodo con el estado *SchedulingDisabled* mientras el nodo se prepara para el proceso de reinicio:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-79590246-2   Ready,SchedulingDisabled   agent     1h        v1.9.11
```

Una vez completado el proceso de actualización, puede ver el estado de los nodos mediante el comando [kubectl get nodes][kubectl-get-nodes] con el parámetro `--output wide`. Esta salida adicional le permite ver la diferencia en *KERNEL-VERSION* de los nodos subyacentes, como se muestra en la siguiente salida de ejemplo. El nodo *aks-nodepool1-79590246-2* se actualizó en un paso anterior y muestra la versión de kernel *4.15.0-1025-azure*. El nodo *aks-nodepool1-79590246-1* que no ha sido actualizado muestra la versión de kernel *4.15.0-1023-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-79590246-1   Ready     agent     1h        v1.9.11   10.240.0.6    <none>        Ubuntu 16.04.5 LTS   4.15.0-1023-azure   docker://1.13.1
aks-nodepool1-79590246-2   Ready     agent     1h        v1.9.11   10.240.0.4    <none>        Ubuntu 16.04.5 LTS   4.15.0-1025-azure   docker://1.13.1
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se ha detallado cómo utilizar `kured` para reiniciar nodos automáticamente como parte del proceso de actualización de seguridad. Para actualizar a la versión más reciente de Kubernetes, puede [actualizar el clúster de AKS][aks-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured#installation
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
