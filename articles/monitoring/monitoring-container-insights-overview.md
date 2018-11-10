---
title: Introducción a Azure Monitor para contenedores (versión preliminar) | Microsoft Docs
description: En este artículo se describe Azure Monitor para contenedores, que supervisa la solución de AKS Container Insights y el valor que entrega mediante la supervisión del estado de los clústeres de AKS y Container Instances en Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: b90aa9e3c627708b2640086b2b812b8c7079e5bf
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912537"
---
# <a name="azure-monitor-for-containers-preview-overview"></a>Introducción a Azure Monitor para contenedores (versión preliminar)

Azure Monitor para contenedores es una característica diseñada para supervisar el rendimiento de las cargas de trabajo de contenedor implementadas en clústeres de Kubernetes administrados y hospedados en Azure Kubernetes Service (AKS). La supervisión de los contenedores es fundamental, sobre todo cuando se ejecuta un clúster de producción, a escala, con varias aplicaciones.

Azure Monitor para contenedores le brinda la posibilidad de visibilizar el rendimiento mediante la recopilación de métricas del procesador y de la memoria de los controladores, nodos y contenedores disponibles en Kubernetes mediante la API de métricas. También se recopilan registros del contenedor.  Una vez habilitada la supervisión de clústeres de Kubernetes, estas métricas y registros se recopilan automáticamente mediante una versión en contenedor del agente de Log Analytics para Linux y se almacenan en el área de trabajo de [Log Analytics](../log-analytics/log-analytics-queries.md). 
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>¿Cómo se proporciona Azure Monitor para contenedores?

Azure Monitor para contenedores incluye varias vistas predefinidas que muestran las cargas de trabajo residentes en el contenedor y que afectan al mantenimiento del rendimiento del clúster Kubernetes supervisado, con el fin de que pueda:  

* Identificar los contenedores de AKS que se ejecutan en el nodo y su utilización media tanto del procesador como de la memoria. Este conocimiento puede ayudarle a identificar cuellos de botella en los recursos.
* Identificar dónde se encuentra el contenedor en un controlador o un pod. Este conocimiento puede ayudarle a ver el rendimiento general del controlador o del pod. 
* Revisar el uso de recursos de las cargas de trabajo que se ejecutan en el host que no estén relacionadas con los procesos estándar que admite el pod.
* Conocer el comportamiento del clúster tanto con cargas medias como con las más pesadas. Este conocimiento puede ayudarle a identificar los requisitos de capacidad y determinar la carga máxima que el clúster puede admitir. 

## <a name="how-do-i-access-this-feature"></a>¿Cómo se obtiene acceso a esta característica?
Puede acceder a Azure Monitor para contenedores de dos maneras: desde Azure Monitor o directamente desde el clúster de AKS seleccionado. Desde Azure Monitor tiene una perspectiva global de todos los contenedores implementados, cuáles se supervisan y cuáles no, lo que le permite buscar y filtrar a través de las suscripciones y los grupos de recursos y, a continuación, explorar en profundidad Azure Monitor para contenedores desde el contenedor seleccionado.  En caso contrario, puede acceder simplemente a la característica directamente desde un contenedor de AKS seleccionado desde la página de AKS.  

![Introducción a los métodos de acceso a Azure Monitor para contenedores](./media/monitoring-container-insights-overview/azmon-containers-views.png)

Si está interesado en la supervisión y la administración de hosts de contenedor de Docker y Windows y desea más información sobre la configuración, la auditoría y la utilización de recursos, consulte la [solución de contenedor](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Pasos siguientes
Para comenzar a supervisar el clúster de AKS, revise [Cómo incorporar Azure Monitor para contenedores](monitoring-container-insights-onboard.md) para conocer los requisitos y los métodos disponibles para habilitar la supervisión.  
