---
title: 'Tutorial de Kubernetes en Azure: implementación del clúster'
description: 'Tutorial de AKS: implementación del clúster'
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8698f16138e9baeb9c9c1142a5d0c8937a69d1b
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341406"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Tutorial: Implementación de un clúster de Azure Kubernetes Service (AKS)

Kubernetes proporciona una plataforma distribuida para aplicaciones en contenedores. Con AKS, puede aprovisionar rápidamente un clúster de Kubernetes listo para producción. En este tutorial, la tercera parte de siete, se implementa un clúster de Kubernetes en AKS. Los pasos completados incluyen:

> [!div class="checklist"]
> * Creación de una entidad de servicio para las interacciones de recursos
> * Implementación de un clúster de AKS de Kubernetes
> * Instalación de la CLI de Kubernetes (kubectl)
> * Configuración de kubectl

En los tutoriales posteriores, la aplicación Azure Vote se implementa en el clúster, se escala y se actualiza.

## <a name="before-you-begin"></a>Antes de empezar

En los tutoriales anteriores, se creó una imagen de contenedor y se actualizó en una instancia de Azure Container Registry. Si no ha realizado estos pasos, pero desea continuar, vuelva al [tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Para permitir que un clúster de AKS interactúe con otros recursos de Azure, se usa una entidad de servicio de Azure Active Directory. Esta entidad de servicio puede crearse automáticamente mediante la CLI de Azure o el portal, o puede crear una previamente y asignar permisos adicionales. En este tutorial, creará una entidad de servicio, concederá acceso a la instancia de Azure Container Registry (ACR) creada en el tutorial anterior y, luego, creará un clúster de AKS.

Cree una entidad de servicio con [az ad sp create-for-rbac][]. El parámetro `--skip-assignment` impide que se asignen permisos adicionales.

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

En primer lugar, obtenga el identificador de recursos de ACR con [az acr show][]. Actualice el nombre del registro `<acrName>` con el de la instancia de ACR y el grupo de recursos donde se encuentra la instancia de ACR.

```azurecli
az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv
```

Para conceder el acceso correcto al clúster de AKS para usar las imágenes almacenadas en ACR, cree una asignación de roles con [az role assignment create][]. Reemplace `<appId`> y `<acrId>` por los valores recopilados en los dos pasos anteriores.

```azurecli
az role assignment create --assignee <appId> --role Reader --scope <acrId>
```

## <a name="create-kubernetes-cluster"></a>Creación de un clúster de Kubernetes

Ahora cree un clúster de AKS con [az aks create][]. En el ejemplo siguiente se crea un clúster denominado *myAKSCluster* en un grupo de recursos denominado *myResourceGroup*. Este grupo de recursos se creó en el [tutorial anterior][aks-tutorial-prepare-acr]. Proporcione sus propios valores de `<appId>` y `<password>` del paso anterior en que creó la entidad de servicio.

```azurecli
az aks create \
    --name myAKSCluster \
    --resource-group myResourceGroup \
    --node-count 1 \
    --generate-ssh-keys \
    --service-principal <appId> \
    --client-secret <password>
```

Al cabo de varios minutos, la implementación se completa y devuelve información en formato JSON sobre la implementación de AKS.

## <a name="install-the-kubectl-cli"></a>Instalación de la CLI de kubectl

Para conectarse al clúster de Kubernetes desde el equipo cliente, use [kubectl][kubectl], el cliente de la línea de comandos de Kubernetes.

Si usa Azure Cloud Shell, kubectl ya está instalado. También puede instalarlo localmente con [az aks install-cli][]:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Conexión con kubectl

Para configurar kubectl para conectarse al clúster de Kubernetes, use [az aks get-credentials][]. En el ejemplo siguiente se obtienen las credenciales del nombre del clúster de AKS *myAKSCluster* en el grupo de recursos *myResourceGroup*:

```azurecli
az aks get-credentials --name myAKSCluster --resource-group myResourceGroup
```

Para comprobar la conexión al clúster, ejecute el comando [kubectl get nodes][kubectl-get].

```azurecli
kubectl get nodes
```

Salida:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-66427764-0   Ready     agent     9m        v1.9.6
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se implementó un clúster de Kubernetes en AKS. Se han completado los siguientes pasos:

> [!div class="checklist"]
> * Creación de una entidad de servicio para las interacciones de recursos
> * Implementación de un clúster de AKS de Kubernetes
> * Instalación de la CLI de Kubernetes (kubectl)
> * Configuración de kubectl

Avance al siguiente tutorial para aprender a ejecutar la aplicación en el clúster.

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
