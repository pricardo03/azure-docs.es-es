---
title: Supervisión de aplicaciones de Python con Azure Application Insights | Microsoft Docs
description: Proporciona instrucciones para conectar Python de OpenCensus con Application Insights
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 07/02/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: 2c043ad793dcf5e59eaf460d1ec4aa7a3b48810d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541434"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Recopilación de seguimientos distribuidos en Python (versión preliminar)

Ahora, Application Insights permite realizar seguimientos distribuidos de las aplicaciones de Python mediante la integración con [OpenCensus](https://opencensus.io). En este artículo, encontrará instrucciones paso a paso que le guiarán en el proceso de configuración de OpenCensus para Python y le ayudarán a llevar los datos de supervisión a Application Insights.

## <a name="prerequisites"></a>Requisitos previos

- Necesita una suscripción de Azure.
- Se debe instalar Python; este artículo usa [Python 3.7.0](https://www.python.org/downloads/), aunque es probable que las versiones anteriores funcionen con un ajuste menor.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-application-insights-resource"></a>Creación de un recurso de Application Insights

En primer lugar, tiene que crear un recurso de Application Insights. Este recurso generará una clave de instrumentación (ikey). La clave ikey se usará para configurar el SDK de OpenCensus para enviar datos de telemetría a Application Insights.

1. Seleccione **Crear un recurso** > **Herramientas de desarrollo** > **Application Insights**.

   ![Incorporación del recurso Application Insights](./media/opencensus-python/0001-create-resource.png)

   Aparece un cuadro de configuración, use la tabla siguiente para rellenar los campos de entrada.

    | Configuración        | Valor           | DESCRIPCIÓN  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | Nombre que identifica la aplicación que se está supervisando |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el nuevo grupo de recursos que hospedará los datos de Application Insights |
   | **Ubicación** | Este de EE. UU | Elija una ubicación cerca de usted o de donde se hospeda la aplicación |

2. Haga clic en **Create**(Crear).

## <a name="install-opencensus-azure-monitor-exporters"></a>Instalar exportadores de Azure Monitor de OpenCensus

1. Instale los exportadores de Azure Monitor de OpenCensus:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure` supone que tiene una variable de entorno PATH establecida para la instalación de Python. Si no la ha configurado, deberá proporcionar la ruta de acceso completa del directorio a la ubicación del archivo ejecutable de Python que desea que resulte en un comando: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. Primero vamos a generar algunos datos de seguimiento localmente. En Python IDLE, o en el editor que prefiera, escriba el código siguiente:

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

3. Al ejecutar el código, se le solicitará varias veces que escriba un valor. Con cada entrada, el valor se imprimirá en el shell y se generará un fragmento correspondiente de **SpanData** en el módulo para Python de OpenCensus. El proyecto OpenCensus define un [ _seguimiento como un árbol de intervalos_](https://opencensus.io/core-concepts/tracing/).
    
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

4. Aunque es útil para fines de demostración, en última instancia queremos emitir el SpanData a Application Insights. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            instrumentation_key='00000000-0000-0000-0000-000000000000',
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
5. Ahora, al ejecutar el script de Python, todavía se le seguirá pidiendo que especifique los valores, pero en el shell solo se imprime el valor.

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

## <a name="opencensus-for-python"></a>OpenCensus para Python

* [Personalización](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Integración con Flask](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Integración con Django](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [Integración con MySQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Pasos siguientes

* [Python de OpenCensus en GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Mapa de aplicación](./../../azure-monitor/app/app-map.md)
* [Supervisión del rendimiento de un extremo a otro](./../../azure-monitor/learn/tutorial-performance.md)