---
title: Solucionar problemas comunes de Azure Kubernetes Service
description: Obtenga información sobre cómo solucionar problemas y resolver problemas comunes al usar Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 00fadd8a98ec4f58783ed8b407e2621a7c107149
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533522"
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

Si se usa Azure CNI (conexiones de red avanzadas), AKS preasigna las direcciones IP según el criterio de "máximos pods" por nodo configurado. El número de nodos en un clúster de AKS puede ser cualquier valor entre 1 y 110. En función del número máximo de pods configurados por nodo, el tamaño de subred debe ser mayor que el producto del número de nodos y el número máximo de pods por nodo. Esto se describe en la siguiente ecuación básica:

Tamaño de subred > número de nodos del clúster (teniendo en cuenta los requisitos de escalado futuras) * máx. pods por nodo.

Para más información, consulte [Planeamiento de direccionamiento IP del clúster](configure-azure-cni.md#plan-ip-addressing-for-your-cluster).

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

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>No logro obtener los registros mediante kubectl logs ni puedo conectarme al servidor de API. Recibo el mensaje "Error de servidor: error de marcado al back-end: marcar tcp…" ¿Cuál debo hacer?

Asegúrese de que el grupo de seguridad de red predeterminado no se ha modificado y que los puertos 22 y 9000 están abiertos para la conexión al servidor de API. Compruebe si el pod `tunnelfront` se ejecuta en el espacio de nombres *kube-system* con el comando `kubectl get pods --namespace kube-system`. Si no se está ejecutando, debe forzar la eliminación del pod y se reiniciará.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Estoy intentando actualizar o escalar y recibo el mensaje "message: Changing property 'imageReference' is not allowed" ("mensaje: No se permite cambiar la propiedad 'imageReference'"). ¿Cómo se corrige este problema?

Es posible que reciba este error porque se han modificado las etiquetas de los nodos de agente dentro del clúster de AKS. Modificar y eliminar etiquetas y otras propiedades de recursos en el grupo de recursos MC_ * puede provocar resultados inesperados. La modificación de los recursos en el grupo MC_* en el clúster de AKS interrumpe objetivo de nivel de servicio.

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Recibo errores que indican que mi clúster presenta errores y que la actualización o el escalado no funcionarán hasta que se corrija.

*Esta ayuda para solucionar problemas se dirige desde https://aka.ms/aks-cluster-failed*

Este error aparece cuando los clústeres entran en un estado con errores por diversos motivos. Siga los pasos que aparecen a continuación para solucionar el estado con errores del clúster antes de reintentar la operación con errores anterior:

1. Hasta que el clúster deje de tener el estado `failed`, las operaciones `upgrade` y `scale` no se realizarán correctamente. Los problemas y resoluciones raíz comunes incluyen:
    * Escalado con una **cuota de proceso insuficiente (CRP)** . Para solucionarlo, primero debe escalar el clúster de vuelta a un estado objetivo estable dentro de la cuota. Luego, siga estos [pasos para solicitar un aumento de cuota de proceso](../azure-supportability/resource-manager-core-quotas-request.md) antes de intentar volver a escalar verticalmente más allá de los límites de cuota iniciales.
    * Escalar un clúster con redes avanzadas y **recursos de subred (redes) insuficientes**. Para solucionarlo, primero debe escalar el clúster de vuelta a un estado objetivo estable dentro de la cuota. Luego, siga estos [pasos para solicitar un aumento de cuota de recursos](../azure-resource-manager/resource-manager-quota-errors.md#solution) antes de intentar volver a escalar verticalmente más allá de los límites de cuota iniciales.
2. Una vez que se resuelve la causa subyacente de un error de actualización, el clúster debería tener un estado correcto. Una vez que compruebe el estado correcto, vuelva a intentar la operación original.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Recibo errores cuando intento actualizar o escalar que indican que el clúster se está actualizando o que hay un error de actualización.

*Esta ayuda para solucionar problemas se dirige desde https://aka.ms/aks-pending-upgrade*

Las operaciones de actualización y escala en un clúster con un único grupo de nodos o un clúster con [varios grupos de nodos](use-multiple-node-pools.md) se excluyen mutuamente. No puede tener un grupo de clústeres o de nodos actualizados y escalados simultáneamente. En su lugar, cada tipo de operación debe completarse en el recurso de destino antes de la siguiente solicitud en ese mismo recurso. Como resultado, las operaciones se ven limitadas cuando se realizan operaciones de actualización activas o de escala o se intentaron pero se produjeron errores. 

Para ayudar a diagnosticar el problema, ejecute `az aks show -g myResourceGroup -n myAKSCluster -o table` para recuperar el estado detallado del clúster. En función del resultado:

* Si el clúster se está actualizando activamente, espere hasta que finalice la operación. Si se realizó correctamente, vuelva a intentar realizar la operación que presentó errores.
* Si el clúster no se pudo actualizar, siga los pasos descritos en la sección anterior.

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>¿Puedo mover el clúster a otra suscripción o mi suscripción con mi clúster a un inquilino nuevo?

Si movió el clúster de AKS a otra suscripción o la suscripción propietaria del clúster a un inquilino nuevo, el clúster perderá funcionalidad debido a que perderá derechos de entidades de servicio y asignaciones de roles. Debido a esta restricción, **AKS no admite mover clústeres entre suscripciones ni inquilinos**.

## <a name="im-receiving-errors-trying-to-use-features-that-require-virtual-machine-scale-sets"></a>Recibos errores cuando intento usar características que requieren conjuntos de escalado de máquinas virtuales.

*Esta ayuda para solucionar problemas se dirige desde aka.ms/aks-vmss-enablement*

Puede recibir errores que indiquen que el clúster de AKS no se encuentra en un conjunto de escalado de máquinas virtuales, como el ejemplo siguiente:

**El grupo de agentes "agentpool" definió el escalado automático como habilitado, pero no se encuentra en Virtual Machine Scale Sets**

Para usar características como el escalador automático de clústeres o varios grupos de nodos, se deben crear clústeres de AKS que usen conjuntos de escalado de máquinas virtuales. Se devuelven errores si intenta usar características que dependen de conjuntos de escalado de máquinas virtuales y el usuario se dirige a un clúster de AKS no en un conjunto de escalado de máquinas virtuales normal. La compatibilidad del conjunto de escalado de máquinas virtuales actualmente está en versión preliminar en AKS.

Siga los pasos de *Antes de empezar* que aparecen en el documento adecuado para registrarse correctamente para obtener la versión preliminar de la característica de conjunto de escalado de máquinas virtuales y crear un clúster de AKS:

* [Uso del escalador automático del clúster](cluster-autoscaler.md).
* [Creación y uso de varios grupos de nodos](use-multiple-node-pools.md).
 
## <a name="what-naming-restrictions-are-enforced-for-aks-resources-and-parameters"></a>¿Qué restricciones de nomenclatura se aplican para los parámetros y recursos de AKS?

*Esta ayuda para solucionar problemas se dirige desde aka.ms/aks-naming-rules*

Tanto AKS como la plataforma de Azure implementan las restricciones de nomenclatura. Si un parámetro o nombre de recurso infringe una de estas restricciones, se devuelve un error en el que se le pide brindar otra entrada. Se aplican estas directrices de nomenclatura comunes:

* El nombre del grupo de recursos *MC_* de AKS combina el nombre del grupo de recursos y el nombre del recurso. La sintaxis generada automáticamente de `MC_resourceGroupName_resourceName_AzureRegion` no puede tener más de 80 caracteres. Si es necesario, disminuya la longitud del nombre del grupo de recursos o del nombre del clúster de AKS.
* *dnsPrefix* debe empezar y terminar con valores alfanuméricos. Los caracteres válidos incluyen valores alfanuméricos y guiones (-). *dnsPrefix* no puede incluir caracteres especiales, como un punto (.).

## <a name="im-receiving-errors-when-trying-to-create-update-scale-delete-or-upgrade-cluster-that-operation-is-not-allowed-as-another-operation-is-in-progress"></a>Recibo errores cuando intento crear, actualizar, escalar, eliminar o actualizar un clúster, donde se indica que no se permite la operación porque hay otra operación en curso.

*Esta ayuda para solucionar problemas se dirige desde aka.ms/aks-pending-operation*

Las operaciones del clúster se limitan cuando todavía hay en curso una operación anterior. Para recuperar un estado detallado del clúster, use el comando `az aks show -g myResourceGroup -n myAKSCluster -o table`. Use el nombre de su propio clúster de AKS y de su propio grupo de recursos, en caso de ser necesario.

En función de la salida del estado del clúster:

* Si el clúster está en cualquier estado de aprovisionamiento distinto de *Succeeded* (Correcto) o *Failed* (Con errores), espere que se termine la operación (*Actualización/Creación/Escalado/Eliminación/Migración*). Cuando finalice la operación anterior, vuelva a intentar la operación de clúster más reciente.

* Si el clúster presenta errores de actualización, siga los pasos que se describen en [Recibo errores que indican que mi clúster presenta errores y que la actualización o el escalado no funcionarán hasta que se corrija](#im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed).
