---
title: Exploración del entorno de demostración de Azure Time Series Insights (versión preliminar) | Microsoft Docs
description: Descripción del entorno de demostración de Azure Time Series Insights (versión preliminar)
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 12/03/2018
ms.openlocfilehash: 76bdc122cab5b4ee87717c9c4df9774ecc3bd8f7
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888790"
---
# <a name="explore-the-azure-time-series-insights-preview-demo-environment"></a>Exploración del entorno de demostración de Azure Time Series Insights (versión preliminar)

Esta guía de inicio rápido muestra cómo comenzar a trabajar con el explorador en versión preliminar de Azure Time Series Insights (TSI) en un entorno de demostración gratuito. Aprenderá a usar el explorador web para visualizar grandes volúmenes de datos históricos de IoT y recorrerá las características clave del explorador de Azure Time Series Insights (versión preliminar).

Azure TSI proporciona una oferta de plataforma como servicio de un extremo a otro para ingerir, procesar, almacenar y consultar datos de escala de IoT altamente contextualizados y optimizados en series de tiempo para la exploración de datos ad-hoc, así como para el análisis operativo. Time Series Insights es una oferta diferenciada adaptada a las necesidades únicas de las implementaciones industriales de IoT.

El entorno de demostración muestra a una empresa generadora de electricidad, Contoso, que utiliza TSI para encontrar información accionable en sus datos y realizar un breve análisis de la causa principal. Contoso tiene en funcionamiento dos parques eólicos, cada uno con 10 turbinas y cada turbina tiene 20 sensores que notifican datos cada minuto a Azure IoT Hub. Los sensores recopilan información sobre las condiciones meteorológicas, la posición de inclinación y el paso de las palas, el rendimiento del generador, el comportamiento de la caja de cambios y los monitores de seguridad.

El TSI (versión preliminar) se utiliza para analizar el creciente conjunto de datos de los últimos dos años, actualmente de 40 GB, para comprender y predecir mejor tanto los errores críticos como los problemas de mantenimiento de movimiento lento.

Si no tiene una suscripción a Azure, cree una  [cuenta de Azure gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Exploración del explorador de Time Series Insights en un entorno de demostración

1. En el explorador, vaya a  [insights.timeseries.azure.com/preview/samples](https://insights.timeseries.azure.com/preview/samples).  

1. Si se le solicita, inicie sesión en el explorador de TSI con las credenciales de cuenta de Azure.

### <a name="demo-step-one"></a>Primer paso de la demostración

1. Echemos un vistazo al **aerogenerador número 7 en el parque eólico número 1**.  

    * **Acción**: actualice el rango de visión a `1/1/17 20:00 – 3/10/17 20:00 (UTC)` y agregue el sensor `Farm 1 > W7 > Generator > GeneratorSpeed`. A continuación, muestre los valores resultantes.

       ![Guía de inicio rápido uno][1]

1. Recientemente, **Contoso encontró un incendio en el aerogenerador número 7**. Vamos a profundizar aquí. Podemos ver el sensor de alerta de incendio activado durante el período del incendio.

    * **Acción**: actualice el rango de visión a `3/9/17 20:00 – 3/10/17 20:00 (UTC)` y agregue el sensor `Safety > FireAlert`.

      ![Guía de inicio rápido dos][2]

1. Veamos qué más ha ocurrido en el momento del incendio. Tanto la presión del aceite como las advertencias activas se dispararon justo antes del incendio, pero en ese momento ya era demasiado tarde para evitar el problema.

    * **Acción**: agregue el sensor `Pitch > HydraulicOilPressure` y el sensor `Pitch > ActiveWarning`.

      ![Guía de inicio rápido tres][3]

1. Si nos alejamos, podemos ver que había señales que conducían al fuego. Ambos sensores fluctuaban. ¿Entonces esto ha pasado antes?

    * **Acción**: actualice el rango de visión a `2/24/17 20:00 – 3/10/17 20:00 (UTC)`.

      ![Guía de inicio rápido cuatro][4]

1. Si examinamos los dos años completos de datos, podemos ver un evento de incendio anterior con las mismas señales. Con estos datos, creamos sistemas para detectar problemas como este a tiempo.

    * **Acción**: actualice el rango de visión a `1/1/16 – 12/31/17` (todos los datos).

       ![Guía de inicio rápido cinco][5]

### <a name="demo-step-two"></a>Segundo paso de la demostración

1. Otros problemas son más sutiles y difíciles de diagnosticar. Time Series Insights proporciona un rango de funcionalidades para ayudarnos a localizar problemas difíciles. Aquí podemos ver una interrupción del sensor de advertencia en `turbine #6` en `6/25`. Pero, ¿qué está pasando en realidad?

    * **Acción**: quite los sensores actuales. Después, actualice el rango de visión a `6/1/17 20:00 – 7/1/17 20:00 (UTC)` y agregue `Farm 1 > W6 > Safety > VoltageActuatorSwitchWarning`.

       ![Guía de inicio rápido seis][6]

1. La advertencia indica un problema con la tensión de salida del generador. Pero, ¿cuál es la causa? La potencia total de salida del generador se ve bien en un intervalo específico. Pero al agregar los datos, podemos ver una caída definitiva.

    * **Acción**: quite `VoltageActuatorSwitchWarning` y agregue `Generator > ActivePower` y actualice el intervalo a `3d`.

       ![Guía de inicio rápido siete][7]

1. Si nos desplazamos hacia adelante en el conjunto de datos, podemos ver que esto no es solo un problema transitorio. El problema continúa.

    * **Acción**: amplíe el intervalo de tiempo a la derecha.

       ![Guía de inicio rápido ocho][8]

1. Vamos a profundizar aún más. Podemos tener otros puntos de datos del sensor para ver el voltaje por fase. Pero todos parecen comparables. Coloquemos un marcador para ver los valores reales. Parece que hay un problema con la salida de la fase 3.

    * **Acción**: `Add Generator > GridVoltagePhase1, 2, & 3`. Coloque un marcador en el último punto de datos en el área visible.

       ![Guía de inicio rápido ocho][8]

1. Si vemos los tres en la misma escala, hace que la caída de la fase 3 sea aún más obvia. En este punto, estamos listos para referir este problema a nuestro equipo de mantenimiento con un buen punto de partida sobre la causa de la advertencia.  

    * **Acción**: actualice la pantalla para superponer todos los sensores en la misma escala del gráfico.

       ![Guía de inicio rápido nueve][9]

## <a name="next-steps"></a>Pasos siguientes

Está listo para crear su propio entorno de Azure TSI (versión preliminar):

> [!div class="nextstepaction"]
> [Planeamiento del entorno de Azure TSI (versión preliminar)](time-series-insights-update-plan.md)

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