---
title: Solucionar problemas comunes de Azure Kubernetes Service
description: Obtenga información sobre cómo solucionar problemas y resolver problemas comunes al usar Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: fd3d1c464c6f2d4cbecd715db0689581ca141769
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654077"
---
# <a name="aks-troubleshooting"></a>Solución de problemas de AKS

Al crear o administrar clústeres de Azure Kubernetes Service (AKS), en ocasiones pueden surgir problemas. En este artículo se detallan algunos problemas comunes y los pasos para solucionarlos.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>En general, ¿dónde puedo encontrar información sobre la depuración de problemas de Kubernetes?

Pruebe la [guía oficial para la solución de problemas de clústeres de Kubernetes](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
Además, existe una [guía para la solución de problemas](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md) publicada por un ingeniero de Microsoft en torno a la solución de problemas de pods, nodos, clústeres y otras características.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Obtengo un error de "cuota excedida" durante la creación o actualización. ¿Cuál debo hacer? 

Deberá [solicitar núcleos](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>¿Cuál es la configuración máxima de pods por nodo de AKS?

La configuración máxima de pods por nodo es 30 de forma predeterminada si implementa un clúster de AKS en Azure Portal.
La configuración máxima de pods por nodo es 110 de forma predeterminada si implementa un clúster de AKS en la CLI de Azure. (Asegúrese de usar la versión más reciente de la CLI de Azure). Se puede cambiar esta configuración predeterminada mediante la marca `–-max-pods` en el comando `az aks create`.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Recibo el error insufficientSubnetSize al implementar un clúster de AKS con redes avanzadas. ¿Cuál debo hacer?

En la opción personalizada de Azure Virtual Network para redes durante la creación de AKS, se usa la interfaz de red de contenedor (CNI) de Azure para la administración de direcciones IP (IPAM). El número de nodos en un clúster de AKS puede ser cualquier valor entre 1 y 100. En función de la sección anterior, el tamaño de subred debe ser mayor que el producto del número de nodos y el máximo de pods por nodo. Se puede expresar la relación de esta manera: tamaño de subred > número de nodos del clúster * máximo de pods por nodo.

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mi pod se ha atascado en el modo CrashLoopBackOff. ¿Cuál debo hacer?

Puede haber varios motivos para que el pod se quede atascado en ese modo. Algunas soluciones posibles son:

* El propio pod, mediante el comando `kubectl describe pod <pod-name>`.
* Los registros, mediante el uso de `kubectl log <pod-name>`.

Para obtener más información sobre cómo solucionar problemas de los pods, consulte cómo [depurar aplicaciones](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Estoy intentando habilitar RBAC en un clúster existente. ¿Cómo puedo hacerlo?

Desafortunadamente, la habilitación del control de acceso basado en rol (RBAC) en los clústeres existentes no se admite en este momento. Debe crear nuevos clústeres de forma explícita. Si usa la CLI, RBAC está habilitado de forma predeterminada. Si usa el portal de AKS, hay disponible un botón de alternancia para habilitar RBAC en el flujo de trabajo de creación.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>He creado un clúster con RBAC habilitado mediante la CLI de Azure con los valores predeterminados o Azure Portal y ahora aparecen numerosas advertencias en el panel de Kubernetes. El panel solía funcionar sin mostrar ninguna advertencia. ¿Cuál debo hacer?

El motivo por el que recibe advertencias en el panel es que ahora el clúster tiene RBAC habilitado y se ha deshabilitado su acceso de forma predeterminada. En general, este enfoque es recomendable, puesto que la exposición predeterminada del panel a todos los usuarios del clúster puede dar lugar a amenazas de seguridad. Si desea habilitar el panel, siga los pasos descritos en esta [entrada de blog](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>No puedo conectarme al panel. ¿Cuál debo hacer?

La manera más fácil de tener acceso al servicio fuera del clúster es ejecutar `kubectl proxy`, que redirigirá mediante proxy las solicitudes para el puerto 8001 de localhost al servidor de API de Kubernetes. Desde allí, el servidor de API puede redirigir mediante proxy a su servicio: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Si no ve el panel de Kubernetes, compruebe que el pod `kube-proxy` se está ejecutando en el espacio de nombres `kube-system`. Si no está en estado de ejecución, elimine el pod y se reiniciará.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>No logro obtener los registros mediante kubectl logs ni puedo conectarme al servidor de API. Recibo el mensaje "Error de servidor: error de marcado al back-end: marcar tcp..." ¿Cuál debo hacer?

Asegúrese de que el grupo de seguridad de red (NSG) predeterminado no se ha modificado y que el puerto 22 está abierto para la conexión al servidor de API. Compruebe si el pod `tunnelfront` se está ejecutando en el espacio de nombres `kube-system`. Si no se está ejecutando, debe forzar la eliminación del pod y se reiniciará.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-problem"></a>Estoy intentando actualizar o escalar y recibo el mensaje "message: Changing property 'imageReference' is not allowed" ("mensaje: No se permite cambiar la propiedad 'imageReference'").  ¿Cómo se corrige este problema?

Es posible que reciba este error porque se han modificado las etiquetas de los nodos de agente dentro del clúster de AKS. Modificar y eliminar etiquetas y otras propiedades de recursos en el grupo de recursos MC_ * puede provocar resultados inesperados. La modificación de los recursos en el grupo MC_* en el clúster de AKS interrumpe objetivo de nivel de servicio.

## <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>¿Cómo se puede renovar el secreto de la entidad de servicio en mi clúster de AKS?

De forma predeterminada, los clústeres de AKS se crean con una entidad de servicio que tiene un período de expiración de un año. A medida que se aproxima a la fecha de expiración, puede restablecer las credenciales para ampliar la entidad de servicio durante un período de tiempo adicional.

En el siguiente ejemplo se realizan los pasos necesarios:

1. Se obtiene el identificador de la entidad de servicio del clúster mediante el comando [az aks show](/cli/azure/aks#az-aks-show).
1. Se enumera el secreto de cliente de la entidad de servicio mediante el comando [az ad sp credential list](/cli/azure/ad/sp/credential#az-ad-sp-credential-list).
1. Se extiende la entidad de servicio durante un año más mediante el comando[az ad sp credential-reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset). El secreto de cliente de la entidad de servicio debe permanecer igual para que el clúster de AKS se ejecute correctamente.

```azurecli
# Get the service principal ID of your AKS cluster.
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret.
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for one year.
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```
