---
title: Visualizar los datos de telemetría de supervisión remota con Azure Time Series Insights | Microsoft Docs
description: Obtenga información sobre cómo configurar el entorno de Time Series Insights para explorar y analizar los datos de telemetría que genere el acelerador de soluciones de supervisión remota.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 02b3afbc62be7a7a9c396de888378d762405248a
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382929"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-remote-monitoring-solution-accelerator"></a>Use Time Series Insights para visualizar los datos de telemetría que se envíen desde el acelerador de soluciones de supervisión remota

Como operador, es posible que quiera ampliar las capacidades que ofrece la visualización de datos integrada del acelerador de soluciones de supervisión remota. En esta guía se muestra cómo usar un entorno de Time Series Insights para visualizar y analizar datos de telemetría enviados al acelerador de soluciones de supervisión remota.

## <a name="prerequisites"></a>Requisitos previos

Recuerde que para completar los pasos de esta guía paso a paso, necesita una suscripción activa a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

En los pasos descritos en esta guía práctica se supone que ya ha implementado el acelerador de soluciones de supervisión remota en su suscripción de Azure. Si no ha implementado el acelerador de soluciones, siga los pasos descritos en la guía de inicio rápido [Implementación y ejecución de una solución de supervisión remota basada en la nube](quickstart-remote-monitoring-deploy.md).

En este artículo se da por supuesto que el nombre del acelerador de soluciones es **contoso-simulation**. Reemplace **contoso-simulation** con el nombre de su acelerador de soluciones a medida que completa los pasos siguientes.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-accelerators-create-tsi.md](../../includes/iot-accelerators-create-tsi.md)]

## <a name="time-series-insights-explorer"></a>Explorador de Time Series Insights

El explorador de Time Series Insights es una aplicación web que puede usar para visualizar los datos de telemetría.

1. En Azure Portal, la pestaña **Información general** de Time Series Insights.

1. Haga clic en **Go To Environment** (Ir al entorno), que abrirá la aplicación web del explorador de Time Series Insights.

    ![Explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-environment.png)

1. En el panel de selección de tiempo, seleccione **Últimos 30 minutos** en el menú rápido de tiempo, y haga clic en **Buscar**.

    ![Búsqueda en el explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-search-time.png)

1. En el panel de términos de la izquierda, seleccione **temperatura** como **Medida** e **iothub-connection-device-id** como el valor **Dividir por**:

    ![Consulta en el explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query1.png)

1. Haga clic con el botón derecho en el gráfico y seleccione **Explorar eventos**:

    ![Eventos del explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explore-events.png)

1. Los datos del evento se muestran en una cuadrícula:

    ![Tabla del explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-table.png)

1. Haga clic en el botón de la vista de perspectiva.

    ![Perspectiva del explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explorer-perspective.png)

1. Haga clic en **+** para agregar una nueva consulta a la perspectiva:

    ![Consulta para agregar en el explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-new-query.png)

1. Seleccione **últimos 30 minutos** como intervalo de tiempo, **Humedad** como **Medida**, y **iothub-connection-device-id** como el valor de **Dividir por**:

    ![Consulta en el explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query2.png)

1. Haga clic en el botón de vista de perspectiva para ver el panel de datos de telemetría del dispositivo.

    ![Panel del explorador de Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-dashboard.png)

[!INCLUDE [iot-accelerators-cleanup-tsi.md](../../includes/iot-accelerators-cleanup-tsi.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo explorar y consultar datos en el explorador de Time Series Insights, vea [Explorador de Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
