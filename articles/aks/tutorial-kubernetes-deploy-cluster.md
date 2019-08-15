---
title: 'Tutorial de Kubernetes en Azure: Implementación de un clúster'
description: En este tutorial de Azure Kubernetes Service (AKS), se crea un clúster de AKS y se usa kubectl para conectarse al nodo maestro de Kubernetes.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: dadab604e95c375e6f963f2d5eb9b619ddad7880
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69018852"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Tutorial: Implementación de un clúster de Azure Kubernetes Service (AKS)

Kubernetes proporciona una plataforma distribuida para aplicaciones en contenedores. Con AKS, puede crear rápidamente un clúster de Kubernetes listo para producción. En este tutorial, la tercera parte de siete, se implementa un clúster de Kubernetes en AKS. Aprenderá a:

> [!div class="checklist"]
> * Crear una entidad de servicio para las interacciones de los recursos
> * Implementar un clúster de AKS de Kubernetes
> * Instalar la CLI de Kubernetes (kubectl)
> * Configurar kubectl para conectarse a un clúster de AKS

En tutoriales adicionales, la aplicación Azure Vote se implementa en el clúster, se escala y se actualiza.

## <a name="before-you-begin"></a>Antes de empezar

En los tutoriales anteriores, se creó una imagen de contenedor y se actualizó en una instancia de Azure Container Registry. Si no ha realizado estos pasos, y desea continuar, inicie el [Tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

Para realizar este tutorial es necesario ejecutar la versión 2.0.53, o superior, de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Para permitir que un clúster de AKS interactúe con otros recursos de Azure, se usa una entidad de servicio de Azure Active Directory. Esta entidad de servicio puede crearse automáticamente mediante la CLI de Azure o el portal, o puede crear una previamente y asignar permisos adicionales. En este tutorial, creará una entidad de servicio, concederá acceso a la instancia de Azure Container Registry (ACR) creada en el tutorial anterior y, luego, creará un clúster de AKS.

Cree una entidad de servicio mediante el comando [az ad sp create-for-rbac][]. El parámetro `--skip-assignment` impide que se asignen permisos adicionales. De forma predeterminada, esta entidad de servicio es válida durante un año.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

La salida es similar a la del ejemplo siguiente:

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Anote el valor de *appId* y *password*. Estos valores se usan en los pasos siguientes.

## <a name="configure-acr-authentication"></a>Configuración de la autenticación de ACR

Para acceder a las imágenes almacenadas en ACR, debe conceder a la entidad de servicio de AKS los derechos adecuados para extraer imágenes de ACR.

En primer lugar, obtenga el identificador de recursos de ACR mediante [az acr show][]. Actualice el nombre del registro `<acrName>` con el de la instancia de ACR y el grupo de recursos donde se encuentra la instancia de ACR.

```azurecli
az acr show --resource-group myResourceGroup --name <acrName> --query "id" --output tsv
```

Para conceder el acceso correcto al clúster de AKS para extraer las imágenes almacenadas en ACR, asigne el rol `AcrPull` mediante el comando [az role assignment create][]. Reemplace `<appId`> y `<acrId>` por los valores recopilados en los dos pasos anteriores.

```azurecli
az role assignment create --assignee <appId> --scope <acrId> --role acrpull
```

## <a name="create-a-kubernetes-cluster"></a>Creación de un clúster de Kubernetes

Los clústeres de AKAS pueden usar los controles de acceso basado en roles (RBAC) de Kubernetes. Dichos controles permiten definir el acceso a los recursos en función de los roles asignados a los usuarios. Los permisos se combinan si a un usuario se le asignan varios roles y los permisos se pueden limitar a un espacio de nombres único o a todo el clúster. De forma predeterminada, la CLI de Azure habilita automáticamente RBAC al crear un clúster de AKS.

Cree un clúster de AKS mediante [az aks create][]. En el ejemplo siguiente se crea un clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*. Este grupo de recursos se creó en el [tutorial anterior][aks-tutorial-prepare-acr]. Use sus propios `<appId>` y `<password>` del paso anterior, en el que se creó la entidad de servicio.

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --service-principal <appId> \
    --client-secret <password> \
    --generate-ssh-keys
```

Al cabo de varios minutos, la implementación se completa y devuelve información en formato JSON sobre la implementación de AKS.

## <a name="install-the-kubernetes-cli"></a>Instalación de la CLI de Kubernetes

Para conectarse al clúster de Kubernetes desde un equipo cliente, use [kubectl][kubectl], el cliente de la línea de comandos de Kubernetes.

Si usa Azure Cloud Shell, `kubectl` ya está instalado. También lo puede instalar localmente. Para ello debe usar el comando [az aks install-cli][]:

```azurecli
az aks install-cli
```

## <a name="connect-to-cluster-using-kubectl"></a>Conexión a un clúster mediante kubectl

Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][]. En el ejemplo siguiente se obtienen las credenciales del clúster de AKS llamado *myAKSCluster* en el grupo de recursos *myResourceGroup*:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Para comprobar la conexión al clúster, ejecute el comando [kubectl get nodes][kubectl-get]:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-12345678-0   Ready    agent   32m   v1.13.9
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se ha implementado un clúster de Kubernetes en AKS y se ha configurado `kubectl` para conectarse a él. Ha aprendido a:

> [!div class="checklist"]
> * Crear una entidad de servicio para las interacciones de los recursos
> * Implementar un clúster de AKS de Kubernetes
> * Instalar la CLI de Kubernetes (kubectl)
> * Configurar kubectl para conectarse a un clúster de AKS

Vaya al siguiente tutorial para aprender a implementar una aplicación en el clúster.

> [!div class="nextstepaction"]
> [Implementación de una aplicación en Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-cli-install]: /cli/azure/install-azure-cli
