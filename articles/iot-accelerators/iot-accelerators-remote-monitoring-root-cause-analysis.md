---
title: Realización de un análisis de causas principales en una alerta - Azure | Microsoft Docs
description: En este tutorial aprenderá a realizar un análisis de causas principales en una alerta con Azure Time Series Insights.
author: aditidugar
ms.author: adugar
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/11/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 8258c8f34b4b9a1b216d9d497dcdf7d3b8db1373
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369518"
---
# <a name="tutorial-conduct-a-root-cause-analysis-on-an-alert"></a>Tutorial: realización de un análisis de causas principales en una alerta

En este tutorial, obtendrá información sobre cómo usar el acelerador de solución de supervisión remota para diagnosticar la causa principal de una alerta. Verá que una alerta se ha activado en el panel de solución de supervisión remota y, a continuación, usará el explorador de Azure Time Series Insights para investigar la causa principal.

El tutorial utiliza dos dispositivos de camión de reparto simulados que envían telemetría de ubicación, altitud, velocidad y temperatura de la carga. Los camiones son administrados por una organización denominada Contoso y están conectados al acelerador de soluciones de supervisión remota. En calidad de operador de Contoso, deberá comprender por qué uno de los camiones (delivery-truck-02) ha registrado una alerta de temperatura baja.

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Filtrar los dispositivos en el panel
> * Ver la telemetría en tiempo real
> * Exploración de datos en el explorador de Time Series Insights
> * Realización de un análisis de causas principales
> * Creación de una nueva regla basada en los aprendizajes obtenidos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Elección de los dispositivos que se van a mostrar

Para seleccionar los dispositivos conectados que se muestran en la página **Panel**, use los filtros. Para mostrar solo los dispositivos **Camión**, elija el filtro **Camiones** integrado en el menú desplegable de filtros:

[![Filtro de los camiones en el panel](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-expanded.png#lightbox)

Cuando aplica un filtro, solo los dispositivos que coinciden con las condiciones de filtro aparecen en el mapa y en el panel de telemetría de la página **Panel**. Puede ver que hay dos camiones conectados al acelerador de soluciones, incluido **truck-02**.

## <a name="view-real-time-telemetry"></a>Ver la telemetría en tiempo real

El acelerador de la solución traza los datos de telemetría en tiempo real en el gráfico de la página **Panel**. De forma predeterminada, el gráfico muestra los datos de telemetría de altitud, que varía con el tiempo:

[![Trazado de telemetría de altitud del camión](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-expanded.png#lightbox)

Para ver la telemetría de temperatura de los camiones, haga clic en **Temperatura** en el **panel Telemetría**. Puede ver cómo ha variado la temperatura de ambos camiones en los últimos 15 minutos. También puede ver que se ha activado una alerta de temperatura baja para el camión delivery-truck-02, en el panel de alertas.

[![Panel de supervisión remota con alerta de temperatura baja](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-expanded.png#lightbox)

## <a name="explore-the-data"></a>Exploración de los datos

Para identificar la causa de la alarma de temperatura baja, abra los datos de telemetría del camión de reparto en el explorador de Time Series Insights. Haga clic en cualquiera de los vínculos para **explorar en Time Series Insights** del panel:

[![Panel de supervisión remota con vínculos a TSI resaltados](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-expanded.png#lightbox)

Cuando se inicie el explorador, verá todos los dispositivos:

[![Vista inicial del Explorador de TSI](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-expanded.png#lightbox)

Filtre los dispositivos escribiendo **delivery-truck** en el cuadro de filtro y seleccione **temperatura** como **Medida** en el panel izquierdo:

[![Temperatura del camión en el explorador de TSI](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Verá la misma vista del panel de supervisión remota, y ahora puede acercarse al período de tiempo en el que se activó la alerta:

[![Zoom del explorador de TSI](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

También puede agregar otros flujos de telemetría procedentes de los camiones. Haga clic en el botón **Agregar** situado en la esquina superior izquierda. Aparece un nuevo panel:

[![Explorador de TSI con el nuevo panel](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

En el nuevo panel, cambie el nombre de la nueva etiqueta a **Dispositivos** para que coincida con el anterior. Seleccione **altitud** como **Medida** y **iothub-connection-device-id** como valor para **Dividir por** a fin de agregar la telemetría de altitud en la vista:

[![Explorador de TSI con la temperatura y la altitud](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Diagnóstico de la alerta

Al examinar las secuencias en la vista actual, puede ver que los perfiles de altitud de los dos camiones son muy diferentes. Además, la caída de temperatura en **delivery-truck-02** ocurre cuando la camioneta alcanza un gran altitud. Le sorprende este hallazgo, puesto que los camiones están programados para tomar la misma ruta.

Para confirmar la sospecha de que los camiones tomaron diferentes rutas, agregue otro panel al panel lateral usando el botón **Agregar**. En el nuevo panel, cambie el nombre de la nueva etiqueta a **Dispositivos** para que coincida con el anterior. Seleccione **longitud** como **Medida** y **iothub-connection-device-id** como valor de **Dividir por** para agregar la telemetría de longitud a la vista. Puede constatar que los camiones tomaron rutas diferentes al consultar la diferencia entre los flujos de **longitud**:

[![Explorador de TSI con la temperatura, la altitud y la longitud](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Crear una nueva regla

Si bien las rutas de los camiones suelen estar optimizadas de antemano, es consciente de que los patrones de tráfico, el clima y otros eventos impredecibles pueden causar retrasos y dejar las decisiones de ruta de último momento a criterio de los conductores. Sin embargo, puesto que la temperatura de sus activos dentro del vehículo es fundamental, debe crear una regla adicional en la solución de supervisión remota para asegurarse de recibir una advertencia si la altitud promedio en un intervalo de 1 minuto sube por encima de 350 pies:

[![Regla de altitud establecida en la pestaña de reglas de supervisión remota](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Para obtener información sobre cómo crear y editar reglas, consulte el tutorial anterior sobre la [detección de problemas del dispositivo](iot-accelerators-remote-monitoring-automate.md).

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial le hemos mostrado cómo usar el explorador de Time Series Insights con el acelerador de solución de supervisión remota para diagnosticar la causa principal de una alerta. Para obtener información sobre cómo usar el acelerador de soluciones para identificar y corregir problemas con los dispositivos conectados, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Uso de alertas de dispositivo para identificar y corregir problemas con los dispositivos conectados a la solución de supervisión](iot-accelerators-remote-monitoring-maintain.md)
