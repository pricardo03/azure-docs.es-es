---
title: Configuración de un modelo de dispositivo personalizado en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar un modelo de dispositivo personalizado en el acelerador de soluciones Simulación de dispositivos.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: 61a7c5314df541b4b44a286efe982f4bf93256e3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967496"
---
# <a name="device-models"></a>Modelos de dispositivos

Al configurar una simulación, puede elegir si usar un modelo de dispositivo existente con un conjunto predefinido de sensores o crear un modelo de dispositivo personalizado con los sensores simulados que prefiera. Los sensores personalizados le permiten modelar con más precisión los dispositivos reales.

## <a name="pre-configured-device-models"></a>Modelos de dispositivos preconfigurados

El acelerador de solución Simulación de dispositivos proporciona tres modelos de dispositivos preconfigurados: refrigeradores, ascensores y camiones.

Los modelos de dispositivos preconfigurados incluyen varios sensores con comportamientos avanzados definidos en un archivo JavaScript. No puede configurar estos comportamientos en la interfaz de usuario Web.

En la tabla siguiente se muestran las configuraciones para cada modelo de dispositivo preconfigurado:

| Modelo de dispositivo  | Sensor           | Unidad  |
| ------------- | ---------------- | ----- |
| Refrigerador       | humedad         | %     |
|               | presión         | psig  |
|               | temperatura      | F     |
| Ascensor      | Planta            |       |
|               | Vibración        | MM    |
|               | Temperatura      | F     |
| Camión         | Latitud         |       |
|               | Longitud        |       |
|               | velocidad            | km/h   |
|               | cargotemperature | F     |

## <a name="custom-device-models"></a>Modelos de dispositivos personalizados

Los modelos de dispositivos personalizados le permiten modelar los sensores que mejor modelan sus propios dispositivos. Un dispositivo personalizado puede tener hasta 10 sensores personalizados.

Cuando selecciona el tipo de modelo del dispositivo personalizado, puede agregar nuevos sensores haciendo clic en **+ Agregar sensor**.

[![Configuración de sensores](./media/iot-accelerators-device-simulation-custom-model/configuresensors-inline.png)](./media/iot-accelerators-device-simulation-custom-model/configuresensors-expanded.png#lightbox)

Los sensores personalizados tienen las siguientes propiedades:

| Campo     | DESCRIPCIÓN |
| --------- | ----------- |
| Nombre del sensor | Un nombre descriptivo para el sensor, como **temperatura** o **velocidad**.  |
| Comportamiento  | Los comportamientos permiten a los datos de telemetría variar de un mensaje al siguiente para simular datos reales. **Incremento** aumenta el valor en uno en cada mensaje enviado empezando por el valor mínimo. Una vez que se alcanza el valor máximo, se empieza de nuevo por el valor mínimo. **Decremento** se comporta de la misma manera que **Incremento** pero cuenta hacia atrás. El comportamiento **Aleatorio** genera un valor aleatorio entre los valores mínimos y máximos. |
| Valor mínimo | El número más bajo dentro del intervalo aceptable. |
| Valor máximo | El número más alto dentro del intervalo aceptable. |
| Unidad      | La unidad de medida del sensor, como °F o km/h. |

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido cómo configurar un modelo de dispositivo personalizado para una simulación. A continuación, puede desear explorar algunas de las otras [aceleradores de soluciones de IoT](about-iot-accelerators.md).