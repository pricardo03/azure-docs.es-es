---
title: Solucionar problemas comunes de Azure Kubernetes Service
description: Obtenga información sobre cómo solucionar problemas y resolver problemas comunes al usar Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: c20f2cc03565ce861dfc6317be8459fdafeef0bf
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384112"
---
# <a name="aks-troubleshooting"></a>Solución de problemas de AKS
En ocasiones, se pueden producir problemas al crear o administrar clústeres de AKS. En este artículo se detallan algunos problemas comunes y los pasos para solucionarlos.

### <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-issues"></a>En general, ¿dónde puedo encontrar información sobre la depuración de problemas de Kubernetes?

[Este](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/) es un vínculo oficial para la solución de problemas de clústeres de Kubernetes.
[Este](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md) es un vínculo para una guía de solución de problemas publicada por un ingeniero de Microsoft en torno a la solución de problemas de pods, nodos, clústeres, etcétera.

### <a name="i-am-getting-a-quota-exceeded-error-during-create-or-upgrade-what-should-i-do"></a>Obtengo un error de cuota excedida durante la creación o actualización. ¿Cuál debo hacer? 

Deberá solicitar núcleos [aquí](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

### <a name="what-is-the-max-pods-per-node-setting-for-aks"></a>¿Cuál es la configuración máxima de pods por nodo de AKS?

El número máximo de pods por nodo se establece en 30 de forma predeterminada si implementa un clúster de AKS en Azure Portal.
El número máximo de pods por nodo se establece en 110 de forma predeterminada si implementa un clúster de AKS en la CLI de Azure. (Asegúrese de usar la versión más reciente de la CLI de Azure). Se puede cambiar esta configuración predeterminada con la marca –max-nodes-per-pod en el comando az aks create.

### <a name="i-am-getting-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Recibo el error "insufficientSubnetSize" al implementar un clúster de AKS con redes avanzadas. ¿Cuál debo hacer?

En la opción seleccionada de VNET personalizada para redes durante las creaciones de AKS, se usa CNI de Azure para IPAM. El número de nodos en un clúster de AKS puede ser cualquier valor entre 1 y 100. En función del 2) anterior, el tamaño de la subred debe ser mayor que el producto del número de nodos y el tamaño de la subred del número máximo de pods por nodo > núm. de nodos del clúster * número máximo de pods por nodo.

### <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mi pod se ha atascado en el modo "CrashLoopBackOff". ¿Cuál debo hacer?

Puede haber varios motivos para que el pod se quede atascado en ese modo. Puede buscarlos en 
* El propio pod usando el comando `kubectl describe pod <pod-name>`
* Los registros usando el comando `kubectl log <pod-name>`

### <a name="i-am-trying-to-enable-rbac-on-an-existing-cluster-can-you-tell-me-how-i-can-do-that"></a>Estoy intentando habilitar RBAC en un clúster existente. ¿Podría saber cómo puedo hacerlo?

Desafortunadamente la habilitación de RBAC en los clústeres existentes no se admite en este momento. Deberá crear nuevos clústeres de forma explícita. Si usa la CLI, RBAC está habilitado de forma predeterminada, aunque está disponible un botón de alternancia para habilitarlo en el flujo de trabajo de creación del portal de AKS.

### <a name="i-created-a-cluster-using-the-azure-cli-with-defaults-or-the-azure-portal-with-rbac-enabled-and-numerous-warnings-in-the-kubernetes-dashboard-the-dashboard-used-to-work-before-without-any-warnings-what-should-i-do"></a>He creado un clúster mediante la CLI de Azure con los valores predeterminados o Azure Portal con RBAC habilitado y aparecen numerosas advertencias en el panel de Kubernetes. Antes, el panel solía funcionar sin mostrar ninguna advertencia. ¿Cuál debo hacer?

El motivo de recibir advertencias en el panel es que ahora está habilitado con RBAC y se ha deshabilitado su acceso de forma predeterminada. En general, este enfoque se considera recomendable, puesto que la exposición predeterminada del panel a todos los usuarios del clúster puede dar lugar a amenazas de seguridad. Si desea habilitar el panel, siga este [blog](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/) para habilitarlo.

### <a name="i-cant-seem-to-connect-to-the-dashboard-what-should-i-do"></a>No logro conectarme al panel. ¿Cuál debo hacer?

La manera más fácil tener acceso al servicio fuera del clúster es ejecutar el proxy kubectl, que enviará mediante proxy las solicitudes para el puerto 8001 de localhost al servidor de API de Kubernetes. Desde allí, el servidor de API puede enviarlas mediante proxy a su servicio: http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default

Si no ve el panel de Kubernetes, compruebe si el pod kube-proxy está en ejecución en el espacio de nombres kube-system. Si no está en estado de ejecución, elimine el pod y se reiniciará.

### <a name="i-could-not-get-logs-using-kubectl-logs-or-cannot-connect-to-the-api-server-getting-the-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>No puedo obtener registros con los registros de Kubectl o no puedo conectarme al servidor de API y recibo "Error from server: error dialing backend: dial tcp…" (Error del servidor: error al marcar el back-end: marcar TCP...). ¿Cuál debo hacer?

Asegúrese de que el NSG predeterminado no se ha modificado y que el puerto 22 está abierto para la conexión al servidor de API. Compruebe si el pod tunnelfront está en ejecución en el espacio de nombres kube-system. Si no es así, fuerce su eliminación y se reiniciará.

### <a name="i-am-trying-to-upgrade-or-scale-and-am-getting-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-issue"></a>Estoy intentando actualizar o escalar y recibo el "mensaje": "Changing property 'imageReference' is not allowed" ("No se permite cambiar la propiedad 'imageReference'"). Error.  ¿Cómo se corrige este problema?

Es posible que se produzca este error porque se han modificado las etiquetas de los nodos de agente dentro del clúster de AKS. Modificar y eliminar etiquetas y otras propiedades de recursos en el grupo de recursos MC_ * puede provocar resultados inesperados. La modificación de los recursos de MC_ * en el clúster de AKS interrumpe el SLO.

### <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>¿Cómo se puede renovar el secreto de la entidad de servicio en mi clúster de AKS?

De forma predeterminada, los clústeres de AKS se crean con una entidad de servicio que tiene un período de expiración de un año. A medida que se aproxima a la fecha de expiración de un año, puede restablecer las credenciales para ampliar la entidad de servicio durante un período de tiempo adicional.

En el siguiente ejemplo se realizan los siguientes pasos:

1. Obtiene el identificador de la entidad de servicio del clúster mediante el comando [az aks show](/cli/azure/aks#az-aks-show).
1. Enumera el secreto de cliente de la entidad de servicio mediante el comando [az ad sp credential list](/cli/azure/ad/sp/credential#az-ad-sp-credential-list)
1. Extiende la entidad de servicio durante un año más mediante el comando[az ad sp credential-reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset). El secreto de cliente de la entidad de servicio debe permanecer igual para que el clúster de AKS se ejecute correctamente.

```azurecli
# Get the service principal ID of your AKS cluster
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for 1 year
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```
