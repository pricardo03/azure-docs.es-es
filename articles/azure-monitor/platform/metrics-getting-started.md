---
title: Introducción al explorador de métricas de Azure
description: Aprenda a crear su primer gráfico de métricas con el Explorador de métricas de Azure.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: e611f1564896cfdecb3ce34ab7c5361e5200b48a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537344"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Introducción al explorador de métricas de Azure

## <a name="where-do-i-start"></a>¿Por dónde comenzar
El explorador de métricas de Azure Monitor es un componente de Microsoft Azure Portal que permite trazar los gráficos, correlacionar visualmente las tendencias e investigar crestas y valles en valores de las métricas. Utilice el Explorador de métricas para investigar el estado y la utilización de los recursos. Iniciar en el orden siguiente:

1. [Seleccione un recurso y una métrica](#create-your-first-metric-chart) y verá un gráfico básico. A continuación, [seleccionar un intervalo de tiempo](#select-a-time-range) que es relevante para la investigación.

1. Pruebe [aplicar los filtros de dimensión y la división de](#apply-dimension-filters-and-splitting). Los filtros y le permiten analizar qué segmentos de la métrica que contribuyen al valor general métrico e identificar posibles valores atípicos de la división.

1. Use [configuración avanzada](#advanced-chart-settings-and-next-steps) para personalizar el gráfico antes de anclar a paneles. [Configurar alertas](alerts-metric-overview.md) para recibir notificaciones cuando el valor de métrica supera o cae por debajo del umbral.

## <a name="create-your-first-metric-chart"></a>Crear su primer gráfico de métricas

Para crear un gráfico de métricas desde su recurso, grupo de recursos, suscripción o vista de Azure Monitor, abra el **métricas** pestaña y siga estos pasos:

1. Con el selector de recursos, seleccione el recurso para el que desea ver las métricas. (El recurso se preselecciona si abrió **métricas** en el contexto de un recurso específico).

    > ![Selección de un recurso](./media/metrics-getting-started/resource-picker.png)

2. Para algunos recursos, debe elegir un espacio de nombres. El espacio de nombres es una manera de organizar las métricas para que se puede encontrar fácilmente. Por ejemplo, las cuentas de almacenamiento tienen espacios de nombres independientes para almacenar las métricas de los archivos, tablas, Blobs y colas. Muchos tipos de recursos solo tienen un espacio de nombres.

3. Seleccione una métrica de una lista de métricas disponibles.

    > ![Seleccione una métrica](./media/metrics-getting-started/metric-picker.png)

4. Si lo desea, puede cambiar la agregación de métricas. Por ejemplo, es posible que desee el gráfico para mostrar los valores mínimos, máximos o promedio de la métrica.

> [!NOTE]
> Use la **agregar métrica** botón y repita estos pasos si desea ver varias métricas trazadas en el mismo gráfico. Para varios gráficos en una vista, seleccione el **Agregar gráfico** botón en la parte superior.

## <a name="select-a-time-range"></a>Selección de un intervalo de tiempo

De forma predeterminada, el gráfico muestra las últimas 24 horas de datos de métricas. Use la **selector de hora** panel para cambiar el intervalo de tiempo, acercar o alejar en el gráfico. 

![Panel de intervalo de tiempo de cambio](./media/metrics-getting-started/time-picker.png)

## <a name="apply-dimension-filters-and-splitting"></a>Aplicar filtros de dimensión y dividir

[Filtrado](metrics-charts.md#apply-filters-to-charts) y [dividir](metrics-charts.md#apply-splitting-to-a-chart) son eficaces herramientas de diagnóstico para las métricas con dimensiones. Estas características mostrar cómo varias impacto de segmentos de métrica ("valores de dimensión") el valor total de la métrica y le permiten identificar a valores atípicos posibles.

- **Filtrado** permite elegir qué valores de dimensión se incluyen en el gráfico. Por ejemplo, desea mostrar las solicitudes correctas al representar gráficamente el *tiempo de respuesta del servidor* métrica. Deberá aplicar el filtro en el *éxito de solicitud* dimensión. 

- **Dividir** controles si el gráfico muestra independiente para cada valor de una dimensión de líneas o agrega los valores en una sola línea. Por ejemplo, puede ver una línea para un tiempo de respuesta promedio entre todas las instancias de servidor o vea líneas independientes para cada servidor. Deberá aplicar la división en el *instancia del servidor* dimensión para ver las líneas independientes.

Consulte [ejemplos de los gráficos](metric-chart-samples.md) que han filtrado y la división aplicado. El artículo muestra que los pasos utilizados para configurar los gráficos.

## <a name="advanced-chart-settings-and-next-steps"></a>Configuración avanzada de los gráficos y los próximos pasos

Puede personalizar el estilo del gráfico, título y modificar la configuración avanzada de los gráficos. Cuando haya terminado con la personalización, puede anclar al panel para guardar el trabajo. También puede configurar alertas de métricas. Siga [documentación del producto](metrics-charts.md) obtener información sobre estas y otras características avanzadas del explorador de métricas de Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes

* [Ver una lista de métricas disponibles para servicios de Azure](metrics-supported.md)
* [Obtenga información sobre las características avanzadas del explorador de métricas](metrics-charts.md)
* [Vea los ejemplos de gráficos configurados](metric-chart-samples.md)
