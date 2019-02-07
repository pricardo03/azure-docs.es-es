---
title: Restablecer las credenciales de un clúster de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo actualizar o restablecer las credenciales de la entidad de servicio para un clúster en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/30/2019
ms.author: iainfou
ms.openlocfilehash: d880615d0d132403c935fe39e8478d7b3fc48dbe
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490080"
---
# <a name="update-or-rotate-the-credentials-for-a-service-principal-in-azure-kubernetes-service-aks"></a>Actualizar o rotar las credenciales de una entidad de servicio en Azure Kubernetes Service (AKS)

De forma predeterminada, los clústeres de AKS se crean con una entidad de servicio que tiene un período de expiración de un año. A medida que se aproxima a la fecha de expiración, puede restablecer las credenciales para ampliar la entidad de servicio durante un período de tiempo adicional. También puede actualizar o rotar las credenciales como parte de una directiva de seguridad definida. En este artículo se describe cómo actualizar estas credenciales para un clúster de AKS.

## <a name="before-you-begin"></a>Antes de empezar

Es preciso que esté instalada y configurada la versión 2.0.56 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="choose-to-update-or-create-a-service-principal"></a>Elija actualizar o crear una entidad de servicio

Cuando quiera actualizar las credenciales de un clúster de AKS, puede hacer lo siguiente:

* actualizar las credenciales de la entidad de servicio existente que usa el clúster, o
* crear a una entidad de servicio y actualizar el clúster para usar estas nuevas credenciales.

Si quiere crear una entidad de servicio y actualizar el clúster de AKS, omita el resto de los pasos descritos en esta sección y vaya a la sección para [crear una entidad de servicio](#create-a-service-principal). Si quiere actualizar las credenciales de la entidad de servicio existente que usa el clúster de AKS, consulte los pasos descritos en esta sección.

### <a name="get-the-service-principal-id"></a>Obtener el id. de la entidad de servicio

Para actualizar las credenciales de la entidad de servicio existente, obtenga el id. de entidad de servicio del clúster mediante el comando [az aks show][az-aks-show]. En el ejemplo siguiente se obtiene el id. del clúster denominado *myAKSCluster* en el grupo de recursos *myResourceGroup*. El id. de entidad de servicio se establece como una variable para usarlo en un comando adicional.

```azurecli-interactive
SP_ID=$(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
```

### <a name="update-the-service-principal-credentials"></a>Actualizar las credenciales de la entidad de servicio

Con un conjunto de variables que contiene el id. de entidad de servicio, debe restablecer las credenciales mediante [az ad sp credential reset][az-ad-sp-credential-reset]. En el ejemplo siguiente se permite a la plataforma Azure generar un nuevo secreto seguro para la entidad de servicio. Este nuevo secreto seguro también se almacena como una variable.

```azurecli-interactive
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

A continuación, vaya a la sección [Actualizar el clúster de AKS con nuevas credenciales](#update-aks-cluster-with-new-credentials).

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Si decidió actualizar las credenciales de la entidad de servicio existente en la sección anterior, omita este paso. Vaya a la sección [Actualizar el clúster de AKS con nuevas credenciales](#update-aks-cluster-with-new-credentials).

Para crear una entidad de servicio y actualizar el clúster de AKS para usar estas nuevas credenciales, use el comando [az ad sp create-for-rbac][az-ad-sp-create]. En el ejemplo siguiente, el parámetro `--skip-assignment` impide la asignación de asignaciones predeterminadas adicionales:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

La salida será similar al del ejemplo siguiente: Tome nota de sus valores `appId` y `password`. Estos valores se usan en el siguiente paso.

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

A continuación, defina las variables del id. de la entidad de servicio y el secreto de cliente; para ello, use el resultado del comando [az ad sp create-for-rbac][az-ad-sp-create], tal como se muestra en el ejemplo siguiente. El valor de *SP_ID* es su *id. de aplicación* y el valor de *SP_SECRET* es su *contraseña*:

```azurecli-interactive
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

## <a name="update-aks-cluster-with-new-credentials"></a>Actualizar el clúster de AKS con nuevas credenciales

Independientemente de si decide actualizar las credenciales de la entidad de servicio existente o de si crea una entidad de servicio, debe actualizar de nuevo el clúster de AKS con sus credenciales mediante el comando [az aks update-credentials][az-aks-update-credentials]. Las variables para los valores de *--service-principal* y *--client-secret* se usan de la siguiente manera:

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

Las credenciales de la entidad de servicio tardan unos minutos en actualizarse en AKS.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, se actualizó la entidad de servicio del propio clúster de AKS. Para obtener más información sobre cómo administrar la identidad de las cargas de trabajo de un clúster, consulte [Best practices for authentication and authorization in AKS][best-practices-identity] (Procedimientos recomendados para la autenticación y autorización en AKS).

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-update-credentials]: /cli/azure/aks#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
