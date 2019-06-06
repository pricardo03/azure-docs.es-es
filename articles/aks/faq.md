---
title: Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)
description: Encuentre respuestas a algunas de las preguntas comunes sobre Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/03/2019
ms.author: iainfou
ms.openlocfilehash: 1cc03cbcffc5253e8b357b6702cd21c45740ff81
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514499"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-currently-provide-aks"></a>¿Qué regiones de Azure proporcionan actualmente AKS?

Para obtener una lista completa de las regiones disponibles, consulte [AKS regiones y disponibilidad][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>¿AKS admite el escalado automático de los nodos?

Sí, el escalado automático está disponible a través de la [Escalador automático Kubernetes] [ auto-scaler] a partir de Kubernetes 1.10. Para obtener información sobre cómo configurar y usar el Escalador automático de clúster manualmente, consulte [escalado automático de clúster de AKS][aks-cluster-autoscale].

También puede usar el Escalador automático integradas de clúster (actualmente en versión preliminar de AKS) para administrar el escalado de nodos. Para obtener más información, consulte [escalar automáticamente un clúster para satisfacer las necesidades de la aplicación en AKS][aks-cluster-autoscaler].

## <a name="does-aks-support-kubernetes-rbac"></a>¿Admite AKS de Kubernetes RBAC?

Sí, Kubernetes control de acceso basado en roles (RBAC) está habilitada de forma predeterminada cuando se crean clústeres con la CLI de Azure. Puede habilitar RBAC para los clústeres que se crearon mediante el portal de Azure o plantillas.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>¿Puedo implementar AKS en mi red virtual existente?

Sí, puede implementar un clúster de AKS en una red virtual existente mediante el [característica de red avanzada][aks-advanced-networking].

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>¿Puedo hacer que el servidor de API de Kubernetes sea accesible únicamente en mi red virtual?

De momento, no. El servidor de API de Kubernetes se expone como un nombre de dominio completo público (FQDN). Puede controlar el acceso al clúster mediante el uso de [Kubernetes RBAC y Azure Active Directory (Azure AD)][aks-rbac-aad].

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>¿Se aplican las actualizaciones de seguridad a los nodos de agente de AKS?

Azure aplica automáticamente revisiones de seguridad en los nodos de Linux del clúster según una programación nocturna. Sin embargo, es responsable de garantizar que los nodos se reinician como de Linux necesarios. Tiene varias opciones para reiniciar los nodos:

- Manualmente, mediante Azure Portal o la CLI de Azure.
- Mediante la actualización del clúster de AKS. Las actualizaciones de clúster [acordonar y vaciar los nodos] [ cordon-drain] automáticamente y, a continuación, colocar un nodo nuevo en línea con la imagen de Ubuntu más reciente y una nueva versión de revisión o una versión secundaria de Kubernetes. Para más información, consulte [Actualización de un clúster de AKS][aks-upgrade].
- Mediante el uso de [Kured](https://github.com/weaveworks/kured), un demonio de reinicio de código abierto para Kubernetes. Kured se ejecuta como un [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) y supervisa cada nodo para detectar la presencia de un archivo que indica que se requiere un reinicio. En el clúster, los reinicios del sistema operativo administrados por el mismo [acordonar y vaciar el proceso] [ cordon-drain] como una actualización de clúster.

Para obtener más información sobre el uso de Kured, consulte [Apply security and kernel updates to nodes in AKS][node-updates-kured] (Aplicación de actualizaciones de kernel y de seguridad en los nodos en AKS).

### <a name="windows-server-nodes"></a>Nodos de Windows Server

Para los nodos de Windows Server (actualmente en versión preliminar de AKS), Windows Update ejecute automáticamente y se aplican las actualizaciones más recientes. Según una programación regular en torno a su propio proceso de validación y el ciclo de lanzamiento de Windows Update, debe realizar una actualización en los grupos de nodos de Windows Server en el clúster de AKS. Este proceso de actualización crea nodos que ejecutan la última imagen de Windows Server y las revisiones, y quita los nodos anteriores. Para obtener más información sobre este proceso, consulte [actualizar un grupo de nodos de AKS][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>¿Por qué se crean dos grupos de recursos con AKS?

Cada implementación de AKS abarca dos grupos de recursos:

1. Crear el primer grupo de recursos. Este grupo contiene sólo el recurso del servicio de Kubernetes. El proveedor de recursos AKS crea automáticamente el segundo grupo de recursos durante la implementación. Un ejemplo del segundo grupo de recursos es *MC_myResourceGroup_myAKSCluster_eastus*. Para obtener información sobre cómo especificar el nombre de este segundo grupo de recursos, consulte la sección siguiente.
1. El segundo grupo de recursos, tales como *MC_myResourceGroup_myAKSCluster_eastus*, contiene todos los recursos de infraestructura asociados con el clúster. Estos recursos incluyen las máquinas virtuales de nodos de Kubernetes, las redes virtuales y el almacenamiento. El propósito de este grupo de recursos es simplificar la limpieza de recursos.

Si crea recursos que desee usar con el clúster AKS, como las cuentas de almacenamiento o direcciones IP públicas y reservadas, colóquelos en el grupo de recursos generado automáticamente.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>¿Proporcionar mi propio nombre para el grupo de recursos de infraestructura AKS?

Sí. De forma predeterminada, el proveedor de recursos AKS crea automáticamente un grupo de recursos secundarios (como *MC_myResourceGroup_myAKSCluster_eastus*) durante la implementación. Para cumplir con la directiva corporativa, puede proporcionar su propio nombre para este clúster administrado (*MC_* ) grupo de recursos.

Para especificar su propio nombre de grupo de recursos, instale el [-versión preliminar de aks] [ aks-preview-cli] versión de la extensión de CLI de Azure *0.3.2* o una versión posterior. Cuando crea un clúster de AKS mediante la [crear az aks] [ az-aks-create] de comandos, use el *--nodo grupo de recursos* parámetro y especifique un nombre para el grupo de recursos. Si se [usar una plantilla de Azure Resource Manager] [ aks-rm-template] para implementar un clúster de AKS, puede definir el nombre del grupo de recursos mediante el *nodeResourceGroup* propiedad.

* El grupo de recursos secundario se crea automáticamente el proveedor de recursos de Azure en su propia suscripción.
* Puede especificar un nombre de grupo de recursos personalizados sólo cuando se crea el clúster.

Al trabajar con el *MC_* grupo de recursos, tenga en cuenta que no es posible:

* Especifique un grupo de recursos existente para el *MC_* grupo.
* Especifique una suscripción diferente para el *MC_* grupo de recursos.
* Cambiar el *MC_* nombre de grupo de recursos una vez creado el clúster.
* Especificar nombres para los recursos administrados dentro de la *MC_* grupo de recursos.
* Modificar o eliminar etiquetas de recursos administrados dentro de la *MC_* grupo de recursos. (Vea la información adicional en la sección siguiente).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>¿Puedo modificar etiquetas y otras propiedades de los recursos AKS en el grupo de recursos MC_?

Si modifica o eliminar etiquetas creadas por Azure y otras propiedades de recursos en el *MC_* grupo de recursos, podría obtener resultados inesperados, como el escalado y actualizar errores. AKS le permite crear y modificar etiquetas personalizadas. Es posible que desee crear o modificar etiquetas personalizadas, por ejemplo, para asignar un centro de costo o la unidad de negocio. Mediante la modificación de los recursos en el *MC_* en el clúster de AKS, interrumpe el objetivo de nivel de servicio (SLO). ¿Para obtener más información, consulte [AKS Does ofrecen un acuerdo de nivel de servicio?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>¿Qué controladores de admisión de Kubernetes admite AKS? ¿Se pueden agregar o eliminar los controladores de admisión?

AKS admite los siguientes [controladores de admisión][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

Actualmente, no se puede modificar la lista de controladores de admisión en AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>¿Azure Key Vault se integra con AKS?

AKS actualmente de forma nativa no se integra con Azure Key Vault. Sin embargo, el [Azure Key Vault FlexVolume para Kubernetes proyecto] [ keyvault-flexvolume] habilita la integración de pods de Kubernetes para los secretos de Key Vault directa.

## <a name="can-i-run-windows-server-containers-on-aks"></a>¿Puedo ejecutar contenedores de Windows Server en AKS?

Sí, los contenedores de Windows Server están disponibles en versión preliminar. Para ejecutar contenedores de Windows Server en AKS, cree un grupo de nodos que se ejecuta Windows Server como el sistema operativo invitado. Contenedores de Windows Server pueden usar solo Windows Server 2019. Para empezar, vea [crear un clúster de AKS con un grupo de nodos de Windows Server][aks-windows-cli].

Compatibilidad de servidor de ventana para el grupo de nodos incluye algunas limitaciones que forman parte de Windows Server nivel superior en el proyecto de Kubernetes. Para obtener más información sobre estas limitaciones, consulte [contenedores de Windows Server en las limitaciones de AKS][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>¿AKS ofrece un acuerdo de nivel de servicio?

En un contrato de nivel de servicio (SLA), el proveedor acuerda reembolsar al cliente por el costo del servicio si no se cumple el nivel de servicio publicado. Puesto AKS es gratuita, costo no está disponible para reembolsar, por lo que AKS no tiene ningún SLA formal. Sin embargo, AKS pretende mantener la disponibilidad de al menos del 99,5% para el servidor de API de Kubernetes.

## <a name="why-cant-i-set-maxpods-below-30"></a>¿Por qué no puedo establecer maxPods por debajo de 30?

En AKS, puede establecer el `maxPods` valor al crear el clúster mediante el uso de las plantillas de CLI de Azure y Azure Resource Manager. Sin embargo, Kubenet y Azure CNI requieren un *valor mínimo* (valida en tiempo de creación):

| Redes | Mínima | Máxima |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

Dado que AKS es un servicio administrado, se implemente y administración los complementos y los pods como parte del clúster. En el pasado, los usuarios se podrían definir un `maxPods` valor menor que el valor que los pods administrados deben para ejecutar (por ejemplo, 30). AKS ahora calcula el número mínimo de pods mediante el uso de esta fórmula: ((maxPods o (maxPods * vm_count)) > mínimo de pods de complemento administrado.

Los usuarios no pueden invalidar el mínimo `maxPods` validación.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>¿Puedo aplicar descuentos de reserva de Azure en mi nodos de agente AKS?

Los nodos de agente AKS se facturan como máquinas virtuales de Azure estándares, si ha comprado [reservas Azure] [ reservation-discounts] para el tamaño de máquina virtual que está usando en AKS, los descuentos se aplican automáticamente.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
