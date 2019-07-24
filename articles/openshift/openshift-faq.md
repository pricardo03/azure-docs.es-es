---
title: Preguntas más frecuentes sobre Red Hat OpenShift en Azure | Microsoft Docs
description: Aquí tiene respuestas a las preguntas habituales sobre Red Hat OpenShift en Azure.
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 32eb2c47ed46aed8e2e3755a83437a21391295c5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122963"
---
# <a name="azure-red-hat-openshift-faq"></a>Preguntas más frecuentes de Red Hat OpenShift en Azure

En este artículo se tratan las preguntas más frecuentes (P+F) sobre Red Hat OpenShift en Microsoft Azure.

## <a name="how-do-i-get-started"></a>¿Cómo empiezo?

Para poder usar Red Hat OpenShift en Azure, deberá adquirir un mínimo de cuatro nodos de aplicación reservados de Red Hat OpenShift en Azure.

Si es cliente de Azure, [compre instancias reservadas de Red Hat OpenShift en Azure](https://aka.ms/openshift/buy) a través de Azure Portal. Después de la compra, la suscripción se activará en un plazo de 24 horas, tras el cual podrá aprovisionar clústeres.

Si no es cliente de Azure, [póngase en contacto con el departamento comercial](https://aka.ms/openshift/contact-sales) y rellene el formulario de ventas de la parte inferior de la página para iniciar el proceso.

Para más información, consulte la [página de precios de Red Hat OpenShift en Azure](https://aka.ms/openshift/pricing).

## <a name="which-azure-regions-are-supported"></a>¿Qué regiones de Azure se admiten?

Vea [Recursos admitidos](supported-resources.md#azure-regions) para obtener una lista de las regiones globales que son compatibles con Red Hat OpenShift en Azure.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>¿Puedo implementar un clúster en una red virtual existente?

No. Pero puede conectar un clúster de Red Hat OpenShift en Azure en una red virtual existente mediante emparejamiento. Vea [Conexión de la red virtual del clúster a una red virtual existente](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) para obtener más información.

## <a name="what-cluster-operations-are-available"></a>¿Qué operaciones de clúster están disponibles?

Solo puede escalar o reducir verticalmente el número de nodos de proceso. No se permite ninguna otra modificación al recurso `Microsoft.ContainerService/openShiftManagedClusters` después de la creación. El número máximo de nodos de proceso está limitado a 20.

## <a name="what-virtual-machine-sizes-can-i-use"></a>¿Qué tamaños de máquina virtual puedo usar?

Consulte los [tamaños de máquina virtual de Red Hat OpenShift en Azure](supported-resources.md#virtual-machine-sizes) para obtener una lista de los tamaños de máquina virtual que puede usar con un clúster de Red Hat OpenShift en Azure.

## <a name="is-data-on-my-cluster-encrypted"></a>¿Están cifrados los datos de mi clúster?

De forma predeterminada, hay cifrado en reposo. La plataforma de Azure Storage cifra automáticamente los datos antes de almacenarlos y los descifra antes de recuperarlos. Consulte [Cifrado del servicio Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) para obtener más detalles.

## <a name="can-i-use-prometheusgrafana-to-monitor-my-applications"></a>¿Puedo usar Prometheus o Grafana para supervisar mis aplicaciones?

Sí, puede implementar Prometheus en su espacio de nombres y supervisar las aplicaciones en él.

## <a name="can-i-use-prometheusgrafana-to-monitor-metrics-related-to-cluster-health-and-capacity"></a>¿Puedo usar Prometheus o Grafana para supervisar las métricas relacionadas con la capacidad y el mantenimiento del clúster?

No, actualmente no.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>¿Está disponible externamente el registro de Docker para poder usar herramientas como Jenkins?

El registro de Docker está disponible desde `https://docker-registry.apps.<clustername>.<region>.azmosa.io/`, pero no se proporciona una gran garantía de durabilidad de almacenamiento. También puede usar [Azure Container Registry](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>¿Se admiten las redes entre espacios de nombres?

Los administradores de proyectos de cliente e individuales pueden personalizar las redes entre espacios de nombres (incluido denegarlas) por proyecto usando objetos `NetworkPolicy`.

## <a name="can-an-admin-manage-users-and-quotas"></a>¿Puede un administrador administrar usuarios y cuotas?

Sí. Un administrador de Red Hat OpenShift en Azure puede administrar usuarios y cuotas, además de acceder a todos los proyectos creados por los usuarios.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>¿Puedo restringir un clúster a solo determinados usuarios de Azure AD?

Sí. Puede restringir qué usuarios de Azure AD pueden iniciar sesión en un clúster configurando la aplicación de Azure AD. Para más información, consulte [Procedimientos para: Restricción de la aplicación a un conjunto de usuarios](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users).

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>¿Un clúster puede tener nodos de proceso en varias regiones de Azure?

No. Todos los nodos de un clúster de Red Hat OpenShift en Azure deben partir de la misma región de Azure.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>¿Los nodos de infraestructura y maestros se abstraen como lo hacen con Azure Kubernetes Service (AKS)?

No. Todos los recursos, incluido el clúster maestro, se ejecutan en su suscripción de cliente. Estos tipos de recursos se colocan en un grupo de recursos de solo lectura.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>¿Es compatible Open Service Broker para Azure (OSBA)?

Sí. Puede usar OSBA con Red Hat OpenShift en Azure. Vea [Open Service Broker para Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template) para obtener más información.

## <a name="i-am-trying-to-peer-into-a-virtual-network-in-a-different-subscription-but-getting-failed-to-get-vnet-cidr-error"></a>Estoy intentando mirar una red virtual en otra suscripción pero recibo el error `Failed to get vnet CIDR`.

En la suscripción que tiene la red virtual, asegúrese de registrar el proveedor `Microsoft.ContainerService` con `az provider register -n Microsoft.ContainerService --wait`. 
