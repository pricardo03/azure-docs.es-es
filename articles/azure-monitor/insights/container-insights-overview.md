---
title: Introducción a Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe Azure Monitor para contenedores, que supervisa la solución de AKS Container Insights y el valor que entrega mediante la supervisión del estado de los clústeres de AKS y Container Instances en Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: d6321564672097fbf901d1d33afac9f606fcb63a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65521832"
---
# <a name="azure-monitor-for-containers-overview"></a>Introducción a Azure Monitor para contenedores

Azure Monitor para contenedores es una característica diseñada para supervisar el rendimiento de las cargas de trabajo de contenedor implementadas en Azure Container Instances clústeres de Kubernetes administrados hospedados en Azure Kubernetes Service (AKS). La supervisión de los contenedores es fundamental, sobre todo cuando se ejecuta un clúster de producción, a escala, con varias aplicaciones.

Azure Monitor para contenedores le brinda la posibilidad de visibilizar el rendimiento mediante la recopilación de métricas del procesador y de la memoria de los controladores, nodos y contenedores disponibles en Kubernetes mediante la API de métricas. También se recopilan registros del contenedor.  Una vez habilitada la supervisión de clústeres de Kubernetes, se recopilan métricas y registros automáticamente mediante una versión en contenedor del agente de Log Analytics para Linux. Las métricas se escriben en el almacén de métricas y los datos de registro se incluyen en el almacén de registros asociado a su área de trabajo de [Log Analytics](../log-query/log-query-overview.md). 

![Azure Monitor para arquitectura de contenedores](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>¿Cómo se proporciona Azure Monitor para contenedores?

Azure Monitor para contenedores ofrece una experiencia de supervisión integral con diferentes características de Azure Monitor que le permiten comprender el rendimiento y el mantenimiento del clúster de Kubernetes y las cargas de trabajo de contenedor. Con Azure Monitor para contenedores, puede hacer lo siguiente:

* Identificar los contenedores de AKS que se ejecutan en el nodo y su utilización media tanto del procesador como de la memoria. Este conocimiento puede ayudarle a identificar cuellos de botella en los recursos.
* Identificar el uso de procesador y memoria de grupos de contenedores y sus contenedores hospedados en Azure Container Instances.  
* Identificar dónde se encuentra el contenedor en un controlador o un pod. Este conocimiento puede ayudarle a ver el rendimiento general del controlador o del pod.
* Revisar el uso de recursos de las cargas de trabajo que se ejecutan en el host que no estén relacionadas con los procesos estándar que admite el pod.
* Conocer el comportamiento del clúster tanto con cargas medias como con las más pesadas. Este conocimiento puede ayudarle a identificar los requisitos de capacidad y determinar la carga máxima que el clúster puede admitir. 
* Configurar alertas que le avisen de forma proactiva si el uso de CPU y memoria en los nodos o contenedores supera los umbrales o alertas que lo registren.  

## <a name="how-do-i-access-this-feature"></a>¿Cómo se obtiene acceso a esta característica?
Puede acceder a Azure Monitor para contenedores de dos maneras: desde Azure Monitor o directamente desde el clúster de AKS seleccionado. Desde Azure Monitor tiene una perspectiva global de todos los contenedores implementados, cuáles se supervisan y cuáles no. Esto le permite buscar y filtrar a través de las suscripciones y los grupos de recursos y, después, explorar en profundidad Azure Monitor para contenedores desde el contenedor seleccionado.  En caso contrario, puede acceder a la característica directamente desde un contenedor de AKS seleccionado desde la página de AKS.  

![Introducción a los métodos de acceso a Azure Monitor para contenedores](./media/container-insights-overview/azmon-containers-experience.png)

Si está interesado en la supervisión y la administración de hosts de contenedor de Docker y Windows y desea más información sobre la configuración, la auditoría y la utilización de recursos, consulte la [solución de supervisión de contenedores](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Pasos siguientes
Para comenzar a supervisar el clúster de AKS, revise [Cómo habilitar Azure Monitor para contenedores](container-insights-onboard.md), a fin de conocer los requisitos y los métodos disponibles para habilitar la supervisión.  
