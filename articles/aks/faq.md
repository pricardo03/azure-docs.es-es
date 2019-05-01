---
title: Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)
description: En este artículo se ofrecen respuestas a algunas de las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2019
ms.author: iainfou
ms.openlocfilehash: 04ed95317311b81af49f5d96addb203b7cfeb74a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725639"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>¿Qué regiones de Azure proporcionan actualmente Azure Kubernetes Service (AKS)?

Para obtener una lista completa de las regiones disponibles, consulte [Regiones AKS y disponibilidad][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>¿AKS admite el escalado automático de los nodos?

Sí, el escalado automático está disponible mediante el [AutoScaler de Kubernetes][auto-scaler] a partir de Kubernetes 1.10. Para más información acerca de cómo configurar y usar el escalador automático de clúster, consulte [Escalador automático de clúster en AKS][aks-cluster-autoscale].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>¿Admite AKS el control de acceso basado en roles (RBAC) de Kubernetes?

Sí, Kubernetes RBAC está habilitado de forma predeterminada cuando se crean clústeres con la CLI de Azure. RBAC puede habilitarse para los clústeres creados usando Azure Portal o plantillas.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>¿Puedo implementar AKS en mi red virtual existente?

Sí, puede implementar un clúster de AKS en una red virtual existente mediante la [característica de redes avanzadas][aks-advanced-networking].

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>¿Puedo restringir el servidor de API de Kubernetes para que solo sea accesible en mi red virtual?

De momento, no. El servidor de API de Kubernetes se expone como un nombre de dominio completo público (FQDN). Puede controlar el acceso al clúster mediante un [control de acceso basado en rol (RBAC) de Kubernetes y Azure Active Directory (AAD)][aks-rbac-aad]

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>¿Se aplican las actualizaciones de seguridad a los nodos de agente de AKS?

Sí, Azure aplica automáticamente revisiones de seguridad a los nodos del clúster siguiendo una programación nocturna. Sin embargo, es responsabilidad suya asegurarse de que los nodos se reinician si es necesario. Para realizar el reinicio de los nodos, cuenta con varias opciones:

- Manualmente, mediante Azure Portal o la CLI de Azure.
- Mediante la actualización del clúster de AKS. Las actualizaciones del clúster [acordonan y purgan los nodos][cordon-drain] automáticamente, y luego ponen cada nodo de nuevo en activo con la imagen de Ubuntu más reciente y una nueva versión de revisión o una versión secundaria de Kubernetes. Para más información, consulte [Actualización de un clúster de AKS][aks-upgrade].
- Mediante [Kured](https://github.com/weaveworks/kured), un demonio de reinicio de código abierto para Kubernetes. Kured se ejecuta como un elemento [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) y supervisa cada nodo para comprobar la presencia de un archivo que indique que hace falta un reinicio. Los reinicios del sistema operativo se administran en el clúster con el mismo [proceso de acordonar y purgar][cordon-drain] que una actualización de clúster.

Para obtener más información sobre el uso de Kured, consulte [Apply security and kernel updates to nodes in AKS][node-updates-kured] (Aplicación de actualizaciones de kernel y de seguridad en los nodos en AKS).

## <a name="why-are-two-resource-groups-created-with-aks"></a>¿Por qué se crean dos grupos de recursos con AKS?

Cada implementación de AKS abarca dos grupos de recursos:

- El primero grupo de recursos lo crea el usuario y solo contiene el recurso de servicio de Kubernetes. El proveedor de recursos de AKS crea automáticamente el segundo durante la implementación, como *MC_myResourceGroup_myAKSCluster_eastus*. Para obtener información sobre cómo puede especificar el nombre de este segundo grupo de recursos, consulte la sección siguiente.
- Este segundo grupo de recursos,como *MC_myResourceGroup_myAKSCluster_eastus*, contiene todos los recursos de infraestructura asociados con el clúster. Estos recursos incluyen las máquinas virtuales de nodos de Kubernetes, las redes virtuales y el almacenamiento. Este grupo de recursos independiente se crea para simplificar la limpieza de recursos.

Si crea recursos para usarlos con el clúster de AKS, como cuentas de almacenamiento o direcciones IP públicas reservadas, colóquelos en el grupo de recursos generado automáticamente.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>¿Proporcionar mi propio nombre para el grupo de recursos de infraestructura AKS?

Sí. De forma predeterminada, el proveedor de recursos AKS crea automáticamente un grupo de recursos secundario durante la implementación, tales como *MC_myResourceGroup_myAKSCluster_eastus*. Para cumplir con la directiva corporativa, puede proporcionar su propio nombre para este clúster administrado (*MC_*) grupo de recursos.

Para especificar su propio nombre de grupo de recursos, instale el [-versión preliminar de aks] [ aks-preview-cli] versión de la extensión de CLI de Azure *0.3.2* o una versión posterior. Cuando se crea un clúster de AKS mediante la [crear az aks] [ az-aks-create] de comandos, use el *--nodo grupo de recursos* parámetro y especifique un nombre para el grupo de recursos. Si se [usar una plantilla de Azure Resource Manager] [ aks-rm-template] para implementar un clúster de AKS, puede definir el nombre de grupo de recursos mediante el *nodeResourceGroup* propiedad.

* Este grupo de recursos se crea automáticamente el proveedor de recursos de Azure en su propia suscripción.
* Solo puede especificar un nombre de grupo de recursos personalizado cuando se crea el clúster.

No se admiten los siguientes escenarios:

* No se puede especificar un grupo de recursos existente para *MC_* grupo.
* No se puede especificar una suscripción diferente para el *MC_* grupo de recursos.
* No puede cambiar el *MC_* nombre de grupo de recursos una vez creado el clúster.
* No se puede especificar nombres para los recursos administrados dentro de la *MC_* grupo de recursos.
* No se puede modificar o eliminar etiquetas de recursos administrados dentro de la *MC_* grupo de recursos (consulte la información adicional en la sección siguiente).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>¿Puedo modificar etiquetas y otras propiedades de los recursos AKS en el grupo de recursos MC_ *?

Modificar y eliminar las etiquetas creadas en Azure y otras propiedades de recursos en el grupo de recursos *MC_* * puede provocar resultados inesperados, como escalado y errores de actualización. Se admite para crear y modificar etiquetas personalizadas adicionales, como asignar un centro de coste o unidad de negocio. La modificación de los recursos en el grupo *MC_** en el clúster de AKS interrumpe el objetivo de nivel de servicio. Para más información, consulte [¿AKS ofrece un contrato de nivel de servicio?](#does-aks-offer-a-service-level-agreement).

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

Actualmente no es posible modificar la lista de controladores de admisión en AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>¿Azure Key Vault se integra con AKS?

Actualmente AKS no está integrado de forma nativa con Azure Key Vault. Sin embargo, el [proyecto Azure KeyVault FlexVolume para Kubernetes][keyvault-flexvolume] permite realizar una integración directa desde pods de Kubernetes a los secretos de KeyVault.

## <a name="can-i-run-windows-server-containers-on-aks"></a>¿Puedo ejecutar contenedores de Windows Server en AKS?

Para ejecutar contenedores de Windows Server, es preciso ejecutar nodos basados en Windows Server. Los nodos basados en Windows Server no están disponibles en AKS por ahora. Sin embargo, puede usar Virtual Kubelet para programar contenedores Windows en Azure Container Instances y administrarlos como parte del clúster de AKS. Para más información, consulte [Virtual Kubelet con AKS][virtual-kubelet].

## <a name="does-aks-offer-a-service-level-agreement"></a>¿AKS ofrece un contrato de nivel de servicio?

En un Acuerdo de Nivel de Servicio (SLA), el proveedor acuerda reembolsar al cliente el costo del servicio si no se pudo cumplir el nivel de servicio publicado. Dado que AKS es gratuito, no hay ningún costo disponible para reembolsar y, por tanto, ningún Acuerdo de Nivel de Servicio formal. Sin embargo, AKS busca mantener una disponibilidad del 99,5 % como mínimo para el servidor de API de Kubernetes.

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

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
