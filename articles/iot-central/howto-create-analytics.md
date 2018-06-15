---
title: Creación de análisis personalizados para la aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, aprenderá a crear un análisis personalizado para su aplicación de Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 5facdf3f02b71e154a23d8f26c7bcc40b5c71e35
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629310"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cómo usar análisis para analizar los datos del dispositivo

Microsoft Azure IoT Central proporciona completas funcionalidades de análisis para dotar de sentido a las enormes cantidades de datos de sus dispositivos. Puede usar análisis para ver y analizar datos de un [conjunto de dispositivos](howto-use-device-sets.md) de su aplicación. Un conjunto de dispositivos es un grupo de dispositivos definido por el usuario. Puede restringir el análisis a un pequeño conjunto de dispositivos o a un único dispositivo.

Como operador, elija **Analytics** en el menú de navegación izquierdo, elija un conjunto de dispositivos y, a continuación, elija las medidas que desea mostrar en el gráfico. Estas son algunas herramientas que puede usar para segmentar los datos aún más:

* **Measurements (Medidas):** elija las mediciones que se mostrarán, como temperatura y humedad.
* **Aggregation (Agregación):** elija la función de agregación de las mediciones. Por ejemplo, **Minimum** (Mínimo) o **Average** (Media).
* **Split-by (Dividir por):** explore en profundidad los datos dividiéndolos por las propiedades del dispositivo o el nombre del dispositivo. Por ejemplo, divida por ubicación del dispositivo:

     ![Página principal de Analytics](media\howto-create-analytics\analytics-main.png)

* **Time-range (Intervalo de tiempo):** puede elegir un intervalo de tiempo de los ya predefinidos o crear uno personalizado: ![Intervalo de tiempo de Analytics](media\howto-create-analytics\analytics-time-range.png)

## <a name="change-the-visualizations"></a>Cambio de las visualizaciones

Dispone de tres modos entre los que elegir para cambiar los gráficos y adaptarlos a sus requisitos de visualización:

* **Stacked (Apilados):** se apila un gráfico de cada medida y cada uno de ellos tiene su propio eje Y. Los gráficos apilados son útiles cuando tiene varias medidas seleccionadas y desea tener una vista distinta de estas medidas.
* **Unstacked (No apilados):** se traza un gráfico de cada medida con respecto a un eje Y, pero los valores del eje Y se cambian en función de la medida resaltada. Los gráficos no apilados son útiles cuando se quieren superponer varias medidas y se quieren ver los patrones entre estas medidas para el mismo intervalo de tiempo.
* **Shared Y-axis (Eje Y compartido):** todos los gráficos comparten el mismo eje Y y los valores del eje no cambian. Los gráficos de eje Y compartido son útiles cuando se quiere examinar una única medida mientras se dividen los datos con Split-by (Dividir por).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear análisis personalizado para su aplicación de Azure IoT Central, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Preparación y conexión de una aplicación de Node.js](howto-connect-nodejs.md)