---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear recursos de Azure Kubernetes (AKS).
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1d68c08f6dfca74c38973af1686d614f3f10cc28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455099"
---
## <a name="create-an-azure-kubernetes-service-aks-cluster-resource"></a>Creación de un recurso de clúster de Azure Kubernetes Service (AKS)

1. Vaya a [Create](https://ms.portal.azure.com/#create/microsoft.aks) (Crear) en Kubernetes Services.

1. En la pestaña **Basics** (Aspectos básicos), escriba los siguientes detalles:

    |Configuración|Valor|
    |--|--|
    |Subscription|Seleccione una suscripción adecuada|
    |Grupos de recursos|Seleccione un grupo de recursos disponible|
    |Nombre del clúster de Kubernetes|Nombre que quiera (en minúsculas)|
    |Region|Seleccione una ubicación cercana|
    |Versión de Kubernetes|1.12.8 (predeterminado)|
    |Prefijo del nombre DNS|Se crea automáticamente, pero puede invalidarlo si lo desea|
    |Tamaño del nodo|Estándar DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Número de nodos|Deje el control deslizante en el valor predeterminado|

1. En la pestaña **Scale** (Escala), deje los *nodos virtuales* y el *conjunto de escalado de máquinas virtuales* con sus los valores predeterminados.
1. En la pestaña **Authentication** (Autenticación), deje la *entidad de servicio* y la opción *Enable RBAC* (Habilitar RBAC) con sus los valores predeterminados.
1. En la pestaña **Networking** (Redes), escriba las siguientes selecciones:

    |Configuración|Valor|
    |--|--|
    |Enrutamiento de aplicación HTTP|Sin|
    |Configuración de red|Básica|

1. En la pestaña **Monitoring** (Supervisión), asegúrese de que la opción *Enable container monitoring* (Habilitar la supervisión del contenedor) esté establecida en **Yes** (Sí) y deje el *área de trabajo de Log Analytics* con su valor predeterminado
1. En la pestaña **Tags** (Etiquetas), deje los pares nombre/valor en blanco por ahora.
1. Haga clic en **Review and Create** (Revisar y crear).
1. Una vez pasada la validación, seleccione **Create** (Crear).

> [!NOTE]
> Si se produce un error en la validación, es posible que se deba a un error de la *entidad de servicio*. Vuelva a la pestaña *Authentication* (Autenticación), y seleccione la pestaña *Review + create* (Revisar + crear) donde se debe ejecutar y pasar la validación.
