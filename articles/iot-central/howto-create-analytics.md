---
title: Análisis de los datos del dispositivo en la aplicación de Azure IoT Central | Microsoft Docs
description: Analice los datos del dispositivo en la aplicación de Azure IoT Central.
author: lmasieri
ms.author: lmasieri
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: ffe8b350c1b5cea23aeb65092c7912c6d6c1ed89
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052955"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Cómo usar análisis para analizar los datos del dispositivo

*Este artículo se aplica a los administradores, operadores y compiladores.*

Azure IoT Central proporciona completas funcionalidades de análisis para dotar de sentido a las enormes cantidades de datos de sus dispositivos. Para comenzar, visite **Analytics** en el menú de navegación izquierdo.

## <a name="querying-your-data"></a>Consulta de los datos

Debe elegir un **conjunto de dispositivos**, agregue un **filtro** (opcional) y seleccione un **período de tiempo** para empezar a trabajar. Cuando haya terminado, seleccione **Mostrar resultados** para comenzar a visualizar los datos.

* **Conjuntos de dispositivos**: un [conjunto de dispositivos](howto-use-device-sets.md) es un grupo de dispositivos definido por el usuario. Por ejemplo, todos los refrigeradores en Oakland o todas las turbinas eólicas rev 2.0.

* **Filtros**: también puede agregar filtros a la búsqueda para centrarse en los datos. Puede agregar hasta 10 filtros a la vez. Por ejemplo, en todos los refrigeradores en Oakland, busque los que han tenido una temperatura superior a 60 grados.
* **Período de tiempo**: de forma predeterminada, se podrán recuperar datos de los últimos 10 minutos. Puede cambiar este valor a uno de los intervalos de tiempo predefinidos o seleccionar un período de tiempo personalizado.

  ![Consulta de Analytics](media/howto-create-analytics/analytics-query.png)

## <a name="visualizing-your-data"></a>Visualización de los datos

Cuando haya consultado los datos, podrá comenzar a visualizarlos. Se pueden mostrar u ocultar las medidas, cambiar la forma en que se agregan los datos y dividirlos por diferentes propiedades del dispositivo.  

* **Dividir por**: la división de datos por propiedades del dispositivo le permite explorar en profundidad los datos. Por ejemplo, puede dividir los resultados por identificador de dispositivo o por ubicación.

* **Medidas**: puede mostrar u ocultar hasta 10 elementos de telemetría distintos que notifiquen los dispositivos a la vez. Las medidas corresponden a elementos tales como la temperatura y la humedad.

* **Agregación**: de forma predeterminada, los datos se agregan por promedio, pero puede cambiar el criterio de agregación de datos por otro que se ajuste sus necesidades.

   ![Visualización de Analytics dividido por](media/howto-create-analytics/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Interacción con los datos

Dispone de varias formas de modificar los resultados de la consulta para satisfacer sus necesidades de visualización. Puede alternar entre una vista de gráfico y una vista de cuadrícula, acercar o alejar la vista, actualizar el conjunto de datos y modificar la forma en que se muestran las líneas.

* **Mostrar cuadrícula**: los resultados están disponibles en formato de tabla para permitirle ver el valor específico para cada punto de datos. Esta vista también cumple los estándares de seguridad.
* **Mostrar gráfico**: los resultados se muestran en un formato de línea que le ayudará a detectar fácilmente tendencias ascendentes/descendentes y anomalías.

  ![Presentación de la vista de cuadrícula para los análisis](media/howto-create-analytics/analytics-showgrid.png)

El zoom le permite dirigirse a sus datos. Si encuentra un período de tiempo en el que quiere centrarse dentro del conjunto de resultados, use el cursor para seleccionar el área que desea ampliar y utilice los controles disponibles para realizar una de las siguientes acciones:

* **Acercar**: cuando haya seleccionado un período de tiempo, este control que permite ampliar los datos se habilitará.
* **Alejar**: este control le permite alejarse un nivel a partir del último zoom. Por ejemplo, si ha acercado tres veces los datos, este control le alejará paso a paso.
* **Restablecer zoom**: cuando haya ejecutado varios niveles de zoom, puede usar el control de restablecimiento de zoom para volver al conjunto de resultados original.

  ![Ejecución del zoom en los datos](media/howto-create-analytics/analytics-zoom.png)

Puede cambiar el estilo de línea para ajustarse sus necesidades. Tiene cuatro opciones:

* **Línea**: línea recta entre los puntos de datos.
* **Suave**: línea curva entre los puntos.
* **Paso**: la línea entre los puntos del gráfico es un paso.
* **Dispersión**: se trazarán todos los puntos del gráfico sin líneas que los conecten.

  ![Distintos tipos de línea disponibles en Analytics](media/howto-create-analytics/analytics-linetypes.png)

Por último, puede organizar los datos en el eje Y eligiendo uno de los tres modos:

* **Apilado**: se apila un gráfico de cada medida y cada gráfico tiene su propio eje Y. Los gráficos apilados son útiles cuando tiene varias medidas seleccionadas y desea tener una vista distinta de estas medidas.
* **No apilado**: se traza un gráfico de cada medida con respecto a un eje Y, pero los valores del eje Y se cambian en función de la medida resaltada. Los gráficos no apilados son útiles cuando se quieren superponer varias medidas y se quieren ver los patrones entre estas medidas para el mismo intervalo de tiempo.
* **Eje Y compartido**: todos los gráficos comparten el mismo eje Y y los valores del eje no cambian. Los gráficos de eje Y compartido son útiles cuando se quiere examinar una única medida mientras se dividen los datos con Split-by (Dividir por).

  ![Organización de datos en el eje Y con distintos modos de visualización](media/howto-create-analytics/analytics-yaxis.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear análisis personalizado para su aplicación de Azure IoT Central, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Preparación y conexión de una aplicación de Node.js](howto-connect-nodejs.md)