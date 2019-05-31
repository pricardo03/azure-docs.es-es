---
title: Azure Red Hat OpenShift preguntas más frecuentes | Microsoft Docs
description: Aquí encontrará respuestas a preguntas habituales sobre Microsoft Azure Red Hat OpenShift
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 2001b849e9c43d552889475ca237c52b141f3f04
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306261"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift preguntas más frecuentes

Este artículo tratan las preguntas más frecuentes (P+f) acerca de Microsoft Azure Red Hat OpenShift.

## <a name="how-do-i-get-started"></a>¿Cómo empiezo?

Para poder usar Azure Red Hat OpenShift, deberá adquirir un mínimo de 4 nodos de aplicación de Azure Red Hat OpenShift reservado.

Si es cliente de Azure,[comprar instancias reservadas de Azure Red Hat OpenShift](https://aka.ms/openshift/buy) a través del portal de Azure. Después de la compra, se activará su suscripción dentro de 24 horas, tras el cual podrá para aprovisionamiento de clústeres.

Si no es un cliente de Azure, [póngase en contacto con ventas](https://aka.ms/openshift/contact-sales) y rellene el formulario de venta en la parte inferior de la página para iniciar el proceso.

Hacer referencia a la [página de precios de Azure Red Hat OpenShift](https://aka.ms/openshift/pricing) para obtener más información.

## <a name="which-azure-regions-are-supported"></a>¿Qué regiones de Azure son compatibles?

Consulte [admite recursos](supported-resources.md#azure-regions) para obtener una lista de las regiones globales que sean compatibles con Azure Red Hat OpenShift.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>¿Puedo implementar un clúster en una red virtual existente?

 No. Pero puede conectarse a una red virtual existente mediante el emparejamiento de un clúster de Azure Red Hat OpenShift. Consulte [conectar la red virtual de un clúster con una red virtual existente ](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) para obtener más información.

## <a name="what-cluster-operations-are-available"></a>¿Qué operaciones de clúster están disponibles?

Solo puede escalar o reducir verticalmente el número de nodos de proceso. No se permiten ninguna otra modificaciones el `Microsoft.ContainerService/openShiftManagedClusters` recursos después de la creación. El número máximo de nodos de proceso se limita a 20.

## <a name="what-virtual-machine-sizes-can-i-use"></a>¿Qué tamaños de máquina virtual se debe usar?

Consulte [tamaños de máquina virtual de Azure Red Hat OpenShift](supported-resources.md#virtual-machine-sizes) para obtener una lista de tamaños de máquina virtual que puede usar con un clúster de Azure Red Hat OpenShift.

## <a name="is-data-on-my-cluster-encrypted"></a>¿Son los datos en mi clúster cifrado?

De forma predeterminada, no hay cifrado en reposo. La plataforma de Azure Storage cifra automáticamente los datos antes de guardar la base de datos y descifra los datos antes de la recuperación. Consulte [Azure Storage Service Encryption para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) para obtener más información.

## <a name="can-i-use-prometheusgrafana-to-monitor-containers-and-manage-capacity"></a>¿Puedo usar Prometheus/Grafana para supervisar los contenedores y administrar la capacidad?

No, no en el momento actual.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>¿Es el registro de Docker disponible externamente por lo que puedo usar herramientas como Jenkins?

Está disponible en el registro de Docker `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` sin embargo, no se proporciona una garantía de durabilidad de almacenamiento seguro. También puede usar [Azure Container Registry](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>¿Se admiten el espacio de nombres entre redes?

Cliente y los administradores de proyectos individuales pueden personalizar redes de entre-espacio de nombres (incluido deniega) en un uso de cada proyecto `NetworkPolicy` objetos.

## <a name="can-an-admin-manage-users-and-quotas"></a>¿Puede administrar un administrador de usuarios y las cuotas?

Sí. Un administrador de Red Hat OpenShift de Azure puede administrar usuarios y las cuotas además de acceder a todos los usuarios crean proyectos.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>¿Puedo restringir un clúster con solo determinados usuarios de Azure AD?

Sí. Puede restringir qué Azure AD usuarios pueden iniciar sesión en un clúster mediante la configuración de la aplicación de Azure AD. Para obtener más información, vea [Cómo: Restringir la aplicación a un conjunto de usuarios](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>¿Puede un clúster tiene nodos de proceso en varias regiones de Azure?

 No. Todos los nodos de un clúster de Azure Red Hat OpenShift deben originarse a partir de la misma región de Azure.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>¿Son los nodos de infraestructura y maestro abstraen tal como están con Azure Kubernetes Service (AKS)?

 No. Todos los recursos, incluida a la maestra del clúster, se ejecutan en su suscripción de cliente. Estos tipos de recursos se colocan en un grupo de recursos de solo lectura.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>¿Es Open Service Broker para Azure (OSBA) admitidas?

Sí. Puede usar OSBA con Azure Red Hat OpenShift. Consulte [Open Service Broker para Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template) para obtener más información.
