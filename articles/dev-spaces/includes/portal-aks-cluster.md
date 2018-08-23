---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: aeb4ff26b05a5542fd77ac32589c8f8efb622798
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "40166533"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Creación de un clúster de Kubernetes habilitado para Azure Dev Spaces

1. Inicie sesión en Azure Portal en http://portal.azure.com.
1. Elija **Crear un recurso** > busque **Kubernetes** > seleccione **Kubernetes Service** > **Crear**.

   Complete los pasos siguientes en cada encabezado del formulario de creación del clúster de AKS.

    - **DETALLES DEL PROYECTO**: seleccione una suscripción de Azure y un grupo de recursos de Azure nuevo o existente.
    - **DETALLES DEL CLÚSTER**: escriba un nombre, una región (actualmente, debe elegir Eastus, Central US, WestEurope, WestUS2, CanadaCentral o CanadaEast), la versión y el prefijo de nombre de DNS del clúster de AKS.
    - **Escala**: seleccione un tamaño de máquina virtual para los nodos de agente de AKS y el número de nodos. Si está empezando a trabajar con Azure Dev Spaces, un nodo es suficiente para explorar todas las características. El número de nodos se puede ajustar fácilmente en cualquier momento después de implementar el clúster. Tenga en cuenta que el tamaño de máquina virtual no se puede cambiar después de crear un clúster de AKS. Sin embargo, una vez que se ha implementado un clúster de AKS, puede crear fácilmente un nuevo clúster de AKS con máquinas virtuales más grandes y usar Dev Spaces para implementar de nuevo en ese clúster más grande si necesita escalar verticalmente.

   Asegúrese de elegir Kubernetes versión 1.9.6 o posterior.

   ![Opciones de configuración de Kubernetes](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Seleccione **Siguiente: autenticación** cuando haya terminado.

1. Elija la configuración deseada para el control de acceso basado en rol (RBAC). Azure Dev Spaces admite los clústeres con RBAC habilitados o deshabilitados.

    ![Configuración de RBAC](../media/common/k8s-RBAC.PNG)

1. Asegúrese de que el enrutamiento de aplicación HTTP está habilitado.

   ![Habilitación del enrutamiento de aplicación HTTP](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!IMPORTANT]
    > Asegúrese de habilitar el enrutamiento de aplicación HTTP cuando se crea el clúster de AKS. No es posible cambiar esta configuración más adelante.

1. Seleccione **Review + create** (Revisar y crear) y, a continuación, **Create** cuando haya terminado.
