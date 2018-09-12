---
title: Introducción a Azure Kubernetes Service
description: Azure Kubernetes Service simplifica la implementación y administración de aplicaciones basadas en contenedor en Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 294ccd70e0339ed5ad68022f002e4864fc52b452
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286727"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) simplifica la implementación de los clústeres de Kubernetes administrados en Azure. AKS reduce la complejidad y la sobrecarga operativa de la administración de Kubernetes al descargar gran parte de esa responsabilidad en Azure. Como servicio hospedado de Kubernetes, Azure maneja tareas críticas como la supervisión del estado y el mantenimiento para usted. Además, el servicio es gratuito, solo se paga por los nodos de agente en los clústeres, no por los maestros.

En este documento se proporciona información general sobre las características de Azure Kubernetes Service (AKS).

## <a name="flexible-deployment-options"></a>Opciones de implementación flexibles

Azure Kubernetes Service ofrece opciones de implementación basada en plantillas (plantillas de Resource Manager y Terraform), la línea de comandos o el portal. Al implementar un clúster de AKS, el maestro y todos los nodos de Kubernetes se implementan y configuran automáticamente. Otras características como redes avanzadas, integración con Azure Active Directory y supervisión se pueden configurar también durante el proceso de implementación.

Para más información, consulte el [inicio rápido del portal de AKS][aks-portal] o el [inicio rápido de la CLI de AKS][aks-cli].

## <a name="identity-and-security-management"></a>Administración de identidades y seguridad

Los clústeres de AKS admiten [control de acceso basado en rol (RBAC)][kubernetes-rbac]. Un clúster de AKS también se puede configurar para integrarse con Azure Active Directory. En esta configuración, se puede configurar el acceso a Kubernetes en función de la identidad y la pertenencia a grupos de Azure Active Directory.

Para más información, consulte [Integrate Azure Active Directory with AKS][aks-aad] (Integración de Azure Active Directory con AKS).

## <a name="integrated-logging-and-monitoring"></a>Supervisión y registro integrados

El mantenimiento del contenedor ofrece visibilidad sobre el rendimiento al recopilar métricas de procesador y memoria de contenedores, nodos y controladores. También se recopilan registros del contenedor. Estos datos se almacenan en el área de trabajo de Log Analytics y están disponibles mediante Azure Portal, la CLI de Azure o un punto de conexión de REST.

Para más información, consulte [Supervisión del mantenimiento de contenedores de Azure Kubernetes Service][container-health].

## <a name="cluster-node-scaling"></a>Escalado de nodos de clúster

A medida que aumenta la demanda de recursos, los nodos de un clúster AKS se pueden escalar horizontalmente para ajustarse a ella. Si la demanda de recursos desciende, se puede reducir horizontalmente el clúster para quitar nodos. Las operaciones de escalado de AKS pueden realizarse mediante Azure Portal o la CLI de Azure.

Para más información, consulte [Escalado de un clúster de Azure Kubernetes Service (AKS)][aks-scale].

## <a name="cluster-node-upgrades"></a>Actualizaciones de nodos de clúster

Azure Kubernetes Service ofrece varias versiones de Kubernetes. A medida que hay nuevas versiones disponibles en AKS, el clúster se puede actualizar mediante Azure Portal o la CLI de Azure. Durante el proceso de actualización, los nodos se acordonan y vacían minuciosamente para minimizar las interrupciones en las aplicaciones en ejecución.

Para más información, consulte [Actualización de un clúster de Azure Kubernetes Service (AKS)][aks-upgrade].

## <a name="http-application-routing"></a>Enrutamiento de aplicación HTTP

La solución de enrutamiento de aplicación HTTP facilita el acceso a las aplicaciones implementadas en el clúster de AKS. Cuando está habilitada, la solución de enrutamiento de aplicaciones HTTP configura un controlador de entrada en el clúster de AKS. A medida que se implementan las aplicaciones, los nombres DNS con acceso público se configuran automáticamente.

Para más información, consulte [Enrutamiento de aplicación HTTP][aks-http-routing].

## <a name="gpu-enabled-nodes"></a>Nodos habilitados para GPU

AKS admite la creación de grupos de nodo habilitados para GPU. Azure proporciona actualmente única o varias máquinas virtuales habilitadas para GPU. Las máquinas virtuales habilitadas para GPU están diseñadas para cargas de trabajo de proceso intensivo, uso intensivo de gráficos y visualización.

Para más información, consulte [Uso de GPU en AKS][aks-gpu].

## <a name="development-tooling-integration"></a>Integración de herramientas de desarrollo

Kubernetes cuenta con un completo ecosistema de herramientas de desarrollo y administración, como Helm, Draft y la extensión de Kubernetes para Visual Studio Code. Estas herramientas funcionan sin problemas con Azure Kubernetes Service.

Además, Azure Dev Spaces proporciona una experiencia de desarrollo rápida e iterativa para los equipos. Con una configuración mínima, puede ejecutar y depurar contenedores directamente en Azure Kubernetes Service (AKS).

Para más información, consulte [Azure Dev Spaces][azure-dev-spaces].

El proyecto de Azure DevOps ofrece una solución sencilla para llevar a Azure el código y el repositorio de Git existentes. El proyecto de DevOps crea automáticamente recursos de Azure, como AKS (una canalización de versión en VSTS que incluye una definición de compilación para integración continua), configura una definición de versión para implementación continua y, a continuación, crea un recurso de Azure Application Insights para la supervisión.

Para más información, consulte [Proyecto de Azure DevOps][azure-devops].

## <a name="virtual-network-integration"></a>Integración de la red virtual

Un clúster de AKS se puede implementar en una red virtual existente. En esta configuración, cada pod del clúster tiene asignada una dirección IP en la red virtual y puede comunicarse directamente con los otros pods del clúster y otros nodos de la red virtual. Un pod se puede conectar también con otros servicios de una red virtual emparejada y con redes locales a través de ExpressRoute y conexiones VPN de sitio a sitio (S2S).

Para más información, consulte [Introducción a las redes AKS][aks-networking].

## <a name="private-container-registry"></a>Registro de contenedor privado

Realice la integración con Azure Container Registry (ACR) para el almacenamiento privado de las imágenes de Docker.

Para más información, consulte [Azure Container Registry (ACR)][acr-docs].

## <a name="storage-volume-support"></a>Compatibilidad con volúmenes de almacenamiento

Azure Kubernetes Service (AKS) admite el montaje de volúmenes de almacenamiento para los datos persistentes. Los clústeres de AKS se crean con compatibilidad con archivos y discos de Azure.

Para más información, consulte las secciones relativas a [archivos de Azure][azure-files] y [discos de Azure][azure-disk].

## <a name="docker-image-support"></a>Compatibilidad con imágenes de Docker

Azure Kubernetes Service (AKS) admite el formato de imagen de Docker.

## <a name="kubernetes-certification"></a>Certificación de Kubernetes

Azure Kubernetes Service (AKS) tiene el certificado CNCF de cumplimiento con Kubernetes.

## <a name="regulatory-compliance"></a>Cumplimiento de normativas

Azure Kubernetes Service (AKS) cumple con SOC, ISO y PCI DSS.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la implementación y administración de AKS en la Guía de inicio rápidos de AKS.

> [!div class="nextstepaction"]
> [Inicio rápido de AKS][aks-cli]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./aad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./networking-overview.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./scale-cluster.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/en-us/azure/devops-project/overview
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md

