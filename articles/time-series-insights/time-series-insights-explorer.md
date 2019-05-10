---
title: Exploración de los datos con el explorador de Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo usar el explorador de Azure Time Series Insights en el explorador web para tener rápidamente una visión global de los macrodatos y validar el entorno de IoT.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 0f22a0245d002b94d9fc0004214c37944350e262
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412914"
---
# <a name="azure-time-series-insights-explorer"></a>Explorador de Azure Time Series Insights

En este artículo se describen las características y opciones de disponibilidad General de Azure Time Series Insights [aplicación web del explorador](https://insights.timeseries.azure.com/). El Explorador de Time Series Insights demuestra las capacidades de visualización de datos eficaz proporcionadas por el servicio y se puede acceder dentro de su propio entorno.

Azure Time Series Insights es un servicio de análisis, almacenamiento y administración totalmente administrado que facilita el análisis y la exploración de miles de millones de eventos de IoT de forma simultánea. Ofrece una visión global de los datos que permite validar rápidamente la solución de IoT y evitar el costoso tiempo de inactividad de los dispositivos críticos. Puede detectar las tendencias ocultas, descubrir anomalías y realizar análisis de causa principal casi en tiempo real. El explorador de Time Series Insights está actualmente en versión preliminar pública.

> [!TIP]
> Para obtener una visita guiada a través del entorno de demostración, lea el [inicio rápido de Azure tiempo Series Insights](time-series-quickstart.md).

## <a name="video"></a>Vídeo

### <a name="learn-about-querying-data-using-the-time-series-insights-explorer-br"></a>Obtenga información sobre cómo consultar datos mediante el Explorador de Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Vea el vídeo anterior <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">"Introducción a TSI mediante un acelerador de solución de IoT de Azure"</a>.

## <a name="prerequisites"></a>Requisitos previos

Antes de que pueda usar el explorador de Time Series Insights, debe:

- Cree el entorno de Time Series Insights. Para obtener más información, consulte [cómo empezar a trabajar con Time Series Insights](./time-series-insights-get-started.md).
- [Proporcionar acceso](time-series-insights-data-access.md) a su cuenta en el entorno.
- Agregar un [IoT Hub](time-series-insights-how-to-add-an-event-source-iothub.md) o [centro de eventos](time-series-insights-how-to-add-an-event-source-eventhub.md) origen del evento a ella.

## <a name="explore-and-query-data"></a>Explorar y consultar los datos

Minutos después de conectar el origen del evento al entorno de Times Series Insights, puede explorar y consultar los datos de serie temporal.

1. Para comenzar, abra el [explorador de Time Series Insights](https://insights.timeseries.azure.com/) en el explorador web y seleccione un entorno en el lado izquierdo de la ventana. Todos los entornos a los que tiene acceso se muestran en orden alfabético.

1. Una vez que selecciona un entorno, use las configuraciones **FROM** y **TO** que aparecen en la parte superior, o bien haga clic y arrastre hasta el intervalo de tiempo deseado.  Haga clic en la lupa que se encuentra en la parte superior derecha o haga clic con el botón derecho en el intervalo de tiempo seleccionado y seleccione **Buscar**.  

1. También puede actualizar automáticamente la disponibilidad cada minuto si selecciona el botón **Auto On** (Activado automáticamente). El **Auto On** botón solo se aplica al gráfico de disponibilidad, no el contenido de la visualización principal.

1. Observe que el icono de la nube de Azure lo dirige a su entorno en Azure Portal.

   [![Entorno de Time Series Insights](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. A continuación, verá un gráfico que muestra un recuento de todos los eventos durante el intervalo de tiempo seleccionado.  A continuación tiene varios controles:

    **Panel Editor de términos**:  el espacio para los términos es donde consulta al entorno.  Se encuentra en el lado izquierdo de la pantalla:
      - **Medida**:  Este menú desplegable muestra todas las columnas numéricas (**dobles**)
      - **Dividir por**: Este menú desplegable muestra las columnas de categorías (**cadenas**)
      - Puede habilitar la interpolación escalonada, mostrar el valor mínimo y el valor máximo, y ajustar el eje Y desde el panel de control que está junto a la medida.  Además, puede ajustar si los datos que se muestran son un recuento, un promedio o la suma de los datos.
      - Puede agregar hasta cinco términos para verlos en el mismo eje X.  Use el botón **copy-down** para agregar un término adicional o haga clic en el botón **Add** (Agregar) para agregar un término nuevo.

        [![Panel del Editor de términos](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Predicado**:  el predicado permite filtrar rápidamente los eventos con el conjunto de operandos que aparecen a continuación. Si realiza una búsqueda mediante selección o clic, el predicado se actualizará automáticamente según esa búsqueda. Los tipos de operando compatibles incluyen:

         |Operación  |Tipos admitidos  |Notas  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Double, DateTime, TimeSpan       |         |
         |`=`, `!=`, `<>`     | String, Bool, Double, DateTime, TimeSpan, NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan, NULL        |  Todos los operandos deben ser del mismo tipo o deben ser una constante NULL.        |
         |HAS     | string        |  Solo los literales de cadenas constantes se permiten en el lado derecho. No se permiten cadenas vacías ni NULL.       |

      - **Ejemplos de consultas**

         [![Consultas de ejemplo](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. La herramienta de control deslizante **Interval Size** (Tamaño del intervalo) permite acercar y alejar los intervalos del mismo intervalo de tiempo.  Esto proporciona un control más preciso del movimiento entre intervalos de tiempo de gran tamaño que muestran tendencias uniformes hasta en intervalos tan pequeños como un milisegundo, lo que permite ver cortes de los datos granulares y de alta resolución. El punto inicial predeterminado del control deslizante está establecido como la vista más óptima de los datos de la selección, lo que permite equilibrar la resolución, la velocidad de la consulta y la granularidad.

1. La herramienta **Time brush** (Pincel para tiempo) facilita la navegación de un intervalo de tiempo a otro, lo que pone la experiencia del usuario intuitiva en el centro para lograr un movimiento sin problemas entre los intervalos de tiempo.

1. El comando **Save** (Guardar) le permite guardar la consulta actual y habilitarla para su uso compartido con otros usuarios del entorno. Con **Open** (Abrir), puede ver todas las consultas guardadas y cualquier consulta compartida de otros usuarios en los entornos a los que tiene acceso.

   [![Consultas](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Visualización de datos

1. La herramienta **Perspective View** (Vista de perspectiva) proporciona una vista simultánea de hasta cuatro consultas únicas. El botón de vista de perspectiva está en la esquina superior derecha del gráfico.  

   [![Vista de perspectiva](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. **Chart** (Gráfico) le permite explorar visualmente los datos. Las herramientas del gráfico son:

    - **Seleccione y haga clic en**, que permite la selección de un intervalo de tiempo específico o de una serie de datos.  
    - Dentro de la selección de un intervalo de tiempo, puede acercar o explorar los eventos.  
    - Dentro de una serie de datos, puede dividir la serie por otra columna, agregar la serie como término nuevo, mostrar solo la serie seleccionada, excluir la serie seleccionada, anclar esa serie o explorar los eventos de la serie seleccionada.
    - En el área de filtro a la izquierda del gráfico, puede ver todas las series de datos mostrados y reordenar por nombre o valor, ver todas las series de datos o cualquier series ancladas o desancladas.  También puede seleccionar una serie de datos única y dividirla por otra columna, agregar la serie como término nuevo, mostrar solo la serie seleccionada, excluir la serie seleccionada, anclar esa serie o explorar los eventos de la serie seleccionada.
    - Cuando se ve varios términos de manera simultánea, puede apilarlos, desapilarlos, ver datos adicionales sobre una serie de datos y usar el mismo eje Y en todos los términos con los botones que aparecen en la esquina superior derecha del gráfico.

    [![Herramientas de gráfico](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. El **mapa térmico** se puede usar para detectar rápidamente una serie de datos única o anómala en una consulta determinada. Solo un término de búsqueda se puede visualizar como un mapa térmico.

    [![Mapa térmico](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. **Eventos**:  cuando decide explorar eventos al seleccionar o hacer clic con el botón derecho los elementos de arriba, aparece disponible el panel de eventos.  En el panel puede ver todos los eventos sin procesar y exportar los eventos como archivos JSON o CSV. Time Series Insights almacena todos los datos sin procesar.

    [![Eventos](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Haga clic en la pestaña **STATS** después de explorar los eventos para exponer los patrones y las estadísticas de columna.  

    - **Patterns** (Patrones): esta característica muestra de manera proactiva los patrones con mayor importancia estadística de una región de datos seleccionada. Esto evita tener que buscar en miles de eventos para comprender cuáles son los patrones que garantizan más tiempo y energía. Además, Time Series Insights permite ir directamente a estos patrones con importancia estadística para seguir realizando el análisis. Esta característica también resulta útil para las investigaciones de análisis final de datos históricos.

    - **Estadísticas de columna**:  las estadísticas de columna proporcionan gráficos y tablas que desglosan los datos de cada columna de la serie de datos seleccionada durante el intervalo de tiempo seleccionado.  

      [![STATS](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

Ya ha visto las distintas características y opciones disponibles dentro de la aplicación web del explorador de Time Series Insights.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [diagnosticar y solucionar problemas del](time-series-insights-diagnose-and-solve-problems.md) en su entorno de Time Series Insights.

- Tome el guiada [inicio rápido de Azure tiempo Series Insights](time-series-quickstart.md) paseo.
