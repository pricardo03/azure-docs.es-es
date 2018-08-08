---
title: Preguntas más frecuentes sobre Azure Kubernetes Service
description: En este artículo se ofrecen respuestas a algunas de las preguntas más frecuentes sobre Azure Kubernetes Service.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/27/2018
ms.author: iainfou
ms.openlocfilehash: b64c770bca84fba8cbed98e420abf649897f7a17
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345861"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>¿Qué regiones de Azure proporcionan actualmente Azure Kubernetes Service (AKS)?

Consulte la documentación sobre [regiones y disponibilidad][aks-regions] de Azure Kubernetes Service para obtener la lista completa.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>¿Se aplican las actualizaciones de seguridad a los nodos de agente de AKS?

Azure aplica automáticamente revisiones de seguridad a los nodos del clúster según una programación nocturna. Sin embargo, es responsabilidad suya asegurarse de que los nodos se reinician si es necesario. Para realizar el reinicio de los nodos, cuenta con varias opciones:

- Manualmente, mediante Azure Portal o la CLI de Azure.
- Mediante la actualización del clúster de AKS. Las actualizaciones del clúster [acordonan y purgan los nodos](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) y luego los ponen de nuevo en activo con la imagen de Ubuntu más reciente. Actualice la imagen de sistema operativo en los nodos sin cambiar las versiones de Kubernetes mediante la especificación de la versión actual del clúster en `az aks upgrade`.
- Mediante [Kured](https://github.com/weaveworks/kured), un demonio de reinicio de código abierto para Kubernetes. Kured se ejecuta como un elemento [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) y supervisa cada nodo para comprobar la presencia de un archivo que indique que hace falta un reinicio. A continuación, administra reinicios del sistema operativo en el clúster, siguiendo el mismo proceso de acordonamiento y purga descrito anteriormente.

## <a name="does-aks-support-node-autoscaling"></a>¿AKS admite el escalado automático de los nodos?

Sí, el escalado automático está disponible mediante el [AutoScaler de Kubernetes][auto-scaler] a partir de Kubernetes 1.10.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>¿Admite AKS el control de acceso basado en roles (RBAC) de Kubernetes?

Sí, se puede habilitar RBAC al [implementar un clúster de AKS desde la CLI de Azure CLI o la plantilla de Azure Resource Manager](https://docs.microsoft.com/en-us/azure/aks/aad-integration). Esta funcionalidad llegará pronto a Azure Portal.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>¿Qué controladores de admisión de Kubernetes admite AKS? ¿Se pueden agregar o eliminar los controladores de admisión?

AKS admite los siguientes [controladores de admisión][admission-controllers]:

* NamespaceLifecycle
* LimitRanger
* ServiceAccount
* DefaultStorageClass
* DefaultTolerationSeconds
* MutatingAdmissionWebhook
* ValidatingAdmissionWebhook
* ResourceQuota
* DenyEscalatingExec
* AlwaysPullImages

Actualmente no es posible modificar la lista de controladores de admisión en AKS.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>¿Puedo implementar AKS en mi red virtual existente?

Sí, puede implementar un clúster de AKS en una red virtual existente mediante la [característica de redes avanzada](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>¿Puedo restringir el servidor de API de Kubernetes para que solo sea accesible en mi red virtual?

De momento, no. El servidor de API de Kubernetes se expone como un nombre de dominio completo público (FQDN). Debe controlar el acceso al clúster mediante un [control de acceso basado en rol (RBAC) de Kubernetes y Azure Active Directory (AAD)](https://docs.microsoft.com/en-us/azure/aks/aad-integration).

## <a name="is-azure-key-vault-integrated-with-aks"></a>¿Azure Key Vault se integra con AKS?

AKS no está integrado de forma nativa con Azure Key Vault por ahora. Sin embargo, el [proyecto KeyVault Flex Volume](https://github.com/Azure/kubernetes-keyvault-flexvol) permite realizar una integración directa desde pods de Kubernetes en secretos de KeyVault.

## <a name="can-i-run-windows-server-containers-on-aks"></a>¿Puedo ejecutar contenedores de Windows Server en AKS?

Para ejecutar contenedores de Windows Server, es preciso ejecutar nodos basados en Windows Server. Los nodos basados en Windows Server no están disponibles en AKS por ahora. Sin embargo, puede usar Virtual Kubelet para programar contenedores Windows en Azure Container Instances y administrarlos como parte del clúster de AKS. Para más información, consulte [Virtual Kubelet con AKS][virtual-kubelet].

## <a name="why-are-two-resource-groups-created-with-aks"></a>¿Por qué se crean dos grupos de recursos con AKS?

Cada implementación de AKS abarca dos grupos de recursos. El primero lo crea el usuario y solo contiene el recurso de servicio de Kubernetes. El proveedor de recursos de AKS crea automáticamente el segundo durante la implementación, con un nombre como *MC_myResourceGroup_myAKSCluster_eastus*. El segundo grupo de recursos contiene todos los recursos de infraestructura asociados con el clúster, como máquinas virtuales, redes y almacenamiento. Se crea para simplificar la limpieza de recursos.

Si va a crear recursos que se usarán con el clúster de AKS, como cuentas de almacenamiento o direcciones IP públicas reservadas, debe colocarlos en el grupo de recursos generado automáticamente.

## <a name="does-aks-offer-a-service-level-agreement"></a>¿AKS ofrece un contrato de nivel de servicio?

En un Acuerdo de Nivel de Servicio (SLA), el proveedor acuerda reembolsar al cliente por el costo del servicio si no se pudo cumplir el nivel de servicio publicado. Dado que AKS es gratuito, no hay ningún costo disponible para reembolsar y, por tanto, ningún Acuerdo de Nivel de Servicio formal. Sin embargo, AKS busca mantener una disponibilidad del 99,5 % como mínimo para el servidor de API de Kubernetes.

<!-- LINKS - internal -->

[aks-regions]: ./container-service-quotas.md
[virtual-kubelet]: virtual-kubelet.md

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
