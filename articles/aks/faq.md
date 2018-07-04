---
title: Preguntas más frecuentes sobre Azure Kubernetes Service
description: En este artículo se ofrecen respuestas a algunas de las preguntas más frecuentes sobre Azure Kubernetes Service.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/25/2018
ms.author: nepeters
ms.openlocfilehash: 5155d0c85e5b3698b0a13d2d5256a235858f0e82
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36940010"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>¿Qué regiones de Azure proporcionan actualmente Azure Kubernetes Service (AKS)?

- Australia Oriental
- Centro de Canadá
- Este de Canadá
- Central EE. UU:
- Este de EE. UU
- Este de EE. UU. 2
- Europa del Norte
- Sur del Reino Unido 2
- Europa occidental
- Oeste de EE. UU
- Oeste de EE. UU. 2

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>¿Se aplican las actualizaciones de seguridad a los nodos de agente de AKS?

Azure aplica automáticamente revisiones de seguridad a los nodos del clúster según una programación nocturna. Sin embargo, es responsabilidad suya asegurarse de que los nodos se reinician si es necesario. Para realizar el reinicio de los nodos, cuenta con varias opciones:

- Manualmente, mediante Azure Portal o la CLI de Azure.
- Mediante la actualización del clúster de AKS. Las actualizaciones del clúster [acordonan y purgan los nodos](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) y luego los ponen de nuevo en activo con la imagen de Ubuntu más reciente. Actualice la imagen de sistema operativo en los nodos sin cambiar las versiones de Kubernetes mediante la especificación de la versión actual del clúster en `az aks upgrade`.
- Mediante [Kured](https://github.com/weaveworks/kured), un demonio de reinicio de código abierto para Kubernetes. Kured se ejecuta como un elemento [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) y supervisa cada nodo para comprobar la presencia de un archivo que indique que hace falta un reinicio. A continuación, organiza reinicios en el clúster, siguiendo el mismo proceso de acordonamiento y purga descrito anteriormente.

## <a name="does-aks-support-node-autoscaling"></a>¿AKS admite el escalado automático de los nodos?

Sí, el escalado automático está disponible mediante el [AutoScaler de Kubernetes][auto-scaler] a partir de Kubernetes 1.10.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>¿Admite AKS el control de acceso basado en roles (RBAC) de Kubernetes?

Sí, se puede habilitar RBAC al implementar un clúster de AKS desde la CLI de Azure CLI o la plantilla de Azure Resource Manager. Esta funcionalidad llegará pronto a Azure Portal.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>¿Qué controladores de admisión de Kubernetes admite AKS? ¿Es posible configurar esto?

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

## <a name="is-azure-key-vault-integrated-with-aks"></a>¿Azure Key Vault se integra con AKS?

AKS no está integrado de forma nativa con Azure Key Vault por ahora. No obstante, existen soluciones de la comunidad como [acs-keyvault-agent de Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>¿Puedo ejecutar contenedores de Windows Server en AKS?

Para ejecutar contenedores de Windows Server, es preciso ejecutar nodos basados en Windows Server. Los nodos basados en Windows Server no están disponibles en AKS por ahora. Si necesita ejecutar contenedores de Windows Server en Kubernetes en Azure, consulte la [documentación de acs-engine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>¿Por qué se crean dos grupos de recursos con AKS?

Cada implementación de AKS abarca dos grupos de recursos. El primero lo crea el usuario y solo contiene el recurso de servicio de Kubernetes. El proveedor de recursos de AKS crea automáticamente el segundo durante la implementación, con un nombre como *MC_myResourceGroup_myAKSCluster_eastus*. El segundo grupo de recursos contiene todos los recursos de infraestructura asociados con el clúster, como máquinas virtuales, redes y almacenamiento. Se crea para simplificar la limpieza de recursos.

Si va a crear recursos que se usarán con el clúster de AKS, como cuentas de almacenamiento o direcciones IP públicas reservadas, debe colocarlos en el grupo de recursos generado automáticamente.

## <a name="does-aks-offer-a-service-level-agreement"></a>¿AKS ofrece un contrato de nivel de servicio?

En un Acuerdo de Nivel de Servicio (SLA), el proveedor acuerda reembolsar al cliente por el costo del servicio si no se pudo cumplir el nivel de servicio publicado. Dado que AKS es gratuito, no hay ningún costo disponible para reembolsar y, por tanto, ningún Acuerdo de Nivel de Servicio formal. Sin embargo, buscamos mantener una disponibilidad del 99,5 % como mínimo para el servidor de API de Kubernetes.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
