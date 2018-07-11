---
title: Supervisión de los dispositivos de IoT desde una solución de Azure | Microsoft Docs
description: En este tutorial aprenderá a supervisar los dispositivos de IoT mediante el acelerador de solución de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 5f42ed0fa5362959e5619f2d550ca1ae3711ed65
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097468"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Tutorial: Supervisión de los dispositivos de IoT

En este tutorial, usará el acelerador de soluciones de supervisión remota para supervisar los dispositivos de IoT conectados. Utilice el panel de soluciones para ver la telemetría, la información del dispositivo, las alertas y los KPI.

Para introducir estas características de supervisión, el tutorial utiliza dos dispositivos de camión simulados. Los camiones son administrados por una organización denominada Contoso y están conectados al acelerador de soluciones de supervisión remota. Como operador de Contoso, debe supervisar la ubicación y el comportamiento de los camiones sobre el terreno.

En este tutorial, va a:

>[!div class="checklist"]
> * Filtrar los dispositivos en el panel
> * Ver la telemetría en tiempo real
> * Ver los detalles de dispositivo
> * Ver las alertas de los dispositivos
> * Ver los KPI del sistema

## <a name="prerequisites"></a>Requisitos previos

Para seguir este tutorial, necesita una instancia implementada de la solución de supervisión remota en la suscripción de Azure.

Si aún no ha implementado el acelerador de soluciones de supervisión remota, debe completar la guía de inicio rápido [Implementación de una solución de supervisión remota basada en la nube](quickstart-remote-monitoring-deploy.md).

## <a name="choose-the-devices-to-display"></a>Elección de los dispositivos que se van a mostrar

Para seleccionar los dispositivos conectados que se muestran en la página **Panel**, use los filtros. Para mostrar solo los dispositivos **Camión**, elija el filtro **Camiones** integrado en el menú desplegable de filtros:

[![Filtro de los camiones en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Cuando aplica un filtro, solo los dispositivos que coinciden con las condiciones de filtro aparecen en el mapa de la página **Panel**:

[![Solo los camiones se muestran en el mapa](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

El filtro también determina cuáles son los dispositivos que ve en el gráfico **Telemetría**:

[![La telemetría del camión se muestra en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-expanded.png#lightbox)

Para crear, editar y eliminar filtros, elija **Manage device groups** (Administrar grupos de dispositivos).

## <a name="view-real-time-telemetry"></a>Ver la telemetría en tiempo real

El acelerador de la solución traza los datos de telemetría en tiempo real en el gráfico de la página **Panel**. La parte superior del gráfico de telemetría muestra los tipos de telemetría disponibles para los dispositivos seleccionados con el filtro actual:

[![Tipos de la telemetría del camión](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Para ver la telemetría de temperatura, haga clic en **Temperatura**:

[![Trazado de la telemetría de temperatura de los camiones](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="use-the-map"></a>Uso del mapa

El mapa muestra información sobre los camiones simulados seleccionados con el filtro actual. Puede hacer zoom y desplazar el mapa para mostrar las ubicaciones con mayor o menor detalle. El color del icono de un dispositivo que aparece en el mapa indica si hay alguna **alerta** o **advertencia** activa para el dispositivo. A la izquierda del mapa, se muestra un resumen del número de **alertas** y **advertencias**.

Para ver los detalles del dispositivo, desplácese en el mapa y haga zoom para ubicar el dispositivo y, luego, haga clic en él en el mapa. A continuación, haga clic en la etiqueta de dispositivo para abrir el panel **Detalles del dispositivo**. Estos detalles incluyen:

* Valores recientes de telemetría
* Métodos que admite el dispositivo
* Propiedades del dispositivo

[![Vista de los detalles del dispositivo en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Visualización de alertas

El panel **Alertas** muestra información detallada sobre las alertas más recientes de los dispositivos:

[![Vista de las alertas de dispositivo en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

Puede usar un filtro para ajustar el intervalo de tiempo de las alertas recientes. De manera predeterminada, el panel muestra las alertas de la última hora:

[![Filtro de las alertas por hora](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-expanded.png#lightbox)

## <a name="view-the-system-kpis"></a>Ver los KPI del sistema

La página **Panel** muestra los KPI del sistema calculados por el acelerador de soluciones en el panel **Análisis**:

[![KPI del panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

El panel muestra tres KPI para las alertas seleccionados por los filtros de dispositivo y el intervalo de tiempo actuales:

* El número de alertas activas para las reglas que han desencadenado la mayoría de las alertas.
* La proporción de alertas por tipo de dispositivo.
* El porcentaje de alertas que son críticas.

Los mismos filtros que establecen el intervalo de tiempo para las alertas y controlan qué dispositivos se muestran, determinan cómo se agregan los KPI. De manera predeterminada, el panel muestra los KPI agregados durante la última hora.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si va a pasar al tutorial siguiente, deje el acelerador de soluciones de supervisión remota implementado. Para reducir los costos de la ejecución del acelerador de soluciones mientras no lo esté usando, puede detener los dispositivos simulados en el panel de configuración:

[![Pausa de la telemetría](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-expanded.png#lightbox)

Puede reiniciar los dispositivos simulados cuando esté preparado para comenzar el tutorial siguiente.

Si ya no necesita el acelerador de soluciones, elimínelo de la página [Soluciones aprovisionadas](https://www.azureiotsolutions.com/Accelerators#dashboard):

![Eliminación de la solución](media/iot-accelerators-remote-monitoring-monitor/deletesolution.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha demostrado cómo usar la página **Panel** en el acelerador de soluciones de supervisión remota para filtrar y supervisar los camiones simulados. Para obtener información sobre cómo usar el acelerador de soluciones para detectar problemas con los dispositivos conectados, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Detección de problemas de dispositivos conectados a la solución de supervisión](iot-accelerators-remote-monitoring-automate.md)