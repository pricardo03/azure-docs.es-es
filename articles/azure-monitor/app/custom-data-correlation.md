---
title: Azure Application Insights | Microsoft Docs
description: ''
services: application-insights
documentationcenter: ''
author: eternovsky
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/08/2018
ms.reviewer: mbullwin
ms.author: Evgeny.Ternovsky
ms.openlocfilehash: d034bf130440fdb5b783db41161ab5a21a306478
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103119"
---
# <a name="correlating-application-insights-data-with-custom-data-sources"></a>Correlación de los datos de Application Insights con orígenes de datos personalizados

Application Insights recopila diferentes tipos de datos: excepciones, seguimientos, vistas de página y otros. Si bien esto suele ser suficiente para investigar el rendimiento, la confiabilidad y el uso de la aplicación, hay casos en los que resulta útil correlacionar los datos almacenados en Application Insights con otros conjuntos de datos completamente personalizados.

Algunas situaciones en las que es posible que desee usar datos personalizados incluyen:

- Enriquecimiento de datos o tablas de búsqueda: complementar, por ejemplo, un nombre de servidor con el propietario del servidor y la ubicación del laboratorio en el que se puede encontrar. 
- Correlación con orígenes de datos que no son de Application Insights: por ejemplo, correlacionar datos sobre una compra en una tienda web con la información del servicio de entrega de compras para determinar el grado de precisión de las estimaciones de la hora de envío. 
- Datos completamente personalizados: a muchos de nuestros clientes les encantan el lenguaje de consulta y el rendimiento de la plataforma de datos de Log Analytics que hay detrás de Application Insights, y desean usarlos para consultar datos que no están relacionados con Application Insights. Por ejemplo, para supervisar el rendimiento de un panel solar como parte de una instalación doméstica inteligente como se describe [aquí]( https://blogs.catapultsystems.com/cfuller/archive/2017/10/04/using-log-analytics-and-a-special-guest-to-forecast-electricity-generation/).

## <a name="how-to-correlate-custom-data-with-application-insights-data"></a>Correlación de datos personalizados con datos de Application Insights 

Puesto que Application Insights está respaldado por la eficaz plataforma de datos de Log Analytics, se puede usar toda la potencia de Log Analytics para la ingesta de datos. De este modo, se escriben consultas con el operador "join" que establecen una correlación de estos datos personalizados con los datos disponibles en Log Analytics. 

## <a name="ingesting-data"></a>Ingesta de datos

En esta sección, revisamos cómo ingerir los datos en Log Analytics.

Si aún no tiene una, aprovisione una nueva área de trabajo de Log Analytics siguiendo [estas instrucciones]( https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm), incluido el paso "Creación de un área de trabajo".

Para empezar a enviar datos a Log Analytics. Existen varias opciones:

- Para un mecanismo sincrónico, puede llamar directamente a [Data Collector API](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) o usar el conector de Logic Apps; simplemente busque "Azure Log Analytics" y elija la opción "Enviar datos":

 ![Captura de pantalla de la selección y la acción](./media/custom-data-correlation/01-logic-app-connector.png)  

- Para una opción asincrónica, use Data Collector API para crear una canalización de procesamiento. Consulte [este artículo](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) para más información.

## <a name="correlating-data"></a>Correlación de datos

Application Insights se basa en la plataforma de datos de Log Analytics. Por lo tanto, podemos usar [combinaciones entre recursos](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search) para correlacionar cualquier dato que se ingiera en Log Analytics con los datos de Application Insights.

Por ejemplo, podemos ingerir el inventario y las ubicaciones del laboratorio en una tabla llamada "LabLocations_CL" en un área de trabajo de Log Analytics llamada "myLA". A continuación, si quisiéramos revisar las solicitudes de la aplicación de Application Insights llamada "myAI" y correlacionar los nombres de equipo que atienden las solicitudes con las ubicaciones de estos equipos almacenados en la tabla personalizada mencionada anteriormente, podemos ejecutar la siguiente consulta en el contexto de Application Insights o de Log Analytics:

```
app('myAI').requests
| join kind= leftouter (
    workspace('myLA').LabLocations_CL
    | project Computer_S, Owner_S, Lab_S
) on $left.cloud_RoleInstance == $right.Computer
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte la referencia de [Data Collector API](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api).
- Más información sobre [combinaciones entre recursos](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).
