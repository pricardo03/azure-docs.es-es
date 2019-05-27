---
title: Solucionar problemas comunes de Azure Kubernetes Service
description: Obtenga información sobre cómo solucionar problemas y resolver problemas comunes al usar Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: f0b0ff3ff4ac742a7e850798c736eb31098f66e8
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966383"
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

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>No logro obtener los registros mediante kubectl logs ni puedo conectarme al servidor de API. Estoy obteniendo "Error de servidor: back-end de marcado del error: marcar tcp...". ¿Cuál debo hacer?

Asegúrese de que no se modifica el grupo de seguridad de red predeterminado y que el puerto 22 está abierto para la conexión al servidor de API. Compruebe si el `tunnelfront` pod se está ejecutando en el *kube-system* espacio de nombres mediante el `kubectl get pods --namespace kube-system` comando. Si no se está ejecutando, debe forzar la eliminación del pod y se reiniciará.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Estoy intentando actualizar o escalar y recibo el mensaje "message: Changing property 'imageReference' is not allowed" ("mensaje: No se permite cambiar la propiedad 'imageReference'"). ¿Cómo se corrige este problema?

Es posible que reciba este error porque se han modificado las etiquetas de los nodos de agente dentro del clúster de AKS. Modificar y eliminar etiquetas y otras propiedades de recursos en el grupo de recursos MC_ * puede provocar resultados inesperados. La modificación de los recursos en el grupo MC_* en el clúster de AKS interrumpe objetivo de nivel de servicio.

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Recibo errores que el clúster está en estado de error y actualización o escala no funcionará hasta que se ha corregido

*Se dirige esta asistencia para solucionar problemas de https://aka.ms/aks-cluster-failed*

Este error se produce cuando los clústeres de entrar en un estado con errores por varios motivos. Siga los pasos siguientes para resolver el estado del clúster no se pudo antes de reintentar la operación con error anteriormente:

1. Hasta que el clúster fuera de `failed` estado, `upgrade` y `scale` operaciones no se realizará correctamente. Raíz de los problemas comunes y resoluciones son:
    * Escalado con **cuota insuficiente proceso (CRP)**. Para resolver, en primer lugar escalar el clúster a un estado estable objetivo dentro de cuota. A continuación, siga estas [pasos para solicitar una cuota de compute aumento](../azure-supportability/resource-manager-core-quotas-request.md) antes de intentar escalar verticalmente más allá de lo vuelva a unos límites de cuota inicial.
    * Escalado de un clúster con conexiones de red avanzadas y **los recursos de subred insuficiente (redes)**. Para resolver, en primer lugar escalar el clúster a un estado estable objetivo dentro de cuota. A continuación, siga [aumentar estos pasos para solicitar una cuota de recursos](../azure-resource-manager/resource-manager-quota-errors.md#solution) antes de intentar escalar verticalmente más allá de lo vuelva a unos límites de cuota inicial.
2. Cuando se resuelva la causa subyacente de un error de actualización, el clúster debe estar en un estado correcto. Una vez que se comprueba un estado correcto, vuelva a intentar la operación original.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Recibo errores al intentar realizar una actualización o escala que notifican el clúster se está actualmente está actualizado o se ha producido un error de actualización

*Se dirige esta asistencia para solucionar problemas de https://aka.ms/aks-pending-upgrade*

Las operaciones del clúster están limitadas cuando se producen las operaciones de actualización activas o se intentó una actualización, pero posteriormente no se pudo. Para diagnosticar el problema, ejecute `az aks show -g myResourceGroup -n myAKSCluster -o table` para recuperar el estado detallado en el clúster. En función del resultado:

* Si el clúster se está actualizando activamente, espere hasta que finaliza la operación. Si se realiza correctamente, intentar la operación errónea anterior.
* Si el clúster tiene un error de actualización, siga los pasos descritos arriba

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>¿Puedo mover mi clúster a una suscripción diferente o mi suscripción con mi clúster a un nuevo inquilino?

Si el clúster de AKS se ha movido a otra suscripción o el propietario de la suscripción a un nuevo inquilino de clúster, el clúster perderá la funcionalidad debido a perder las asignaciones de roles y derechos de entidades de seguridad de servicio. **AKS no admite clústeres de movimiento entre suscripciones o inquilinos** debido a esta restricción.

## <a name="im-receiving-errors-trying-to-use-features-that-require-virtual-machine-scale-sets"></a>Recibo errores al intentar usar las características que requieren conjuntos de escalado de máquinas virtuales

*Se dirige esta asistencia para solucionar problemas de aka.ms/aks-vmss-habilitación*

Puede recibir errores que indican que el clúster de AKS no está en un conjunto de escalado de máquinas virtuales, como en el ejemplo siguiente:

**AgentPool 'agentpool' ha establecido como habilitado el escalado automático, pero no está en Virtual Machine Scale Sets**

Para usar características como el Escalador automático del clúster o nodo de varios grupos, se deben crear clústeres AKS que usan conjuntos de escalado de máquinas virtuales. Se devuelven errores si intenta utilizar las características que dependen de los conjuntos de escalado de máquinas virtuales y el destino un clúster de AKS del conjunto de escalado normal, el equipo no virtual. Compatibilidad con conjunto de escalado de máquina virtual está actualmente en versión preliminar de AKS.

Siga el *antes de comenzar* pasos en el documento adecuado para registrar correctamente para el escalado de máquinas virtuales del conjunto de característica de vista previa y crear un clúster de AKS:

* [Usar el Escalador automático de clúster](cluster-autoscaler.md)
* [Crear y usar varios grupos de nodos](use-multiple-node-pools.md)
 
## <a name="what-naming-restrictions-are-enforced-for-aks-resources-and-parameters"></a>¿Se aplican las restricciones de nomenclatura para los recursos AKS y los parámetros?

*Se dirige esta asistencia para solucionar problemas de aka.ms/aks--reglas de nomenclatura*

Las restricciones de nomenclatura se implementan mediante la plataforma Windows Azure y el AKS. Si un nombre de recurso o el parámetro interrumpe una de estas restricciones, se devuelve un error que le pide que proporcione una entrada distinta. Se aplican las siguientes directrices de nomenclatura comunes:

* El AKS *MC_* nombre del grupo de recursos combina el nombre del grupo de recursos y el nombre del recurso. La sintaxis generado automáticamente de `MC_resourceGroupName_resourceName_AzureRegion` debe ser no supere los 80 caracteres. Si es necesario, reduzca la longitud del nombre del grupo de recursos o el nombre del clúster AKS.
* El *dnsPrefix* debe empezar y terminar por valores alfanuméricos. Caracteres válidos son valores alfanuméricos y guiones (-). El *dnsPrefix* no puede incluir caracteres especiales, como un punto (.).

## <a name="im-receiving-errors-when-trying-to-create-update-scale-delete-or-upgrade-cluster-that-operation-is-not-allowed-as-another-operation-is-in-progress"></a>Recibo errores al intentar crear, actualizar, escalar, eliminar o actualizar el clúster, la operación no se permite porque otra operación está en curso.

*Se dirige esta asistencia para solucionar problemas de aka.ms/aks-pendientes-operación*

Las operaciones del clúster están limitadas cuando una operación anterior aún está en curso. Para recuperar un estado detallado del clúster, use el `az aks show -g myResourceGroup -n myAKSCluster -o table` comando. Use su propio grupo de recursos y el nombre del clúster de AKS según sea necesario.

En función de la salida del estado del clúster:

* Si el clúster está en cualquier estado de aprovisionamiento distinto *Succeeded* o *Failed*, espere hasta que la operación (*actualizar / actualizar / crear / escalado / eliminar / migración*) finaliza. Cuando haya finalizado la operación anterior, vuelva a intentar la operación de clúster más reciente.

* Si el clúster tiene un error de actualización, siga los pasos descritos [estoy recibiendo errores que el clúster está en estado de error y actualización o escalado no funcionará hasta que se solucione](#im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed).