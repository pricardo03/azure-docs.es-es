---
title: Limitación del acceso a kubeconfig en Azure Kubernetes Service (AKS)
description: Aprenda a controlar el acceso al archivo de configuración de Kubernetes (kubeconfig) de los administradores y los usuarios de los clústeres
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/03/2019
ms.author: iainfou
ms.openlocfilehash: ae45a268536f6a8fcb4ab27336a1281837cf5dc7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54045493"
---
# <a name="use-azure-role-based-access-controls-to-define-access-to-the-kubernetes-configuration-file-in-azure-kubernetes-service-aks"></a>Uso de los controles de acceso basados en roles de Azure para definir el acceso al archivo de configuración de Kubernetes en Azure Kubernetes Service (AKS)

Puede interactuar con los clústeres de Kubernetes mediante la herramienta `kubectl`. La CLI de Azure proporciona una manera fácil de obtener las credenciales de acceso y la información de configuración para conectarse a los clústeres de AKS mediante `kubectl`. Para limitar quién puede obtener esa información de configuración de Kubernetes (*kubeconfig*) y los correspondientes permisos, puede usar los controles de acceso basados en roles (RBAC) de Azure.

En este artículo se muestra cómo asignar roles RBAC que limiten quién puede obtener la información de configuración para un clúster de AKS.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, vea la guía de inicio rápido AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

En este artículo también se requiere que ejecute la versión 2.0.53 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="available-cluster-roles-permissions"></a>Permisos de los roles de clúster disponibles

Al interactuar con un clúster de AKS mediante la herramienta `kubectl`, se usa un archivo de configuración que define la información de conexión del clúster. Este archivo de configuración se almacena normalmente en *~/.kube/config*. En este archivo *kubeconfig* se pueden definir varios clústeres. El comando [kubectl config use-context][kubectl-config-use-context] sirve para cambiar de un clúster a otro.

El comando [az aks get-credentials][az-aks-get-credentials] permite obtener las credenciales de acceso para un clúster de AKS y las combina en el archivo *kubeconfig*. Puede usar los controles de acceso basados en roles (RBAC) de Azure para controlar el acceso a estas credenciales. Estos roles RBAC de Azure permiten definir quién puede recuperar el archivo *kubeconfig* y los permisos correspondientes en el clúster.

Los dos roles integrados son:

* **Rol de administrador de clúster de Azure Kubernetes Service**  
    * Permite el acceso a la llamada API *Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action*. Esta llamada API [enumera las credenciales de administrador de clúster][api-cluster-admin].
    * Descarga el archivo *kubeconfig* para el rol *clusterAdmin*.
* **Rol de usuario de clúster de Azure Kubernetes Service**
    * Permite el acceso a la llamada API *Microsoft.ContainerService/managedClusters/listClusterUserCredential/action*. Esta llamada API [enumera las credenciales de usuario de clúster][api-cluster-user].
    * Descarga *kubeconfig* para el rol *clusterUser*.

## <a name="assign-role-permissions-to-a-user"></a>Asignación de permisos de rol a un usuario

Para asignar uno de los roles de Azure a un usuario, debe obtener el identificador de recurso del clúster de AKS y el de la cuenta de usuario. Los siguientes comandos de ejemplo realizan los siguientes pasos:

* Obtienen el identificador de recurso del clúster mediante el comando [az aks show][az-aks-show] para el clúster denominado *myAKSCluster* del grupo de recursos *myResourceGroup*. Proporcione los nombres de su propio clúster y grupo de recursos según proceda.
* Usan los comandos [az account show][az-account-show] y [az ad user show][az-ad-user-show] para obtener el identificador de usuario.
* Por último, asignan un rol mediante el comando [az role assignment create][az-role-assignment-create].

En el ejemplo siguiente se asigna el *rol de administrador de clústeres de Azure Kubernetes Service*:

```azurecli-interactive
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
ACCOUNT_UPN=$(az account show --query user.name -o tsv)
ACCOUNT_ID=$(az ad user show --upn-or-object-id $ACCOUNT_UPN --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $ACCOUNT_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"
```

Puede cambiar la asignación anterior al *rol Cluster User* cuando proceda.

La siguiente salida de ejemplo muestra que la asignación de roles se realizó correctamente:

```
{
  "canDelegate": null,
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/providers/Microsoft.Authorization/roleAssignments/b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "name": "b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "principalId": "946016dd-9362-4183-b17d-4c416d1f8f61",
  "resourceGroup": "myResourceGroup",
  "roleDefinitionId": "/subscriptions/<guid>/providers/Microsoft.Authorization/roleDefinitions/0ab01a8-8aac-4efd-b8c2-3ee1fb270be8",
  "scope": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-and-verify-the-configuration-information"></a>Obtención y verificación de la información de configuración

Con los roles RBAC asignados, use el comando [az aks get-credentials][az-aks-get-credentials] para obtener la definición de *kubeconfig* para el clúster de AKS. En el ejemplo siguiente se obtienen las credenciales *--admin*, que funcionan correctamente si se ha concedido al usuario el *rol Cluster Admin*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

A continuación, puede usar el comando [kubectl config view][kubectl-config-view] para verificar que el *contexto* del clúster muestra que se ha aplicado la información de configuración del administrador:

```
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://myaksclust-myresourcegroup-19da35-4839be06.hcp.eastus.azmk8s.io:443
  name: myAKSCluster
contexts:
- context:
    cluster: myAKSCluster
    user: clusterAdmin_myResourceGroup_myAKSCluster
  name: myAKSCluster-admin
current-context: myAKSCluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_myResourceGroup_myAKSCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: e9f2f819a4496538b02cefff94e61d35
```

## <a name="remove-role-permissions"></a>Eliminación de permisos de rol

Para eliminar asignaciones de rol, use el comando [az role assignment delete][az-role-assignment-delete]. Especifique el identificador de la cuenta y del recurso de clúster como los obtuvo con los comandos anteriores:

```azurecli-interactive
az role assignment delete --assignee $ACCOUNT_ID --scope $AKS_CLUSTER
```

## <a name="next-steps"></a>Pasos siguientes

Para mejorar la seguridad y el acceso a los clústeres de AKS, [integre la autenticación de Azure Active Directory][aad-integration].

<!-- LINKS - external -->
[kubectl-config-use-context]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config
[kubectl-config-view]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[api-cluster-admin]: /rest/api/aks/managedclusters/listclusteradmincredentials
[api-cluster-user]: /rest/api/aks/managedclusters/listclusterusercredentials
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-role-assignment-delete]: /cli/azure/role/assignment#az-role-assignment-delete
[aad-integration]: aad-integration.md
