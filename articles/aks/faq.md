---
title: Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)
description: Encuentre respuestas a algunas de las preguntas comunes sobre Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/03/2019
ms.author: iainfou
ms.openlocfilehash: d4fa365e1ed055fa8ddeb8fd475e152af84a3b71
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560446"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-currently-provide-aks"></a>¿Qué regiones de Azure proporcionan actualmente AKS?

Para obtener una lista completa de las regiones disponibles, consulte [AKS regions and availability][aks-regions] (Regiones y disponibilidad de AKS).

## <a name="does-aks-support-node-autoscaling"></a>¿AKS admite el escalado automático de los nodos?

Sí, la capacidad para escalar automáticamente los nodos de agente horizontalmente en AKS está disponible actualmente en versión preliminar. Consulte el artículo [Escalado automático de un clúster para satisfacer las necesidades de aplicación en AKS][aks-cluster-autoscaler] for instructions. AKS autoscaling is based on the [Kubernetes autoscaler][auto-scaler].

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>¿Puedo implementar AKS en mi red virtual existente?

Sí, puede implementar un clúster de AKS en una red virtual existente mediante la [característica de redes avanzadas][aks-advanced-networking].

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>¿Puedo limitar quién tiene acceso al servidor de API de Kubernetes?

Sí, puede limitar el acceso al servidor de API de Kubernetes mediante los [intervalos de IP autorizados del servidor de API][api-server-authorized-ip-ranges], que actualmente se encuentran en versión preliminar.

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>¿Puedo restringir el servidor de API de Kubernetes para que solo sea accesible en mi red virtual?

En este momento no es posible, pero está planeado. Puede realizar un seguimiento del progreso en el [repositorio de GitHub de AKS][private-clusters-github-issue].

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>¿Puedo tener diferentes tamaños de máquina virtual en un único clúster?

Sí, puede usar diferentes tamaños de máquinas virtuales en el clúster de AKS mediante la creación de [varios grupos de nodos][multi-node-pools], cuya función actualmente se encuentra en versión preliminar.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>¿Se aplican las actualizaciones de seguridad a los nodos de agente de AKS?

Azure aplica automáticamente revisiones de seguridad a los nodos de Linux del clúster siguiendo una programación nocturna. Sin embargo, es responsabilidad suya asegurarse de que los nodos de Linux se reinician según sea necesario. Cuenta con varias opciones para reiniciar los nodos:

- Manualmente, mediante Azure Portal o la CLI de Azure.
- Mediante la actualización del clúster de AKS. El clúster actualiza los [nodos de acordonamiento y purga][cordon-drain] automatically and then bring a new node online with the latest Ubuntu image and a new patch version or a minor Kubernetes version. For more information, see [Upgrade an AKS cluster][aks-upgrade].
- Mediante [Kured](https://github.com/weaveworks/kured), un demonio de reinicio de código abierto para Kubernetes. Kured se ejecuta como un elemento [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) y supervisa en todos los nodos la presencia de un archivo que indique que hace falta un reinicio. En el clúster, los reinicios del sistema operativo se administran con el mismo [proceso de acordonar y purgar][cordon-drain] que una actualización de clúster.

Para obtener más información sobre el uso de Kured, consulte [Apply security and kernel updates to nodes in AKS][node-updates-kured] (Aplicación de actualizaciones de kernel y de seguridad en los nodos de AKS).

### <a name="windows-server-nodes"></a>Nodos de Windows Server

Para los nodos de Windows Server (actualmente en versión preliminar en AKS), Windows Update no ejecuta ni aplica las actualizaciones más recientes de manera automática. En una programación normal del ciclo de versiones de Windows Update y su proceso de validación propio, debe realizar una actualización de los grupos de nodos de Windows Server en el clúster de AKS. Este proceso de actualización crea nodos que ejecutan la imagen y las revisiones más recientes de Windows Server y elimina los nodos anteriores. Para obtener más información sobre este proceso, consulte [Actualización de un grupo de nodos en AKS][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>¿Por qué se crean dos grupos de recursos con AKS?

Cada implementación de AKS abarca dos grupos de recursos:

1. Debe cree el primer grupo de recursos. Este grupo solo contiene el recurso del servicio de Kubernetes. El proveedor de recursos de AKS crea automáticamente el segundo grupo de recursos durante la implementación. Un ejemplo del segundo grupo de recursos es *MC_myResourceGroup_myAKSCluster_eastus*. Para obtener información sobre cómo especificar el nombre de este segundo grupo de recursos, consulte la sección siguiente.
1. El segundo grupo de recursos, como *MC_myResourceGroup_myAKSCluster_eastus*, contiene todos los recursos de infraestructura asociados con el clúster. Estos recursos incluyen las máquinas virtuales de nodos de Kubernetes, las redes virtuales y el almacenamiento. La finalidad de este grupo de recursos es de simplificar la limpieza de recursos.

Si crea recursos para usarlos con el clúster de AKS, como cuentas de almacenamiento o direcciones IP públicas reservadas, colóquelos en el grupo de recursos generado automáticamente.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>¿Puedo proporcionar mi propio nombre para el grupo de recursos de infraestructura de AKS?

Sí. De manera predeterminada, el proveedor de recursos de AKS crea automáticamente un segundo grupo de recursos (como *MC_myResourceGroup_myAKSCluster_eastus*) durante la implementación. Para cumplir con la directiva corporativa, puede proporcionar su propio nombre para este grupo de recursos del clúster administrado (*MC_* ).

Para especificar un nombre de su elección para el grupo de recursos, instale la versión de la extensión de la CLI de Azure [aks-preview][aks-preview-cli] *0.3.2* o una posterior. Cuando cree un clúster de AKS mediante el comando [az aks create][az-aks-create], use el parámetro *--node-resource-group* y especifique un nombre para el grupo de recursos. Si [usa una plantilla de Azure Resource Manager][aks-rm-template] para implementar un clúster de AKS, puede definir el nombre del grupo de recursos mediante la propiedad *nodeResourceGroup*.

* El proveedor de recursos de Azure crea automáticamente el grupo de recursos secundario en su propia suscripción.
* Solo puede especificar un nombre personalizado para el grupo de recursos cuando cree el clúster.

Mientras trabaje con el grupo de recursos *MC_* , tenga en cuenta que no puede realizar lo siguiente:

* Especificar un grupo de recursos existente para el grupo *MC_* .
* Especificar otra suscripción para el grupo de recursos *MC_* .
* Cambiar el nombre del grupo de recursos *MC_* una vez se haya creado el clúster.
* Especificar los nombres de los recursos administrados del grupo de recursos *MC_* .
* Modificar o eliminar las etiquetas de los recursos administrados del grupo de recursos *MC_* . (Puede ver la información adicional en la sección siguiente).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>¿Puedo modificar etiquetas y otras propiedades de los recursos de AKS en el grupo de recursos MC_?

Si modifica o elimina etiquetas creadas por Azure y otras propiedades de recursos en el grupo de recursos *MC_* , es posible que obtenga resultados inesperados, como errores de escalado y actualización. AKS le permite crear y modificar las etiquetas personalizadas. Es posible que quiera crear o modificar etiquetas personalizadas, por ejemplo, para asignar un centro de costos o una unidad de negocio. Si modifica los recursos del grupo *MC_* en el clúster de AKS, interrumpirá el objetivo de nivel de servicio (SLO). Para obtener más información, consulte [¿AKS ofrece un contrato de nivel de servicio?](#does-aks-offer-a-service-level-agreement)

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

Actualmente, no puede modificar la lista de controladores de admisión en AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>¿Azure Key Vault se integra con AKS?

AKS no está integrado de forma nativa con Azure Key Vault en estos momentos. Sin embargo, el [proyecto FlexVolume de Azure Key Vault para Kubernetes][keyvault-flexvolume] permite realizar una integración directa desde pods de Kubernetes a los secretos de Key Vault.

## <a name="can-i-run-windows-server-containers-on-aks"></a>¿Puedo ejecutar contenedores de Windows Server en AKS?

Sí, los contenedores de Windows Server están disponibles en versión preliminar. Para ejecutar los contenedores de Windows Server en AKS, cree un grupo de nodos que ejecute Windows Server como sistema operativo invitado. Los contenedores de Windows Server solo pueden usar Windows Server 2019. Para empezar, vea [Creación de un clúster de AKS con un grupo de nodos de Windows Server][aks-windows-cli].

La compatibilidad de Windows Server con el grupo de nodos incluye algunas limitaciones que forman parte de la versión de Windows Server ascendente en el proyecto de Kubernetes. Para obtener más información sobre estas limitaciones, consulte [Windows Server containers in AKS limitations][aks-windows-limitations] (Limitaciones de los contenedores de Windows Server en AKS).

## <a name="does-aks-offer-a-service-level-agreement"></a>¿AKS ofrece un Acuerdo de Nivel de Servicio?

En un Acuerdo de Nivel de Servicio (SLA), el proveedor acuerda reembolsar al cliente el costo del servicio si no se pudo cumplir el nivel de servicio publicado. Dado que AKS es gratuito, no hay que reembolsar ningún costo, por lo que AKS no tiene ningún SLA formal. Sin embargo, AKS busca mantener una disponibilidad del 99,5 % como mínimo para el servidor de API de Kubernetes.

## <a name="why-cant-i-set-maxpods-below-30"></a>¿Por qué no puedo establecer el argumento maxPods por debajo de 30?

En AKS, puede establecer el valor `maxPods` al crear el clúster mediante el uso de las plantillas de la CLI de Azure y Azure Resource Manager. Sin embargo, Kubenet y la CNI de Azure requieren un *valor mínimo* (validado en el momento de la creación):

| Redes | Mínima | Máxima |
| -- | :--: | :--: |
| CNI de Azure | 30 | 250 |
| Kubenet | 30 | 110 |

Dado que AKS es un servicio administrado, debe implementar y administrar los complementos y los pods como parte del clúster. En el pasado, los usuarios podían definir un valor de `maxPods` menor que el valor que requerían los pods administrados para poder ejecutarse (por ejemplo, 30). Ahora, AKS calcula el número mínimo de pods mediante el uso de esta fórmula: ((maxPods o (maxPods * vm_count)) > mínimo de pods de complemento administrados.

Los usuarios no pueden reemplazar la validación de `maxPods` mínima.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>¿Puedo aplicar descuentos de las reservas de Azure en mis nodos de agente de AKS?

Los nodos de agente de AKS se facturan como máquinas virtuales de Azure estándares, por lo que, si ha comprado [reservas Azure][reservation-discounts] para el tamaño de máquina virtual que está usando en AKS, los descuentos se aplican automáticamente.

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
[aks-rm-template]: /azure/templates/microsoft.containerservice/2019-06-01/managedclusters
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948