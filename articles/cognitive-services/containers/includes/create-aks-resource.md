---
title: Creación de un recurso de clúster de Azure Kubernetes Service
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear recursos de Azure Kubernetes (AKS).
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 5e442f38a8e81170eeeac2f31f00a7d0eeb08d2b
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377382"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Creación de un recurso de clúster de Azure Kubernetes Service

1. Vaya a [Azure Kubernetes Service](https://ms.portal.azure.com/#create/microsoft.aks) y seleccione **Crear**.

1. Introduzca la siguiente información de la pestaña **Aspectos básicos**:

    |Configuración|Valor|
    |--|--|
    |Subscription|Seleccione una suscripción adecuada.|
    |Resource group|Seleccione un grupo de recursos disponible.|
    |Nombre del clúster de Kubernetes|Escriba un nombre (en minúsculas).|
    |Region|Seleccione una ubicación cercana.|
    |Versión de Kubernetes|1.12.8 (predeterminado).|
    |Prefijo del nombre DNS|Se crea automáticamente, pero puede invalidarlo.|
    |Tamaño del nodo|Estándar DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Número de nodos|Deje el control deslizante en el valor predeterminado.|

1. En la pestaña **Escala**, deje **Nodos virtuales** y **Conjuntos de escalado de máquinas virtuales (versión preliminar)** establecidos en sus valores predeterminados.
1. En la pestaña **Autenticación**, deje las opciones **Entidad de servicio** y **Habilitar RBAC** establecidas en sus valores predeterminados.
1. En la pestaña **Networking** (Redes), escriba las siguientes selecciones:

    |Configuración|Valor|
    |--|--|
    |Enrutamiento de aplicación HTTP|Sin|
    |Configuración de red|Básica|

1. En la pestaña **Supervisión**, asegúrese de que la opción **Habilitar la supervisión de contenedores** esté establecida en **Sí** y deje **Área de trabajo de Log Analytics** con el valor predeterminado.
1. En la pestaña **Tags** (Etiquetas), deje los pares nombre/valor en blanco por ahora.
1. Seleccione **Revisar y crear**.
1. Una vez pasada la validación, seleccione **Crear**.

> [!NOTE]
> Si se produce un error en la validación, podría deberse a un error de la entidad de servicio. Vuelva a la pestaña **Autenticación** y, luego, vaya a **Revisar y crear**, donde se debe ejecutar y superar la validación.
