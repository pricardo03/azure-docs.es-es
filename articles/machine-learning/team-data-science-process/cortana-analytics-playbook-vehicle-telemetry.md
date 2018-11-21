---
title: Predicción del estado de vehículo y los hábitos de conducción - Azure | Microsoft Docs
description: Aproveche las posibilidades de Cortana Intelligence para obtener información en tiempo real y predictiva del estado de los vehículos y los hábitos de conducción.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.openlocfilehash: 0594b9b40fdf8647f99946a31dbb47a95c9694ac
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300360"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Cuaderno de estrategias de la solución Vehicle Telemetry Analytics

Los superequipos han salido del laboratorio y están aparcados ahora en nuestro garaje. Se están incorporando en vehículos vanguardistas que contienen un gran número de sensores, lo que les permite supervisar millones de eventos por segundo y hacer un seguimiento de estos. En 2020, la mayoría de estos vehículos estarán conectados a Internet. Explotar esta riqueza de datos ofrece mejor seguridad, confiabilidad y experiencia de conducción. Microsoft convierte ese sueño en realidad gracias a Cortana Intelligence.

Cortana Intelligence es un conjunto de análisis avanzados y macrodatos totalmente administrado que puede usar para transformar los datos en acciones inteligentes. La plantilla de la solución Cortana Intelligence Vehicle Telemetry Analytics ilustra cómo los fabricantes y los concesionarios de automóviles, así como las compañías de seguros, pueden obtener información predictiva y en tiempo real sobre el estado de los vehículos y los hábitos de conducción.

La solución se implementa como [patrón de arquitectura lambda](https://en.wikipedia.org/wiki/Lambda_architecture) en el que se muestran todas las posibilidades de la plataforma Cortana Intelligence para procesar datos por lotes y en tiempo real.

## <a name="architecture"></a>Arquitectura
La arquitectura de la solución Vehicle Telemetry Analytics se muestra en este diagrama:

![Diagrama de la arquitectura de la solución](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Esta solución incorpora los siguientes componentes de Cortana Intelligence y presenta su integración:

* **Azure Event Hubs** introduce millones de eventos de telemetría del vehículo en Azure.
* **Azure Stream Analytics** proporciona información en tiempo real sobre el estado del vehículo y conserva esos datos en almacenamiento a largo plazo para un análisis por lotes más completo.
* **Azure Machine Learning** detecta anomalías en tiempo real y usa el procesamiento por lotes para proporcionar información detallada de predicción.
* **Azure HDInsight** transforma los datos a escala.
* **Azure Data Factory** controla la orquestación, la programación, la administración de recursos y la supervisión de la canalización del procesamiento por lotes.
* **Power BI** ofrece a esta solución un panel completo para datos en tiempo real y visualizaciones de análisis predictivo.

Esta solución tiene acceso a dos orígenes de datos diferentes: 

* **Diagnósticos y señales de vehículos simuladas**: un simulador telemático de vehículo emite información de diagnóstico y señales que se corresponden con el estado del vehículo y el patrón de conducción en un momento determinado. 
* **Catálogo del vehículo**: este conjunto de datos de referencia asigna los números de chasis a los modelos correspondientes.

## <a name="next-steps"></a>Pasos siguientes

Para explorar más esta solución, consulte el [guión de procedimientos de la solución Vehicle Telemetry Analytics: profundización en la solución](cortana-analytics-playbook-vehicle-telemetry-deep-dive.md).

Para información sobre cómo configurar los informes de Power BI y los paneles para esta solución, consulte las [instrucciones de configuración del panel de Power BI de la plantilla de solución de Vehicle Telemetry Analytics](cortana-analytics-playbook-vehicle-telemetry-powerbi.md).
