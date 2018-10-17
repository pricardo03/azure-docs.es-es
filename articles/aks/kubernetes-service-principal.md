---
title: Entidades de servicio para Azure Kubernetes Service (AKS)
description: Cree y administre una entidad de servicio de Azure Active Directory para un clúster en Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: ef3139c4b3f06644b219e177fad0c094ed600fb6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394597"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Entidades de servicio con Azure Kubernetes Service (AKS)

Para interactuar con las API de Azure, un clúster de AKS requiere una [entidad de servicio de Azure Active Directory][aad-service-principal]. La entidad de servicio se necesita para crear y administrar dinámicamente otros recursos de Azure, como Azure Load Balancer o Container Registry.

En este artículo se muestra cómo crear y usar una entidad de servicio para los clústeres de AKS.

## <a name="before-you-begin"></a>Antes de empezar

Para crear una entidad de servicio de Azure AD, es preciso tener los permisos suficientes para registrar una aplicación en un inquilino de Azure AD y asignarle un rol en una suscripción. Si no tiene los permisos necesarios, es posible que tenga que pedir al administrador de Azure AD o de la suscripción que asigne los permisos necesarios o crear previamente una entidad de servicio para su uso con el clúster de AKS.

También es preciso que esté instalada y configurada la versión 2.0.46 de la CLI de Azure u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Creación y uso de una entidad de servicio automáticamente

Cuando crea un clúster de AKS en Azure Portal o mediante el comando [az aks create][az-aks-create], Azure puede generar automáticamente una entidad de servicio.

En el siguiente ejemplo de la CLI de Azure, no se especifica una entidad de servicio. En este escenario, la CLI de Azure crea un entidad de servicio para el clúster de AKS. Para completar esta operación correctamente, la cuenta debe tener los derechos apropiados para crear una entidad de servicio.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>Creación manual de una entidad de servicio

Para crear manualmente una entidad de servicio con la CLI de Azure, utilice el comando [az ad sp create-for-rbac][az-ad-sp-create]. En el ejemplo siguiente, el parámetro `--skip-assignment` impide la asignación de asignaciones predeterminadas adicionales:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

La salida será similar al del ejemplo siguiente: Tome nota de sus valores `appId` y `password`. Se usan al crear un clúster de AKS en la sección siguiente.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Especificación de una entidad de servicio para un clúster de AKS

Para usar una entidad de servicio al crear un clúster de AKS mediante el comando [az aks create][az-aks-create], use los parámetros `--service-principal` y `--client-secret` para especificar los valores `appId` y `password` a partir del resultado del comando [az ad sp create-for-rbac][az-ad-sp-create]:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Si implementa un clúster de AKS mediante Azure Portal, en la página *autenticación* del cuadro de diálogo **Creación de un clúster de Kubernetes**, elija **Configurar la entidad de servicio**. Seleccione **Usar existente** y especifique los valores siguientes:

- **Id. de cliente de la entidad de servicio** es su *appId*
- **Secreto del cliente de la entidad de servicio** es el valor de *contraseña*

![Imagen de la exploración hasta Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="additional-considerations"></a>Consideraciones adicionales

Cuando use entidades de servicio de AKS y Azure AD, tenga en cuenta lo siguiente.

- La entidad de servicio para Kubernetes forma parte de la configuración del clúster. Sin embargo, no use la identidad para implementar el clúster.
- Cada entidad de servicio está asociada a una aplicación de Azure AD. La entidad de servicio de un clúster de Kubernetes puede asociarse con cualquier nombre de aplicación de Azure AD válido (por ejemplo, *https://www.contoso.org/example*). La dirección URL de la aplicación no tiene por qué ser un punto de conexión real.
- Al especificar el **identificador de cliente**, utilice el valor de `appId`.
- En las máquinas virtuales principal y nodo del clúster de Kubernetes, las credenciales de la entidad de servicio se almacenan en el archivo `/etc/kubernetes/azure.json`.
- Cuando use el comando [az aks create][az-aks-create] para generar la entidad de servicio automáticamente, sus credenciales se escriben en el archivo `~/.azure/aksServicePrincipal.json` de la máquina que se usa para ejecutar el comando.
- Al eliminar un clúster de AKS creado mediante [az aks create][az-aks-create], no se elimina la entidad de servicio que se creó automáticamente.
    - Para eliminar la entidad de servicio, primero hay que obtener el identificador para el servicio principal con [az ad app list][az-ad-app-list]. En el ejemplo siguiente se consulta el clúster denominado *myAKSCluster* y, a continuación, se elimina el identificador de aplicación con [az ad app delete][az-ad-app-delete]. Reemplace estos nombres con sus propios valores:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de las entidades de servicio de Azure Active Directory, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory][service-principal].

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
