---
title: Explorador de métricas de Azure Monitor
description: Descubra las nuevas características en el Explorador de métricas de Azure Monitor
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: vitaly.gorbenko
ms.component: metrics
ms.openlocfilehash: 8932f8aa1dd950693378e9fc58c79838fad6166b
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53741766"
---
# <a name="azure-monitor-metrics-explorer"></a>Explorador de métricas de Azure Monitor

El Explorador de métricas de Azure Monitor es un componente de Microsoft Azure Portal que permite trazar los gráficos, correlacionar visualmente las tendencias e investigar crestas y valles en valores de las métricas. El Explorador de métricas es un punto de partida esencial para investigar diversos problemas de rendimiento y disponibilidad en las aplicaciones e infraestructura hospedadas en Azure o supervisadas por servicios de Azure Monitor.

## <a name="metrics-in-azure"></a>Métricas en Azure

Las métricas en Microsoft Azure son la serie de valores medidos y recuentos que se recopilan y se almacenan con el tiempo. Hay métricas estándar (o de la “plataforma”) y métricas personalizadas. La misma plataforma de Azure le proporciona las métricas estándares. Las métricas estándares reflejan las estadísticas de uso y mantenimiento de los recursos de Azure. Mientras que las métricas personalizadas se envían a Azure desde las aplicaciones mediante la [API de Application Insights para los eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Las métricas personalizadas se almacenan en los recursos de Application Insights junto con otras métricas específicas de las aplicaciones.

## <a name="create-a-new-chart"></a>Creación de un canal

1. Abra Azure Portal.
2. Vaya a la nueva pestaña **Monitor** y seleccione **Métricas**.

   ![Imagen de métricas](./media/metrics-charts/00001.png)

3. El **selector de métricas** se abrirá automáticamente. Elija un recurso de la lista para ver las métricas asociadas. Solo los recursos con métricas se muestran en la lista.

   ![Imagen de métricas](./media/metrics-charts/00002.png)

   > [!NOTE]
   >Si tiene más de una suscripción a Azure, el Explorador de métricas extrae los recursos de todas las suscripciones que están seleccionadas en la lista de Configuración del portal -> Filtrar por suscripciones. Para cambiar esto, haga clic en el icono de engranaje de Configuración del portal en la parte superior de la pantalla y seleccione las suscripciones que quiere utilizar.

4. Para algunos tipos de recursos (cuentas de almacenamiento y máquinas virtuales), antes de seleccionar una métrica, debe elegir un **Espacio de nombres**. Cada espacio de nombres lleva su propio conjunto de métricas que son pertinentes para este espacio de nombres únicamente, y no para otros espacios de nombres.

   Por ejemplo, cada Azure Storage tiene métricas para los subservicios “Blobs”, “Files”, “Queues” y “Tables”, que son todas partes de la cuenta de almacenamiento. Sin embargo, la métrica “Queue Message Count” se aplica naturalmente al subservicio “Queue” y no a ningún otro subservicio de la cuenta de almacenamiento.

   ![Imagen de métricas](./media/metrics-charts/00003.png)

5. Seleccione una métrica de la lista. Si conoce un nombre parcial de la métrica que desea, puede empezar a escribirlo para ver una lista filtrada de las métricas disponibles:

   ![Imagen de métricas](./media/metrics-charts/00004.png)

6. Después de seleccionar una métrica, el gráfico se representará con la agregación predeterminada de la métrica seleccionada. En este momento, puede hacer clic fuera del **selector de métricas** para cerrarlo. Como alternativa, también puede cambiar el gráfico a otra agregación. Para algunas métricas, cambiar la agregación le permite elegir qué valor desea ver en el gráfico. Por ejemplo, puede cambiar entre los valores promedio, mínimo y máximo. 

7. Al hacer clic en **Agregar métrica** y repetir los pasos del 3 al 6, puede agregar más métricas en el mismo gráfico.

   > [!NOTE]
   > Normalmente no querrá tener en un gráfico métricas con unidades de medida diferentes (es decir, milisegundos y kilobytes) ni con una escala muy diferente. En su lugar, considere el uso de varios gráficos. Haga clic en el botón Agregar gráfico para crear varios gráficos en el Explorador de métricas.

## <a name="apply-filters-to-charts"></a>Aplicación de filtros a gráficos

Puede aplicar filtros a los gráficos que muestran métricas con dimensiones. Por ejemplo, si la métrica “Transaction count” tiene una dimensión, “Response type”, que indica si la respuesta de las transacciones se realizó correctamente o no, el filtrado en esta dimensión trazaría una línea de gráfico solo para transacciones correctas (o solo con errores). 

### <a name="to-add-a-filter"></a>Para agregar un filtro

1. Seleccione **Agregar filtro** encima del gráfico

2. Seleccione qué dimensión (propiedad) quiere filtrar.

   ![imagen de métrica](./media/metrics-charts/00006.png)

3. Seleccione qué valores de dimensión quiere incluir al trazar el gráfico (en este ejemplo se muestra el filtrado excluyente de las transacciones de almacenamiento correctas):

   ![imagen de métrica](./media/metrics-charts/00007.png)

4. Después de seleccionar los valores de filtro, haga clic fuera del Selector de filtro para cerrarlo. Ahora el gráfico muestra cuántas transacciones de almacenamiento han tenido error:

   ![imagen de métrica](./media/metrics-charts/00008.png)

5. Puede repetir los pasos del 1 al 4 para aplicar varios filtros a los gráficos mismos.

## <a name="segment-a-chart"></a>Segmentación de un gráfico

Puede dividir una métrica por dimensión para visualizar cómo se comparan entre sí los distintos segmentos de la métrica, y para identificar los segmentos no relevantes de una dimensión. 

### <a name="to-segment-a-chart"></a>Para segmentar un gráfico

1. Haga clic en **Aplicar separación** encima del gráfico.
 
   > [!NOTE]
   > Puede tener varios filtros, pero solo un valor de segmentación o separación en un único gráfico.

2. Elija una dimensión por la que quiera segmentar el gráfico:

   ![imagen de métrica](./media/metrics-charts/00010.png)

   Ahora el gráfico muestra varias líneas, una para cada segmento de dimensión:

   ![imagen de métrica](./media/metrics-charts/00012.png)

3. Haga clic fuera del **Selector de agrupación** para cerrarlo.

   > [!NOTE]
   > Use el filtrado y la separación en la misma dimensión para ocultar los segmentos que no son pertinentes para su escenario y facilitar la lectura de los gráficos.

### <a name="new-alert-rule"></a>Nueva alerta de reglas

También puede usar los criterios que ha establecido para visualizar las métricas como la base de la lógica subyacente de una métrica basada en la regla de alerta. 

Si hace clic en **Nueva regla de alertas**

![Botón de nueva regla de alertas resaltado en rojo](./media/metrics-charts/015.png)

Se le dirigirá al panel de creación de reglas de alertas con las dimensiones de métricas subyacentes desde el gráfico rellenado previamente para que resulte más fácil generar reglas de alertas personalizadas.

![Crear regla de alertas](./media/metrics-charts/016.png)

Consulte este [artículo](alerts-metric.md) para obtener más información sobre cómo configurar alertas de métricas.

## <a name="lock-boundaries-of-chart-y-axis"></a>Bloqueo de límites del eje y del gráfico

El bloqueo del intervalo del eje y cobra importancia cuando el gráfico muestra fluctuaciones pequeñas de valores mayores. 

Por ejemplo, cuando el volumen de solicitudes correctas cae del 99,99 % al 99,5 %, puede representar una reducción significativa de la calidad del servicio. Sin embargo, percibir una pequeña fluctuación de un valor numérico sería difícil, o incluso imposible, desde la configuración predeterminada del gráfico. En ese caso podría bloquear el límite inferior del gráfico al 99 %, lo que haría que esta pequeña caída fuera más evidente. 

Otro ejemplo es una fluctuación en la memoria disponible, donde el valor técnicamente nunca llegará a 0. El ajuste del intervalo a un valor mayor puede facilitar las detección de caídas en la memoria disponible. 

Para controlar el intervalo del eje y, use el menú "..." del gráfico y seleccione **Editar gráfico** para acceder a la configuración avanzada del gráfico. Modifique los valores en la sección Intervalo del eje Y o utilice el botón **Auto** para volver a los valores predeterminados.

![imagen de métrica](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> El bloqueo de los límites del eje Y de los gráficos que realizan el seguimiento de varios recuentos o sumas durante un período (y, por tanto, use recuento de uso, suma y agregaciones mínima o máxima) normalmente requiere que se especifique una granularidad de tiempo fijo, en lugar de usar los valores predeterminados automáticos. Esto es necesario porque los valores de los gráficos cambian cuando la granularidad de tiempo se modifica automáticamente cuando el usuario cambia el tamaño de la ventana del explorador o pasa de una resolución de pantalla a otro. El cambio resultante en la granularidad de tiempo afecta al aspecto del gráfico, lo que invalida la selección actual del eje Y.

## <a name="pin-charts-to-dashboards"></a>Anclaje de gráficos a paneles

Después de configurar un gráfico, puede que quiera agregarlo a los paneles para poder volver a verlo, posiblemente en el contexto de otra telemetría de supervisión, o compartirlo con su equipo.

Para anclar un gráfico configurado a un panel:

Después de configurar el gráfico, haga clic en el menú **Chart Actions** (Acciones del gráfico) en la esquina superior derecha del gráfico y haga clic en **Anclar al panel**.

![imagen de métrica](./media/metrics-charts/00013.png)

## <a name="next-steps"></a>Pasos siguientes

  Lea [Creating custom KPI dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) (Creación de paneles de KPI personalizados) para obtener información sobre las prácticas recomendadas para la creación de paneles accionables con métricas.
