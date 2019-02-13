---
title: Paneles y navegación en Azure Application Insights | Microsoft Docs
description: Cree vistas de los gráficos APM y consultas principales.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: afbf2bc32aa737eb5f6dde41035b206d6e260252
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813774"
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Navegación y paneles en el portal de Application Insights
Una vez [configurado Application Insights en su proyecto](../../azure-monitor/app/app-insights-overview.md), aparecerán los datos de telemetría acerca del rendimiento y el uso de la aplicación en el recurso del proyecto Application Insights en [Azure Portal](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Encontrar la telemetría
Inicie sesión en [Azure Portal](https://portal.azure.com), vaya al recurso de Application Insights que ha creado para la aplicación.

![Haga clic en Examinar, seleccione Application Insights y, después, seleccione su aplicación.](./media/app-insights-dashboards/00-start.png)

La hoja de información general (página) de la aplicación muestra un resumen de las métricas de diagnóstico clave de la aplicación y sirve de puerta de enlace a las demás características del portal.

![Rutas principales para ver los datos de telemetría](./media/app-insights-dashboards/010-oview.png)

Puede personalizar cualquiera de los gráficos y cuadrículas y anclarlos a un panel. De este modo, se puede reunir la telemetría clave de aplicaciones diferentes en un panel central.

## <a name="dashboards"></a>Paneles
Lo primero que verá después de iniciar sesión en el [Portal de Microsoft Azure](https://portal.azure.com) es un panel. Aquí puede agrupar los gráficos que le resultan más importantes de todos los recursos de Azure, incluidos los datos de telemetría desde [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md).

![Panel personalizado.](./media/app-insights-dashboards/31.png)

1. **Vaya a los recursos específicos** como la aplicación en Application Insights: utilice la barra de la izquierda.
2. **Vuelva al panel actual** o cambie a otras vistas recientes: use el menú desplegable de la parte superior izquierda.
3. **Cambie los paneles**: use el menú desplegable en el título del panel.
4. **Cree, edite y comparta paneles** con la barra de herramientas del panel.
5. **Edite el panel**: Mantenga el puntero sobre un icono y, a continuación, utilice la barra superior para moverlo, personalizarlo o quitarlo.

## <a name="add-to-a-dashboard"></a>Adición a un panel
Cuando esté consultando una hoja o un conjunto de gráficos especialmente interesante, puede anclar una copia al panel. Lo verá la próxima vez que regrese.

![Para anclar un gráfico, mantenga el puntero encima y haga clic en "..." en el encabezado.](./media/app-insights-dashboards/33.png)

1. Ancle un gráfico al panel. Una copia del gráfico aparece en el panel.
2. Ancle la hoja completa en el panel: aparecerá en el panel como un icono en el que puede hacer clic.
3. Haga clic en la esquina superior izquierda para volver al panel actual. A continuación, puede utilizar el menú desplegable para volver a la vista actual.

Observe que los gráficos se agrupan en iconos: un icono puede contener más de un gráfico. El icono se ancla entero al panel.

El gráfico se actualiza automáticamente con una frecuencia que depende del intervalo de tiempo del gráfico:

* Intervalo de tiempo de hasta 1 hora: actualizar cada 5 minutos
* Intervalo de tiempo de 1 a 24 horas: actualizar cada 15 minutos
* Intervalo de tiempo superior a 24 horas: (intervalo de tiempo)/60.

### <a name="pin-any-query-in-analytics"></a>Ancle cualquier consulta en Analytics
También puede [anclar gráficos de Analytics](../../azure-monitor/log-query/get-started-portal.md) a un panel compartido. Esto permite agregar gráficos de cualquier consulta arbitraria junto con las métricas estándares. 

Los resultados se recalculan automáticamente cada hora. Haga clic en el icono de actualización en el gráfico para recalcular inmediatamente. (Cuando el explorador se actualiza, no se recalculan los resultados).

## <a name="adjust-a-tile-on-the-dashboard"></a>Ajuste de un icono en el panel
Una vez que un icono se encuentra en el panel, es posible ajustarlo.

![Mantenga el mouse sobre un gráfico para poder editarlo.](./media/app-insights-dashboards/36.png)

1. Agregue un gráfico al icono.
2. Configure la métrica, la dimensión de agrupación y el estilo (tabla, gráfico) de un gráfico.
3. Arrastre por el diagrama para acercar; haga clic en el botón Deshacer para restablecer el intervalo de tiempo; establezca las propiedades de filtro para los gráficos del icono.
4. Establezca el título del icono.

Los iconos anclados desde hojas del explorador de métricas tienen más opciones de edición que los iconos anclados desde una hoja de información general.

El icono original que ancló no se ve afectado por las modificaciones.

## <a name="switch-between-dashboards"></a>Cambio entre paneles
Puede guardar más de un panel y cambiar de uno a otro. Cuando se ancla un gráfico o una hoja, se agrega al panel actual.

![Para cambiar entre los paneles, haga clic en Panel y seleccione un panel guardado. Para crear un panel y guardarlo, haga clic en Nuevo. Para reorganizar, haga clic en Editar.](./media/app-insights-dashboards/32.png)

Por ejemplo, podría tener un panel para mostrarlo en pantalla completa en la sala de reuniones y otro para el desarrollo general.

En el panel, una hoja aparece como un icono. Haga clic en él para ir a la hoja. Un gráfico replica el gráfico en su ubicación original.

![Haga clic en un icono para abrir la hoja que representa.](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Uso compartido de paneles
Cuando haya creado un panel, puede compartirlo con otros usuarios.

![En el encabezado del panel, haga clic en Compartir.](./media/app-insights-dashboards/41.png)

Aprenda más acerca de [Roles y control de acceso](../../azure-monitor/app/resources-roles-access-control.md).

## <a name="create-dashboards-programmatically"></a>Crear paneles mediante programación
Puede automatizar la creación de paneles mediante [Azure Resource Manager](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically) y un editor de JSON sencillo.

## <a name="app-navigation"></a>Navegación en la aplicación
La hoja de información general es la puerta de enlace para más información acerca de la aplicación.

* **Cualquier gráfico o icono**: haga clic en cualquier icono o gráfico para ver más detalles acerca de lo que muestra.

### <a name="overview-blade-buttons"></a>Botones de la hoja de información general
![Barra de navegación superior de hoja de información general](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Explorador de métricas**](../../azure-monitor/app/metrics-explorer.md): cree sus propios gráficos de rendimiento y uso.
* [**Búsqueda**](../../azure-monitor/app/diagnostic-search.md): investigue casos específicos de eventos como solicitudes, excepciones o seguimientos de registro.
* [**Analytics**](../../azure-monitor/app/analytics.md): consultas eficaces sobre los datos de telemetría.
* **Intervalo de tiempo**: ajuste el intervalo mostrado por todos los gráficos de la hoja.
* **Eliminar**: elimine el recurso de Application Insights para esta aplicación. Debe también quitar los paquetes de Application Insights del código de la aplicación, o editar la [clave de instrumentación](../../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) de la aplicación para dirigir la telemetría a un recurso de Application Insights diferente.

### <a name="essentials-tab"></a>Pestaña Essentials
* [Clave de instrumentación](../../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key): identifica este recurso de la aplicación.

### <a name="app-navigation-bar"></a>Barra de navegación de la aplicación
![Barra de navegación izquierda](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Información general**: vuelve a la hoja de información general de la aplicación.
* **Registro de actividad**: alertas y eventos administrativos de Azure.
* [**Control de acceso**](../../azure-monitor/app/resources-roles-access-control.md): proporciona acceso a los miembros del equipo y a otros elementos.
* [**Etiquetas**](../../azure-resource-manager/resource-group-using-tags.md): usa etiquetas para agrupar la aplicación con otras.

INVESTIGAR

* [**Mapa de aplicación**](app-map.md): mapa activo que muestra los componentes de la aplicación, obtenidos de la información de dependencia.
* [**Detección inteligente**](../../azure-monitor/app/proactive-diagnostics.md): revise las alertas recientes de rendimiento.
* [**Stream en vivo**](../../azure-monitor/app/live-stream.md): un conjunto fijo de métricas casi instantáneas, útiles al implementar una nueva compilación o depurar.
* [**Disponibilidad / pruebas web**](../../azure-monitor/app/monitor-web-app-availability.md): envíe solicitudes regulares a la aplicación web desde cualquier parte del mundo*.
* [**Errores, rendimiento**](../../azure-monitor/app/web-monitor-performance.md): excepciones, tasas de error y tiempos de respuesta para las solicitudes realizadas a la aplicación y para las solicitudes realizadas desde la aplicación a las [dependencias](../../azure-monitor/app/asp-net-dependencies.md).
* [**Rendimiento**](../../azure-monitor/app/web-monitor-performance.md): tiempo de respuesta, tiempos de respuesta de dependencia.
* [Servidores](../../azure-monitor/app/web-monitor-performance.md) : contadores de rendimiento. Está disponible si se [instala el Monitor de estado](../../azure-monitor/app/monitor-performance-live-website-now.md).
* **Explorador** : vista de página y rendimiento de AJAX. Está disponible si se [instrumentan las páginas web](../../azure-monitor/app/javascript.md).
* **Uso** : vista de página, recuentos de usuarios y sesiones. Está disponible si se [instrumentan las páginas web](../../azure-monitor/app/javascript.md).

CONFIGURAR

* **Introducción** : tutorial en línea.
* **Propiedades** : clave de instrumentación, suscripción e identificador de recursos.
* [Alertas](../../azure-monitor/app/alerts.md) : configuración de alertas de métricas.
* [Exportación continua](../../azure-monitor/app/export-telemetry.md) : configuración de la exportación de los datos de telemetría a Almacenamiento de Azure.
* [Pruebas de rendimiento](../../azure-monitor/app/monitor-web-app-availability.md#performance-tests) : configuración de una carga simétrica en el sitio web.
* [Cuotas y precios](../../azure-monitor/app/pricing.md) y [muestreo de ingesta](../../azure-monitor/app/sampling.md).
* **Acceso a API**: cree [anotaciones de versión](annotations.md) y para la API de acceso a datos.
* [**Elementos de trabajo**](../../azure-monitor/app/diagnostic-search.md#create-work-item): conéctese a un sistema de seguimiento de trabajos de modo que pueda crear errores mientras se inspeccionan los datos de telemetría.

CONFIGURACIÓN

* [**Bloqueos**](../../azure-resource-manager/resource-group-lock-resources.md): bloquee recursos de Azure.
* [**Script de Automation**](../../azure-monitor/app/powershell.md): exporte una definición del recurso de Azure de forma que pueda usarla como plantilla para crear nuevos recursos.


## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Pasos siguientes

|  |  |
| --- | --- |
| [Explorador de métricas](../../azure-monitor/app/metrics-explorer.md)<br/>Filtre y segmente las métricas |![Ejemplo de búsqueda](./media/app-insights-dashboards/64.png) |
| [Búsqueda de diagnóstico](../../azure-monitor/app/diagnostic-search.md)<br/>Busque y examine eventos y eventos relacionados, y cree errores. |![Ejemplo de búsqueda](./media/app-insights-dashboards/61.png) |
| [Analytics](../../azure-monitor/app/analytics.md)<br/>Lenguaje de consulta eficaz |![Ejemplo de búsqueda](./media/app-insights-dashboards/63.png) |
