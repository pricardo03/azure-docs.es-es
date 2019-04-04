---
title: Explorador de métricas de Azure Monitor
description: Descubra las nuevas características en el explorador de métricas de Azure Monitor
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 08ae74bcd9ee0a7cf5e0fb6d38758b1429c39145
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916349"
---
# <a name="azure-monitor-metrics-explorer"></a>Explorador de métricas de Azure Monitor

El explorador de métricas de Azure Monitor es un componente de Microsoft Azure Portal que permite trazar los gráficos, correlacionar visualmente las tendencias e investigar crestas y valles en valores de las métricas. El explorador de métricas es un punto de partida esencial para investigar diversos problemas de rendimiento y disponibilidad en las aplicaciones e infraestructura hospedadas en Azure o supervisadas por servicios de Azure Monitor.

## <a name="metrics-in-azure"></a>Métricas en Azure

Las [métricas en Azure Monitor](data-platform-metrics.md) son la serie de valores medidos y recuentos que se recopilan y se almacenan con el tiempo. Hay métricas estándar (o de la “plataforma”) y métricas personalizadas. La misma plataforma de Azure le proporciona las métricas estándares. Las métricas estándares reflejan las estadísticas de uso y mantenimiento de los recursos de Azure. Mientras que las métricas personalizadas se envían a Azure desde las aplicaciones mediante el [API de Application Insights para eventos y métricas personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), [extensión de Windows Azure Diagnostics (WAD)](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview), o por [Azure Supervisar la API de REST](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api).

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
   > Normalmente no querrá tener en un gráfico métricas con unidades de medida diferentes (es decir, milisegundos y kilobytes) ni con una escala muy diferente. En su lugar, considere el uso de varios gráficos. Haga clic en el botón Agregar gráfico para crear varios gráficos en el explorador de métricas.

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

## <a name="multiple-metrics-and-charts"></a>Varias métricas y gráficos

También puede crear gráficos que trazar varias métricas o mostrar varios gráficos de métricas a la vez. Esta funcionalidad le permite:

- poner en correlación las métricas relacionadas en el mismo gráfico para ver cómo una valor está relacionada con otra
- mostrar las métricas con las diferentes unidades de medida cerca
- Agregar visualmente y comparar las métricas de varios recursos

Por ejemplo, si tiene 5 cuentas de almacenamiento y desea saber la cantidad total de espacio se consume entre ellos, puede crear un gráfico de áreas (apiladas) que muestra la persona y la suma de todos los valores en momentos concretos de tiempo.

### <a name="multiple-metrics-on-a-chart"></a>Varias métricas en un gráfico

Primero, [cree un nuevo gráfico](#create-a-new-chart). Haga clic en **agregar métrica** y repita los pasos para agregar otra métrica en el mismo gráfico.

### <a name="multiple-charts"></a>Varios gráficos

Haga clic en el **Agregar gráfico** y cree otro gráfico con una métrica diferente.

### <a name="order-or-delete-multiple-charts"></a>El orden o eliminar varios gráficos

Para ordenar o eliminar varios gráficos, haga clic en el botón de puntos suspensivos ( **...**  ) símbolos para abrir el menú de gráfico y elija el elemento de menú adecuado **Subir**, **Bajar**, o **eliminar**.

## <a name="apply-splitting-to-a-chart"></a>Se aplican a un gráfico de la división

Puede dividir una métrica por dimensión para visualizar cómo se comparan entre sí los distintos segmentos de la métrica, y para identificar los segmentos no relevantes de una dimensión.

### <a name="apply-splitting"></a>Aplicar división

1. Haga clic en **Aplicar separación** encima del gráfico.
 
   > [!NOTE]
   > La división no se puede usar con los gráficos que tienen varias métricas. Además, puede tener varios filtros, pero solamente una dimensión de división aplicado a un único gráfico.

2. Elija una dimensión por la que quiera segmentar el gráfico:

   ![imagen de métrica](./media/metrics-charts/00010.png)

   Ahora el gráfico muestra varias líneas, una para cada segmento de dimensión:

   ![imagen de métrica](./media/metrics-charts/00012.png)

3. Haga clic fuera del **Selector de agrupación** para cerrarlo.

   > [!NOTE]
   > Use el filtrado y la separación en la misma dimensión para ocultar los segmentos que no son pertinentes para su escenario y facilitar la lectura de los gráficos.

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

## <a name="create-alert-rules"></a>Creación de reglas de alerta

También puede usar los criterios que ha establecido para visualizar las métricas como la base de una regla de alerta basada en la métrica. La nueva regla de alerta incluirá su recurso de destino, métrica, división y dimensiones de filtro del gráfico. Podrá modificar esta configuración más adelante en el panel de creación de la regla de alerta.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Para crear una nueva regla de alertas, haga clic en **Nueva regla de alertas**.

![Botón de nueva regla de alertas resaltado en rojo](./media/metrics-charts/015.png)

Se le dirigirá al panel de creación de reglas de alertas con las dimensiones de métricas subyacentes desde el gráfico rellenado previamente para que resulte más fácil generar reglas de alertas personalizadas.

![Crear regla de alertas](./media/metrics-charts/016.png)

Consulte este [artículo](alerts-metric.md) para obtener más información sobre cómo configurar alertas de métricas.

## <a name="troubleshooting"></a>solución de problemas

*No veo ningún dato en el gráfico.*

* Los filtros se aplican a todos los gráficos del panel. Asegúrese de que, al centrarse en un gráfico, no ha establecido un filtro que excluya todos los datos en otro.

* Si desea establecer filtros diferentes en gráficos diferentes, créelos en hojas diferentes y guárdelos como elementos favoritos independientes. Si lo desea, puede anclarlos al panel para verlos en paralelo.

* Si segmenta un gráfico por una propiedad que no está definida en la métrica, no habrá nada en el gráfico. Intente borrar la segmentación (división) o elija otro nombre de propiedad.

## <a name="next-steps"></a>Pasos siguientes

  Lea [Creating custom KPI dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) (Creación de paneles de KPI personalizados) para obtener información sobre las prácticas recomendadas para la creación de paneles accionables con métricas.

