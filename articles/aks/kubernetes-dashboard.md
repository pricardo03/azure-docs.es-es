---
title: Administración de un clúster de Azure Kubernetes con una interfaz de usuario web
description: Obtenga información sobre cómo usar el panel integrado de la interfaz de usuario web de Kubernetes con Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 65525114f46002c5b9300f6bbabcee06cc27ef3a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091145"
---
# <a name="access-the-kubernetes-dashboard-with-azure-kubernetes-service-aks"></a>Acceso al panel de Kubernetes con Azure Kubernetes Service (AKS)

Kubernetes incluye un panel web que se puede usar para operaciones básicas de administración. Este artículo muestra cómo acceder al panel de Kubernetes mediante la CLI de Azure y luego lo guía por algunas operaciones básicas del panel. Para más información sobre el panel de Kubernetes, consulte [Kubernetes Web UI Dashboard][kubernetes-dashboard] (Panel de la interfaz de usuario web de Kubernetes).

## <a name="before-you-begin"></a>Antes de empezar

En los pasos que se detallan en este documento se presume que se ha creado un clúster de AKS y que ha establecido una conexión `kubectl` con dicho clúster. Si necesita crear un clúster de AKS, consulte el [inicio rápido de AKS][aks-quickstart].

También es preciso que esté instalada y configurada la versión 2.0.27 de la CLI de Azure u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Inicio del panel de Kubernetes

Para iniciar el panel de Kubernetes, use el comando [az aks browse][az-aks-browse]. El ejemplo siguiente abre el panel para el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Este comando crea a un proxy entre el sistema de desarrollo y la API de Kubernetes y abre un explorador web en el panel de Kubernetes.

### <a name="for-rbac-enabled-clusters"></a>Para los clústeres habilitados para RBAC

Si el clúster de AKS usa RBAC, se debe crear un *ClusterRoleBinding* antes de que pueda acceder correctamente al panel. Para crear un enlace, use el comando [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding] como se muestra en el ejemplo siguiente. 

> [!WARNING]
> Este enlace de ejemplo no aplica ningún componente de autenticación adicional y puede dar lugar a un uso no seguro. El panel de Kubernetes está abierto a cualquier persona con acceso a la dirección URL. No exponga públicamente el panel de Kubernetes.
>
> Puede usar mecanismos como los tokens de portador o un nombre de usuario y una contraseña para controlar quién puede acceder al panel y qué permisos tienen. Esto permite un uso más seguro del panel. Para más información sobre cómo usar los distintos métodos de autenticación, consulte la wiki del panel de Kubernetes sobre los [controles de acceso][dashboard-authentication].

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Ahora puede acceder al panel de Kubernetes en el clúster habilitado para RBAC. Para iniciar el panel de Kubernetes, use el comando [az aks browse][az-aks-browse] como se detalló en el paso anterior.

## <a name="run-an-application"></a>Ejecución de una aplicación

En el panel de Kubernetes, haga clic en el botón **Create** (Crear) de la ventana derecha superior. Asigne el nombre `nginx` a la implementación y escriba `nginx:latest` para el nombre de las imágenes de contenedor. En **Service** (Servicio), seleccione **External** (Externo) y escriba `80` para el puerto y el puerto de destino.

Cuando esté listo, haga clic en **Deploy** (Implementar) para crear la implementación.

![Cuadro de diálogo de creación del servicio de Kubernetes](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Visualización de la aplicación

El estado de la aplicación puede verse en el panel de Kubernetes. Una vez que se ejecuta la aplicación, cada componente tiene una casilla verde junto a ella.

![Pod de Kubernetes](./media/container-service-kubernetes-ui/complete-deployment.png)

Para más información sobre los pods de aplicación, haga clic en **Pod** en el menú izquierdo y, luego, seleccione el pod **NGINX**. Aquí puede ver información específica del pod como el consumo de recursos.

![Recursos de Kubernetes](./media/container-service-kubernetes-ui/running-pods.png)

Para buscar la dirección IP de la aplicación, haga clic en **Services** (Servicios) en el menú izquierdo y, luego, seleccione el servicio **NGINX**.

![vista de nginx](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Edición de aplicación

Además de crear y visualizar las aplicaciones, el panel de Kubernetes puede utilizarse para editar y actualizar las implementaciones de aplicaciones.

Para editar una implementación, haga clic en **Deployments** (Implementaciones) en el menú izquierdo y, luego, seleccione la implementación **NGINX**. Por último, seleccione **Edit** (Editar) en la barra de navegación superior derecha.

![Edición de Kubernetes](./media/container-service-kubernetes-ui/view-deployment.png)

Busque el valor `spec.replica`, que debe ser 1, y cambie este valor a 3. Si lo hace, el número de réplicas de la implementación NGINX aumenta de 1 a 3.

Seleccione **Update** (Actualizar) cuando esté listo.

![Edición de Kubernetes](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el panel de Kubernetes, vea la documentación de Kubernetes.

> [!div class="nextstepaction"]
> [Kubernetes Web UI Dashboard][kubernetes-dashboard] (Panel de la interfaz de usuario web de Kubernetes)

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
