---
title: 'Introducción al acelerador de soluciones de mantenimiento predictivo: Azure | Microsoft Docs'
description: Una introducción al acelerador de soluciones de mantenimiento predictivo de Azure IoT.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 822e02d42be8ce516901190af4be579d13ac841d
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888329"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Introducción al acelerador de soluciones de mantenimiento predictivo

El acelerador de soluciones de mantenimiento predictivo es una solución completa para escenarios empresariales que predice el punto en el que es probable que se produzca un error. Este acelerador se puede utilizar de forma proactiva para actividades como, por ejemplo, la optimización del mantenimiento. La solución combina servicios esenciales de los aceleradores de soluciones de IoT de Azure, como IoT Hub, Stream Analytics y un área de trabajo de [Azure Machine Learning][lnk-machine-learning]. Esta área de trabajo contiene un modelo, basado en un conjunto público de datos de ejemplo para predecir la vida útil restante (RUL) de un motor de avión. La solución proporciona una implementación completa del escenario empresarial como punto de partida para planear e implementar una solución que satisfaga sus propios requisitos empresariales específicos.

## <a name="logical-architecture"></a>Arquitectura lógica

El diagrama siguiente describe los componentes lógicos del acelerador de soluciones:

![][img-architecture]

Los elementos de color azul son servicios de Azure que se aprovisionan en la región en la que se implementó el acelerador de soluciones. La lista de regiones en las que se puede implementar el acelerador de soluciones se muestra en la [página de aprovisionamiento][lnk-azureiotsuite].

El elemento verde es un dispositivo simulado que representa un motor de avión. Más información acerca de estos dispositivos simulados en la sección [Dispositivos simulados](#simulated-devices).

Los elementos de color gris representan los componentes que implementan funcionalidades de *administración de dispositivos*. La versión actual del acelerador de soluciones de mantenimiento predictivo no aprovisiona estos recursos. Para obtener más información sobre la administración de dispositivos, consulte la [Acelerador de la solución de supervisión remota][lnk-remote-monitoring].

## <a name="azure-resources"></a>Recursos de Azure

En el Portal de Azure, desplácese al grupo de recursos con el nombre de la solución elegida para poder ver los recursos aprovisionados.

![Recursos de acelerador][img-resource-group]

Al aprovisionar el acelerador de soluciones, recibirá un mensaje de correo electrónico con un vínculo al área de trabajo de Machine Learning. También puede desplazarse al área de trabajo de Machine Learning desde la página de [aceleradores de soluciones de Microsoft Azure IoT][lnk-azureiotsuite] de la solución aprovisionada. Hay un icono disponible en esta página cuando la solución se encuentra en el estado **Listo**.

![Modelo de Machine Learning][img-machine-learning]

## <a name="simulated-devices"></a>Dispositivos simulados

En el acelerador de soluciones, un dispositivo simulado representa un motor de avión. La solución se aprovisiona con dos motores que se asignan a un avión único. Cada motor emite cuatro tipos de telemetría: Sensor 9, Sensor 11, Sensor 14 y Sensor 15 proporcionan los datos necesarios para el modelo de Machine Learning a fin de calcular la vida útil restante (RUL) del motor. Cada dispositivo simulado envía los siguientes mensajes de telemetría a IoT Hub:

*Recuento de ciclos*. Un ciclo representa un vuelo completo con una duración de entre dos y diez horas. Durante el vuelo, se capturan datos de telemetría cada media hora.

*Telemetría*. Existen cuatro sensores que representan atributos de motor. Los sensores se etiquetan genéricamente Sensor 9, Sensor 11, Sensor 14 y Sensor 15. Estos cuatro sensores representan la telemetría suficiente para obtener resultados útiles en el modelo de RUL. El modelo usado en el acelerador de soluciones se crea a partir de un conjunto de datos público que incluye datos reales del sensor del motor. Para más información sobre cómo se creó el modelo del conjunto de datos original, consulte [Predictive Maintenance Template (Plantilla de mantenimiento predictivo) en la Galería de Cortana Intelligence][lnk-cortana-analytics].

Los dispositivos simulados también pueden controlar los siguientes comandos enviados desde el centro de IoT en la solución:

| Get-Help | DESCRIPCIÓN |
| --- | --- |
| StartTelemetry |Controla el estado de la simulación.<br/>Inicia el envío de telemetría del dispositivo. |
| StopTelemetry |Controla el estado de la simulación.<br/>Detiene el envío de telemetría del dispositivo. |

IoT Hub proporciona la confirmación de los comandos del dispositivo.

## <a name="azure-stream-analytics-job"></a>Trabajo de Azure Stream Analytics

**Trabajo: telemetría** opera en la transmisión entrante de la telemetría del dispositivo mediante dos instrucciones:

* La primera selecciona todos los datos de telemetría de los dispositivos y los envía al almacenamiento de blobs. A partir de aquí, se visualizan en la aplicación web.
* La segunda calcula los valores medios del sensor con una ventana deslizante de dos minutos y envía estos datos a través del centro de eventos a un **procesador de eventos**.

## <a name="event-processor"></a>procesador de eventos
El **host de procesador de eventos** se ejecuta en un trabajo web de Azure. El **procesador de eventos** toma los valores promedio de los sensores para un ciclo completado. Luego, pasa esos valores a una API que expone el modelo entrenado para calcular la vida útil restante (RUL) de un motor. La API se expone un área de trabajo de Machine Learning que se aprovisiona como parte de la solución.

## <a name="machine-learning"></a>Machine Learning
El componente de Machine Learning usa un modelo que se deriva de los datos recopilados de los motores de avión reales. Puede navegar al área de trabajo de Machine Learning desde el icono de solución de la página [azureiotsuite.com][lnk-azureiotsuite]. El icono está disponible cuando la solución se encuentra en el estado **Listo**.

El modelo de Azure Machine Learning está disponible como una plantilla para mostrar cómo actúan estas funcionalidades a partir de los datos de telemetría del dispositivo recopilados mediante los servicios de aceleradores de soluciones de IoT. Microsoft ha creado un [modelo de regresión][lnk_regression_model] de un motor de avión basado en datos disponibles públicamente<sup>\[1\]</sup>, e instrucciones paso a paso sobre cómo utilizar dicho modelo.

El acelerador de soluciones de mantenimiento predictivo de IoT de Azure usa el modelo de regresión creado a partir de esta plantilla. El modelo se implementa en la suscripción de Azure y se expone mediante una API generada automáticamente. La solución incluye un subconjunto de los datos de prueba que representa 4 motores (de un total de 100) y 4 flujos de datos de sensor (de un total de 21). Estos datos son suficientes para proporcionar un resultado preciso del modelo entrenado.

*\[1\] A. Saxena and K. Goebel (2008). "Conjunto de datos de simulación de degradación del motor de turbofán", NASA Ames Prognostics Data Repository (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Research Center, Moffett Field, CA*

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha visto los componentes clave del acelerador de soluciones de mantenimiento predictivo, puede personalizarlo si lo desea.

También puede explorar algunas de las demás características y funcionalidades de los aceleradores de soluciones de IoT:

* [Preguntas más frecuentes acerca de los aceleradores de la solución de IoT][lnk-faq]
* [Seguridad de Internet de las cosas desde el principio][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-accelerators-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-accelerators-predictive-walkthrough/machine-learning.png

[lnk-remote-monitoring]: quickstart-predictive-maintenance-deploy.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
