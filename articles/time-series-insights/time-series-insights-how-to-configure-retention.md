---
title: Configuración de la retención en el entorno de Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo configurar la retención en el entorno de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: ff4d326af691ae27894dc94d7581ba68951f090e
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990051"
---
# <a name="configuring-retention-in-time-series-insights"></a>Configuración de la retención en Time Series Insights

En este artículo se describe cómo configurar el **Tiempo de retención de datos** y el **Comportamiento correspondiente a un exceso del límite de almacenamiento** en Azure Time Series Insights.

## <a name="summary"></a>Resumen

Cada entorno de Azure Time Series Insights tiene una opción para configurar el **tiempo de retención de los datos**. El valor abarca de 1 a 400 días. Los datos se eliminan según la capacidad de almacenamiento del entorno o la duración de la retención (1-400), lo que se agote antes.

Cada entorno de Time Series Insights tiene una opción adicional de **Comportamiento correspondiente a un exceso del límite de almacenamiento**. Esta configuración controla el comportamiento de entrada y de purga cuando se alcanza la capacidad máxima de un entorno. Hay dos comportamientos para elegir:

- **Purgar datos antiguos** (valor predeterminado)
- **Pausar entradas**

Para obtener información detallada para comprender mejor estas opciones de configuración, consulte [Understanding retention in Time Series Insights](time-series-insights-concepts-retention.md) (Información sobre la retención en Time Series Insights).  

## <a name="configure-data-retention"></a>Configuración de la retención de datos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Busque su entorno de Time Series Insights existente. Seleccione **Todos los recursos** en el menú izquierdo de Azure Portal. Seleccione el entorno de Time Series Insights.

1. En el encabezado **Configuración**, haga clic en **Configurar**.

    [![Selección de Configuración y, luego, Configurar](media/data-retention/1-configure-data-retention.png)](media/data-retention/1-configure-data-retention.png#lightbox)

1. Seleccione el **tiempo de retención de los datos (en días)** para configurar el período de retención; use para ello la barra de control deslizante o escriba un número en el cuadro de texto.

1. Tenga en cuenta la opción de configuración **Capacidad**, ya que este valor afecta a la cantidad máxima de eventos de datos y la capacidad de almacenamiento total para almacenar los datos.

1. Alterne la opción de configuración **Comportamiento correspondiente a un exceso del límite de almacenamiento**. Seleccione el comportamiento **Purgar datos antiguos** o **Pausar entradas**.

    [![Retención de datos: aceptar y guardar.](media/data-retention/2-accept-and-save.png)](media/data-retention/2-accept-and-save.png#lightbox)

1. Acepte la casilla que indica que ha revisado la documentación y que comprende los posibles riesgos de pérdida de datos. Seleccione **Guardar** para configurar los cambios.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información, consulte [Understanding retention in Time Series Insights](time-series-insights-concepts-retention.md) (Información sobre la retención en Time Series Insights).

- Conozca [cómo escalar su entorno de Time Series Insights](time-series-insights-how-to-scale-your-environment.md).

- Aprenda sobre el [planeamiento de su entorno ](time-series-insights-environment-planning.md).