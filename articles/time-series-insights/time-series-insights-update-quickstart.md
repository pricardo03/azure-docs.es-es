---
title: 'Guía de inicio rápido: Exploración del entorno de demostración de la versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Consulte una descripción del entorno de demostración de la versión preliminar de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 12/03/2018
ms.openlocfilehash: de5e853db6c6a0e98dea9251cc07b526288574e1
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276839"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Guía de inicio rápido: Exploración del entorno de demostración de la versión preliminar de Azure Time Series Insights

En esta guía de inicio rápido se explica cómo usar el explorador en versión preliminar de Azure Time Series Insights en un entorno de demostración gratuito. Aprenderá a usar el explorador web para visualizar grandes volúmenes de datos históricos de IoT y recorrerá las características clave del explorador en versión preliminar de Time Series Insights.

Time Series Insights es una oferta de plataforma como servicio (PaaS) de un extremo a otro. Puede ingerir, procesar, almacenar y consultar datos a escala de IoT optimizados para series temporales y muy contextualizados para una exploración de datos improvisada. También proporciona análisis operativos. Time Series Insights es una oferta diferenciada adaptada a las necesidades únicas de las implementaciones industriales de IoT.

El entorno de demostración muestra una empresa de generación de electricidad, Contoso. En el entorno, use Time Series Insights para buscar información útil en los datos de Contoso y realizar un breve análisis de la causa principal. Contoso gestiona dos parques con turbinas eólica, cada uno con diez turbinas. Cada turbina tiene veinte sensores que comunican los datos cada minuto a Azure IoT Hub. Los sensores recopilan información sobre las condiciones meteorológicas, la posición de inclinación y el paso de las palas, el rendimiento del generador, el comportamiento de la caja de cambios y los monitores de seguridad.

Use la versión preliminar de Time Series Insights para analizar el creciente conjunto de datos de Contoso de los dos últimos años, que actualmente tiene un tamaño de 40 GB. Esto puede ayudar a mejorar el reconocimiento y la predicción de errores críticos y problemas de mantenimiento de ralentización.

Si no tiene una suscripción a Azure, cree una  [cuenta de Azure gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Exploración del explorador de Time Series Insights en un entorno de demostración

1. En el explorador, vaya al  [entorno de Contoso Wind Farm](https://insights.timeseries.azure.com/preview/samples).  

1. Si se le solicita, inicie sesión en el explorador de Time Series Insights con las credenciales de la cuenta de Azure.

### <a name="demo-step-1"></a>Primer paso de la demostración

1. Echemos un vistazo a la turbina eólica **W7** en **Contoso Plant 1**.  

    * **Acción**: actualice el intervalo de la vista a **1/1/17 20:00 a 3/10/17 20:00 (UTC)**, agregue el sensor **Contoso Plant 1** > **W7** > **Generator System** > **GeneratorSpeed** y después muestre los valores resultantes.

       ![Guía de inicio rápido uno][1]

1. Recientemente, Contoso encontró un incendio en el **aerogenerador número 7**. Vamos a profundizar aquí. Podemos ver que el sensor de alerta de incendio se activó durante el incendio.

    * **Acción**: actualice el intervalo de la vista a **3/9/17 20:00 a 3/10/17 20:00 (UTC)** y agregue el sensor **Safety System** > **FireAlert**.

      ![Guía de inicio rápido dos][2]

1. Veamos qué más ha ocurrido en el momento del incendio. Tanto la presión del aceite como las advertencias activas se dispararon justo antes del incendio, pero en ese momento ya era demasiado tarde para evitar el problema.

    * **Acción**: agregue los sensores **Pitch System** > **HydraulicOilPressure** y **Pitch System** > **ActiveWarning**.

      ![Guía de inicio rápido tres][3]

1. Si nos alejamos, podemos ver que había señales que conducían al fuego. Ambos sensores fluctuaban. ¿Entonces este problema ha ocurrido antes?

    * **Acción**: actualice el intervalo de la vista a **2/24/17 20:00 a 3/10/17 20:00 (UTC)**.

      ![Guía de inicio rápido cuatro][4]

1. Si examinamos los dos años completos de datos, podemos ver un evento de incendio anterior con las mismas señales. Con estos datos, se pueden crear sistemas para detectar problemas como este a tiempo.

    * **Acción**: actualice el intervalo de la vista a **1/1/16 a 12/31/17** (todos los datos).

       ![Guía de inicio rápido cinco][5]

### <a name="demo-step-2"></a>Segundo paso de la demostración

1. Otros problemas son más sutiles y difíciles de diagnosticar. Time Series Insights proporciona distintos métodos para ayudarnos a localizar problemas difíciles. Aquí podemos ver una interrupción del sensor de advertencia en **W6** en **6/25**. Pero, ¿qué está pasando en realidad?

    * **Acción**: quite los sensores, actualice el intervalo de la vista a **6/1/17 20:00 a 7/1/17 20:00 (UTC)** y, a continuación, agregue el sensor **Contoso Plant 1** > **W6** > **Safety System** > **VoltageActuatorSwitchWarning**.

       ![Guía de inicio rápido seis][6]

1. La advertencia indica un problema con la tensión de salida del generador. Pero, ¿cuál es la causa? La potencia total de salida del generador se ve bien en un intervalo específico. Pero al agregar los datos, podemos ver una caída definitiva.

    * **Acción**: quite el sensor **VoltageActuatorSwitchWarning**, agregue el sensor **Generator System** > **ActivePower** y actualice el intervalo a **3d**.

       ![Guía de inicio rápido siete][7]

1. Si nos desplazamos hacia adelante en el conjunto de datos, podemos ver que este problema no es transitorio. Es continuado.

    * **Acción**: amplíe el intervalo de tiempo a la derecha.

       ![Guía de inicio rápido ocho][8]

1. Vamos a profundizar aún más. Podemos agregar otros puntos de datos del sensor para ver el voltaje por fase. Pero todos los puntos de datos son comparables. Coloquemos un marcador para ver los valores reales. Parece que hay un problema con la salida de la fase 3.

    * **Acción**: agregue los sensores **Generator System** > **GridVoltagePhase1**, **GridVoltagePhase2** y **GridVoltagePhase3**. Coloque un marcador en el último punto de datos en el área visible.

       ![Guía de inicio rápido ocho][8]

1. Si vemos los tres puntos de datos en la misma escala, la caída de la fase 3 parece aún más obvia. En este punto, estamos listos para referir el problema a nuestro equipo de mantenimiento con un buen punto de partida sobre la causa de la advertencia.  

    * **Acción**: actualice la pantalla para superponer todos los sensores en la misma escala del gráfico.

       ![Guía de inicio rápido nueve][9]

## <a name="next-steps"></a>Pasos siguientes

Está listo para crear su propio entorno en versión preliminar de Time Series Insights:

> [!div class="nextstepaction"]
> [Planeamiento de un entorno en versión preliminar de Time Series Insights](time-series-insights-update-plan.md)

<!-- Images -->
[1]: media/v2-update-quickstart/quickstart-one.png
[2]: media/v2-update-quickstart/quickstart-two.png
[3]: media/v2-update-quickstart/quickstart-three.png
[4]: media/v2-update-quickstart/quickstart-four.png
[5]: media/v2-update-quickstart/quickstart-five.png
[6]: media/v2-update-quickstart/quickstart-six.png
[7]: media/v2-update-quickstart/quickstart-seven.png
[8]: media/v2-update-quickstart/quickstart-eight.png
[9]: media/v2-update-quickstart/quickstart-nine.png
