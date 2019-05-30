---
title: Explorar los datos mediante el Explorador de Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo usar el explorador de Azure Time Series Insights en el explorador web para tener rápidamente una visión global de los macrodatos y validar el entorno de IoT.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: cbb6d75b2ed6b73ce7e3421596520f6a3f1bf3b1
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399818"
---
# <a name="azure-time-series-insights-explorer"></a>Explorador de Azure Time Series Insights

En este artículo se describen las características y opciones de disponibilidad general de Azure Time Series Insights [aplicación web del explorador](https://insights.timeseries.azure.com/). El Explorador de Time Series Insights demuestra las capacidades de visualización de datos eficaz proporcionadas por el servicio y se puede acceder dentro de su propio entorno.

Azure Time Series Insights es un servicio de análisis, almacenamiento y administración totalmente administrado que facilita el análisis y la exploración de miles de millones de eventos de IoT de forma simultánea. Ofrece una visión global de los datos que permite validar rápidamente la solución de IoT y evitar el costoso tiempo de inactividad de los dispositivos críticos. Puede detectar las tendencias ocultas, descubrir anomalías y realizar análisis de causa principal casi en tiempo real. El explorador de Time Series Insights está actualmente en versión preliminar pública.

> [!TIP]
> Para obtener una visita guiada a través del entorno de demostración, lea el [inicio rápido de Azure Time Series Insights](time-series-quickstart.md).

## <a name="video"></a>Vídeo

### <a name="learn-about-querying-data-by-using-the-time-series-insights-explorer-br"></a>Obtenga información sobre cómo consultar datos con el Explorador de Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Vea el vídeo anterior <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">"Introducción a Time Series Insights mediante el uso de un acelerador de solución de IoT de Azure."</a>

## <a name="prerequisites"></a>Requisitos previos

Antes de que pueda usar el explorador de Time Series Insights, debe:

- Cree el entorno de Time Series Insights. Para obtener más información, consulte [cómo empezar a trabajar con Time Series Insights](./time-series-insights-get-started.md).
- [Proporcionar acceso](time-series-insights-data-access.md) a su cuenta en el entorno.
- Agregar un [IoT hub](time-series-insights-how-to-add-an-event-source-iothub.md) o [centro de eventos](time-series-insights-how-to-add-an-event-source-eventhub.md) origen del evento a ella.

## <a name="explore-and-query-data"></a>Explorar y consultar los datos

Minutos después de conectar el origen del evento al entorno de Times Series Insights, puede explorar y consultar los datos de serie temporal.

1. Para comenzar, abra el [Explorador de Time Series Insights](https://insights.timeseries.azure.com/) en el explorador web. En el lado izquierdo de la ventana, seleccione un entorno. Todos los entornos a los que tiene acceso se muestran en orden alfabético.

1. Después de seleccionar un entorno, o bien usar la **desde** y **a** configuraciones en la parte superior, o haga clic y arrastre sobre el intervalo de tiempo que desee. Seleccione el icono de lupa en la esquina superior derecha, o haga doble clic en el intervalo de tiempo seleccionado y seleccione **búsqueda**.

1. También puede actualizar automáticamente la disponibilidad cada minuto seleccionando el **Auto On** botón. El **Auto On** botón solo se aplica al gráfico de disponibilidad, no el contenido de la visualización principal.

1. El icono de nube de Azure le lleva a su entorno en el portal de Azure.

   [![Entorno de Time Series Insights](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. A continuación, verá un gráfico que muestra un recuento de todos los eventos durante el intervalo de tiempo seleccionado. A continuación tiene varios controles:

    - **Panel del Editor de términos**: el espacio para los términos es donde consulta al entorno. Se encuentra en el lado izquierdo de la pantalla:
      - **Medida**: Esta lista desplegable muestra todas las columnas numéricas (**dobles**).
      - **Split by**: Esta lista desplegable muestra las columnas de categorías (**cadenas**).
      - Puede habilitar la interpolación escalonada, mostrar mínimo y máximo y ajustar el eje y desde el panel de control junto a **medida**. También puede ajustar si los datos que se muestra un recuento, Media o suma de los datos.
      - Puede agregar hasta cinco términos para verlos en el mismo eje x. Use la **Copy-down** botón para agregar un término adicional, o bien seleccione **agregar** para agregar un término nuevo.

        [![Panel del Editor de términos](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Predicado**: Utilice el predicado para filtrar rápidamente los eventos con el conjunto de operados que aparecen en la tabla siguiente. Si lleva a cabo una búsqueda seleccionando o al hacer clic en, el predicado automáticamente las actualizaciones según esa búsqueda. Los tipos de operando compatibles incluyen:

         |Operación  |Tipos admitidos  |Notas  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Double, DateTime, TimeSpan       |         |
         |`=`, `!=`, `<>`     | String, Bool, Double, DateTime, TimeSpan, NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan, NULL        |  Todos los operandos deben ser del mismo tipo o deben ser una constante NULL.        |
         |HAS     | String        |  Literales de cadena constante solo se permiten en el lado derecho. No se permiten cadenas vacías ni NULL.       |

      - **Ejemplos de consultas**

         [![Consultas de ejemplo](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. Puede usar el **tamaño de intervalo** herramienta de control deslizante de zoom dentro y fuera de los intervalos en el mismo intervalo de tiempo. El control deslizante proporciona un control más preciso del movimiento entre grandes intervalos de tiempo que muestran tendencias uniformes hasta en intervalos tan pequeños como un milisegundo, que le permiten ver cortes granulares y de alta resolución de los datos. Predeterminado del control deslizante punto de inicio se establece como la vista más óptima de los datos de la selección para equilibrar la resolución, velocidad de la consulta y granularidad.

1. El **tiempo pincel** herramienta facilita la navegar de un intervalo de tiempo a otra.

1. Use la **guardar** comando para guardar la consulta actual y compartirlo con otros usuarios del entorno. Cuando usas **abierto**, puede ver todas las consultas guardadas y cualquier consulta compartida de otros usuarios en entornos que tiene acceso.

   [![Consultas](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Visualización de datos

1. Use la **vista en perspectiva** herramienta para una vista simultánea de hasta cuatro consultas únicas. El **vista en perspectiva** botón está en la esquina superior derecha del gráfico.

   [![Vista de perspectiva](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. Ver un gráfico para explorar los datos visualmente y usar el **gráfico** herramientas:

    - **Seleccione** o **haga clic en** un intervalo de tiempo específico o una serie de datos.
    - Dentro de una selección de intervalo de tiempo, puede acercar o explorar los eventos.
    - Dentro de una serie de datos, puede dividir la serie por otra columna, agregar la serie como término nuevo, mostrar solo la serie seleccionada, excluir la serie seleccionada, anclar esa serie o explorar los eventos de la serie seleccionada.
    - En el área de filtro a la izquierda del gráfico, puede ver todas las series de datos mostrados y reordenar por nombre o valor. También puede ver todas las series de datos o cualquier series ancladas o desancladas. Puede seleccionar una serie de datos única y dividirla por otra columna, agregar la serie como término nuevo, mostrar solo la serie seleccionada, excluir la serie seleccionada, anclar esa serie o explorar los eventos de la serie seleccionada.
    - Cuando ve varios términos al mismo tiempo, la pila, desapilarlos, ver datos adicionales sobre una serie de datos y usar el mismo eje y en todos los términos. Utilice los botones en la esquina superior derecha del gráfico.

    [![Herramientas de gráfico](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. Use la **mapa térmico** detectar rápidamente la serie de datos única o anómala en una consulta determinada. Solo un término de búsqueda se puede visualizar como un mapa térmico.

    [![Mapa térmico](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. Al explorar eventos seleccionando o con el botón secundario, el **eventos** panel está disponible. En el panel puede ver todos los eventos sin procesar y exportar los eventos como archivos JSON o CSV. Time Series Insights almacena todos los datos sin procesar.

    [![Eventos](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Seleccione el **estadísticas** pestaña después de explorar los eventos para exponer los patrones y estadísticas de columna.

    - **Patrones**: Esta característica muestra de manera proactiva los patrones de mayor importancia estadística de una región de datos seleccionada. No tiene que buscar en miles de eventos para comprender cuáles son los patrones que requieren más tiempo y energía. Con Time Series Insights, puede ir directamente a estos patrones con importancia estadística para seguir realizando el análisis. Esta característica también resulta útil para las investigaciones de análisis final de datos históricos.
    - **Estadísticas de columna**: Estadísticas de columna proporcionan gráficos y tablas que desglosan los datos de cada columna de la serie de datos seleccionada durante el intervalo de tiempo seleccionado.

      [![STATS](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

Ahora ha visto las distintas características y opciones que están disponibles dentro de la aplicación web de explorador de Time Series Insights.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [diagnosticar y solucionar problemas](time-series-insights-diagnose-and-solve-problems.md) en su entorno de Time Series Insights.
- Tome el guiada [inicio rápido de Azure Time Series Insights](time-series-quickstart.md) paseo.
