---
title: Administración de un clúster de Azure Kubernetes Service con el panel web
description: Obtenga información sobre cómo usar el panel integrado de la interfaz de usuario web de Kubernetes para administrar un clúster de Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 9d953cdb82412c07fe0ed4bef75dece4a929cad9
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067596"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Acceso al panel web de Kubernetes en Azure Kubernetes Service (AKS)

Kubernetes incluye un panel web que se puede usar para operaciones básicas de administración. Este panel le permite ver el estado de mantenimiento básico y las métricas para sus aplicaciones, crear e implementar servicios, y modificar las aplicaciones existentes. Este artículo muestra cómo acceder al panel de Kubernetes mediante la CLI de Azure y luego lo guía por algunas operaciones básicas del panel.

Para más información sobre el panel de Kubernetes, consulte [Kubernetes Web UI Dashboard][kubernetes-dashboard] (Panel de la interfaz de usuario web de Kubernetes).

## <a name="before-you-begin"></a>Antes de empezar

En los pasos que se detallan en este documento se da por hecho que ha creado un clúster de AKS y que ha establecido una conexión `kubectl` con dicho clúster. Si necesita crear un clúster de AKS, consulte el [inicio rápido de AKS][aks-quickstart].

También es preciso que esté instalada y configurada la versión 2.0.46 de la CLI de Azure u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Inicio del panel de Kubernetes

Para iniciar el panel de Kubernetes, use el comando [az aks browse][az-aks-browse]. El ejemplo siguiente abre el panel para el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Este comando crea a un proxy entre el sistema de desarrollo y la API de Kubernetes y abre un explorador web en el panel de Kubernetes. Si un explorador web no se abre en el panel de Kubernetes, copie y pegue la dirección URL que anotó en la CLI de Azure, normalmente *http://127.0.0.1:8001*.

![Página de información general del panel web de Kubernetes](./media/kubernetes-dashboard/dashboard-overview.png)

### <a name="for-rbac-enabled-clusters"></a>Para los clústeres habilitados para RBAC

Si el clúster de AKS usa RBAC, se debe crear un *ClusterRoleBinding* antes de que pueda acceder correctamente al panel. De forma predeterminada, el panel de Kubernetes se implementa con un mínimo acceso de lectura y muestra los errores de acceso del RBAC. El panel de Kubernetes no admite actualmente las credenciales proporcionadas por el usuario para determinar el nivel de acceso, sino que utiliza los roles que se conceden a la cuenta de servicio. Un administrador de clústeres puede conceder acceso adicional a la cuenta de servicio *kubernetes-dashboard*, aunque esto puede ser un vector de elevación de privilegios. También se puede integrar la autenticación de Azure Active Directory para proporcionar un nivel de acceso más pormenorizado.

Para crear un enlace, use el comando [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding] como se muestra en el ejemplo siguiente. 

> [!WARNING]
> Este enlace de ejemplo no aplica ningún componente de autenticación adicional y puede dar lugar a un uso no seguro. El panel de Kubernetes está abierto a cualquier persona con acceso a la dirección URL. No exponga públicamente el panel de Kubernetes.
>
> Para más información sobre cómo usar los distintos métodos de autenticación, consulte la wiki del panel de Kubernetes sobre los [controles de acceso][dashboard-authentication].

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Ahora puede acceder al panel de Kubernetes en el clúster habilitado para RBAC. Para iniciar el panel de Kubernetes, use el comando [az aks browse][az-aks-browse] como se detalló en el paso anterior.

## <a name="create-an-application"></a>Creación de una aplicación

Para ver cómo el panel de Kubernetes puede reducir la complejidad de las tareas de administración, vamos a crear una aplicación. Puede crear una aplicación desde el panel de Kubernetes, ya sea proporcionando la entrada de texto, un archivo YAML o mediante un asistente gráfico.

Para crear una aplicación, realice los pasos siguientes:

1. Seleccione el botón **Crear** de la ventana superior derecha.
1. Para usar el asistente para gráficos, elija **Crear una aplicación**.
1. Proporcione un nombre para la implementación, por ejemplo *nginx*
1. Escriba el nombre de la imagen de contenedor que se utilizará, como *nginx:1.15.5*
1. Para exponer el puerto 80 para el tráfico web, cree un servicio de Kubernetes. En **Service** (Servicio), seleccione **External** (Externo) y escriba **80** tanto para el puerto como para el puerto de destino.
1. Cuando esté preparado, seleccione **Implementar** para crear la aplicación.

![Implementación de una aplicación en el panel web de Kubernetes](./media/kubernetes-dashboard/create-app.png)

Se tarda un minuto o dos en que una dirección IP externa pública se asigne al servicio de Kubernetes. A la izquierda, bajo **Discovery and Load Balancing** (Detección y equilibrio de carga), seleccione **Servicios**. Se muestra el servicio de la aplicación, incluidos los *puntos de conexión externos*, como se muestra en el ejemplo siguiente:

![Visualización de la lista de servicios y puntos de conexión](./media/kubernetes-dashboard/view-services.png)

Seleccione la dirección de punto de conexión para abrir una ventana del explorador web con la página predeterminada de NGINX:

![Visualización de la página predeterminada de NGINX de la aplicación implementada](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Visualización de la información de pod

El panel de Kubernetes puede proporcionar métricas de supervisión básicas e información para solución de problemas como los registros.

Para más información acerca de los pods de aplicación, seleccione **Pods** en el menú izquierdo. Se muestra una lista de los pods disponibles. Elija su pod *nginx* para ver información, por ejemplo, del consumo de recursos:

![Visualización de la información de pod](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Edición de aplicación

Además de crear y visualizar las aplicaciones, el panel de Kubernetes puede utilizarse para editar y actualizar las implementaciones de aplicaciones. Para proporcionar redundancia adicional para la aplicación, vamos a aumentar el número de réplicas de NGINX.

Para editar una implementación:

1. Seleccione **Deployments** (Implementaciones) en el menú izquierdo y, a continuación, elija su implementación de *nginx*.
1. Seleccione **Edit** (Editar) en la barra de navegación superior derecha.
1. Busque el valor de `spec.replica`, en torno a la línea 20. Para aumentar el número de réplicas de la aplicación, cambie este valor de *1* a *3*.
1. Seleccione **Update** (Actualizar) cuando esté listo.

![Edición de la implementación para actualizar el número de réplicas](./media/kubernetes-dashboard/edit-deployment.png)

Se tarda unos minutos en que los nuevos pods se creen dentro de un conjunto de réplicas. En el menú izquierdo, elija **Replica Sets** (Conjuntos de réplicas) y, a continuación, elija su conjunto de réplicas de *nginx*. La lista de los pods ahora refleja el número de réplicas actualizado, como se muestra en la salida del ejemplo siguiente:

![Visualización de información sobre el conjunto de réplicas](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el panel de Kubernetes, vea el [panel de la interfaz de usuario web de Kubernetes][kubernetes-dashboard].

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
