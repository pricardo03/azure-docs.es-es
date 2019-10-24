---
title: Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar) | Microsoft Docs
description: Se proporcionan instrucciones para conectar Python de OpenCensus con Azure Monitor.
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: ed61cb1bc88c48fe89c4a9390f04747749bd48c5
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329479"
---
# <a name="set-up-azure-monitor-for-your-python-application-preview"></a>Configuración de Azure Monitor para las aplicaciones de Python (versión preliminar)

Azure Monitor admite seguimiento distribuido, recopilación de métricas y registro de aplicaciones de Python gracias a la integración con [OpenCensus](https://opencensus.io). En este artículo, encontrará instrucciones paso a paso para configurar OpenCensus para Python y llevar los datos de supervisión a Azure Monitor.

## <a name="prerequisites"></a>Requisitos previos

- Necesita una suscripción de Azure.
- Debe estar instalado Python; en este artículo se usa [Python 3.7.0](https://www.python.org/downloads/), aunque es probable que las versiones anteriores funcionen con unos pocos ajustes.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-application-insights-resource-in-azure-monitor"></a>Creación de un recurso de Application Insights en Azure Monitor

En primer lugar, tiene que crear un recurso de Application Insights en Azure Monitor, que generará una clave de instrumentación (ikey). La clave ikey se usará entonces para configurar el SDK de OpenCensus y enviar datos de telemetría a Azure Monitor.

1. Seleccione **Crear un recurso** > **Herramientas de desarrollo** > **Application Insights**.

   ![Incorporación del recurso Application Insights](./media/opencensus-python/0001-create-resource.png)

   Aparece un cuadro de configuración, use la tabla siguiente para rellenar los campos de entrada.

    | Configuración        | Valor           | DESCRIPCIÓN  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | Nombre que identifica la aplicación que se está supervisando |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el nuevo grupo de recursos que hospedará los datos de Application Insights |
   | **Ubicación** | East US | Elija una ubicación cerca de usted o de donde se hospeda la aplicación |

2. Haga clic en **Create**(Crear).

## <a name="instrumenting-with-opencensus-python-sdk-for-azure-monitor"></a>Instrumentación con el SDK de Python para OpenCensus en Azure Monitor

1. Instale los exportadores de Azure Monitor de OpenCensus:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure` supone que tiene una variable de entorno PATH establecida para la instalación de Python. Si no la ha configurado, deberá proporcionar la ruta de acceso completa del directorio a la ubicación del archivo ejecutable de Python que desea que resulte en un comando: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. El SDK emplea tres exportadores de Azure Monitor para enviar distintos tipos de telemetría a Azure Monitor: seguimiento, métricas y registros. Eche un vistazo a la [información general sobre la plataforma de datos](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform) para más información sobre estos distintos tipos. Siga las instrucciones que se indican a continuación para ver cómo enviar estos tipos diferentes mediante los tres exportadores.

### <a name="trace"></a>Seguimiento

1. Primero vamos a generar algunos datos de seguimiento localmente. En el IDLE de Python, o en el editor que prefiera, escriba el código siguiente:

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Al ejecutar el código, se le solicitará varias veces que escriba un valor. Con cada entrada, el valor se imprimirá en el shell y se generará un fragmento correspondiente de **SpanData** en el módulo para Python de OpenCensus. El proyecto OpenCensus define un [ _seguimiento como un árbol de intervalos_](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Aunque es útil con fines de demostración, al final de lo que se trata es de emitir el valor de `SpanData` a Azure Monitor. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        ),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Ahora, al ejecutar el script de Python, todavía se le seguirá pidiendo que especifique los valores, pero en el shell solo se imprime el valor. El valor de `SpanData` creado se enviará a Azure Monitor. Puede encontrar los datos del intervalo emitidos en `dependencies`.

### <a name="metrics"></a>Métricas

1. Primero, se van a generar algunos datos de métrica locales. Se creará una métrica sencilla para realizar el seguimiento del número de veces que el usuario presiona la tecla Entrar.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Al ejecutar el código, se le solicitará varias veces que presione Entrar. Se crea una métrica para realizar el seguimiento del número de veces que se presiona esta tecla. Con cada pulsación, el valor aumentará y se mostrará la información de la métrica en la consola, con el valor y la marca de tiempo de cuando se actualizó la métrica.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Aunque es útil con fines de demostración, al final de lo que se trata es de emitir los datos de métricas a Azure Monitor. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. El exportador enviará datos de métricas a Azure Monitor según un intervalo fijo, cuyo valor predeterminado es cada 15 segundos. Se está realizando un seguimiento de una sola métrica, estos datos de métricas se enviarán en cada intervalo con el valor y la marca de tiempo que contengan. Puede encontrar los datos en `customMetrics`.

### <a name="logs"></a>Registros

1. Primero, se van a generar algunos datos de registro locales.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  El código pedirá continuamente que se escriba un valor. Se emite una entrada de registro para cada valor que se escriba que contenga el valor mencionado.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Aunque es útil con fines de demostración, al final de lo que se trata es de emitir los datos de métricas a Azure Monitor. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. El exportador enviará los datos de registro a Azure Monitor. Puede encontrar los datos en `traces`.

## <a name="start-monitoring-in-the-azure-portal"></a>Inicio de la supervisión en Azure Portal

1. Ahora puede volver a abrir la página **Información general** de Application Insights en Azure Portal para ver los detalles de la aplicación en ejecución. Seleccione **Live Metrics Stream**.

   ![Captura de pantalla del panel de información general con Live Metrics Stream seleccionado en el cuadro rojo](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Vaya de nuevo a la página **Información general** y seleccione **Mapa de aplicación** para obtener un esquema visual de las relaciones de dependencia y la temporalización de las llamadas entre los componentes de la aplicación.

    ![Captura de pantalla del mapa de aplicación](./media/opencensus-python/0007-application-map.png)

    Puesto que solo estábamos realizando el seguimiento de una llamada a un método, el mapa de aplicación no resulta tan interesante. Pero puede escalarlo para visualizar más aplicaciones distribuidas:

   ![Mapa de aplicación](media/opencensus-python/application-map.png)

3. Seleccione **Investigar el rendimiento** para realizar el análisis detallado del rendimiento y determinar la causa de un rendimiento lento.

    ![Captura de pantalla del panel de rendimiento](./media/opencensus-python/0008-performance.png)

4. Al seleccionar **Ejemplos** y, a continuación, hacer clic en alguno de los ejemplos que aparecen en el panel derecho, se iniciará la experiencia de los detalles de la transacción completa. Aunque nuestra aplicación de ejemplo solo nos mostrará un único evento, una aplicación más compleja podría permitirle explorar la transacción completa hasta el nivel de la pila de llamadas de un evento individual.

     ![Captura de pantalla de la interfaz de una transacción completa](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="view-your-data-with-queries"></a>Visualización de los datos con consultas

1. Puede ver los datos de telemetría que se enviaron desde la aplicación mediante la pestaña Registros (Analytics).

    ![Captura de pantalla del panel de información general con la pestaña Registros (Analytics) seleccionada en el cuadro rojo](./media/opencensus-python/0010-logs-query.png)

2. En el caso de la telemetría enviada con el exportador de seguimiento de Azure Monitor, las solicitudes entrantes se mostrarán en `requests` y las solicitudes salientes y en proceso se mostrarán en `dependencies`.

3. En el caso de la telemetría enviada con el exportador de métricas de Azure Monitor, las métricas enviadas se mostrarán en `customMetrics`.

4. En el caso de la telemetría enviada con el exportador de registros de Azure Monitor, los registros se mostrarán en `traces` y las solicitudes en `exceptions`.

5. Eche un vistazo a [Registros en Azure Monitor ](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) para más información sobre cómo usar consultas y registros.

## <a name="opencensus-for-python"></a>OpenCensus para Python

* [Python de OpenCensus en GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Personalización](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Integración con Flask](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Integración con Django](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [Integración con MySQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Pasos siguientes

* [Resumen de API](./../../azure-monitor/app/api-custom-events-metrics.md)
* [Mapa de aplicación](./../../azure-monitor/app/app-map.md)
* [Supervisión del rendimiento de un extremo a otro](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Alertas

* [Pruebas de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md): cree pruebas para asegurarse de que el sitio sea visible en la Web.
* [Diagnósticos inteligentes](../../azure-monitor/app/proactive-diagnostics.md): estas pruebas se realizan automáticamente, por lo que no es preciso hacer nada para configurarlas. Le indican si la aplicación tiene una tasa de solicitudes con error inusual.
* [Alertas de métricas](../../azure-monitor/app/alerts.md): Establezca alertas que le adviertan si una métrica supera un umbral. Puede establecerlas en las métricas personalizadas que codifique en la aplicación.