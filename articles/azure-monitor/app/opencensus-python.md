---
title: Seguimiento de Python en OpenCensus con Azure Application Insights | Microsoft Docs
description: Proporciona instrucciones para conectar el seguimiento de Python en OpenCensus con el reenviador local y Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ae9db483e15197e6cdaaaa5981410630184cc6ca
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957247"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Recopilación de seguimientos distribuidos en Python (versión preliminar)

Ahora, Application Insights permite realizar seguimientos distribuidos de las aplicaciones de Python mediante la integración con [OpenCensus](https://opencensus.io) y el nuevo [reenviador local](./../../azure-monitor/app/opencensus-local-forwarder.md). En este artículo, encontrará instrucciones paso a paso que le guiarán en el proceso de configuración de OpenCensus para Python y le ayudarán a llevar los datos de seguimiento a Application Insights.

## <a name="prerequisites"></a>Requisitos previos

- Necesita una suscripción de Azure.
- Se debe instalar Python; este artículo usa [Python 3.7.0](https://www.python.org/downloads/), aunque es probable que las versiones anteriores funcionen con un ajuste menor.
- Siga las instrucciones para instalar el [reenviador local como un servicio de Windows](./../../azure-monitor/app/opencensus-local-forwarder.md).

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-application-insights-resource"></a>Crear recurso de Application Insights

En primer lugar, tiene que crear un recurso de Application Insights. Este recurso generará una clave de instrumentación (ikey). La ikey se usa para configurar el reenviador local de forma que envíe seguimientos distribuidos desde la aplicación instrumentada de OpenCensus a Application Insights.   

1. Seleccione **Crear un recurso** > **Herramientas de desarrollo** > **Application Insights**.

   ![Incorporación del recurso Application Insights](./media/opencensus-python/0001-create-resource.png)

   Aparece un cuadro de configuración, use la tabla siguiente para rellenar los campos de entrada.

    | Configuración        | Valor           | DESCRIPCIÓN  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | Nombre que identifica la aplicación que se está supervisando |
   | **Tipo de aplicación** | General | Tipo de aplicación que se está supervisando |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el nuevo grupo de recursos que hospedará los datos de Application Insights |
   | **Ubicación** | Este de EE. UU | Elija una ubicación cerca de usted o de donde se hospeda la aplicación |

2. Haga clic en **Create**(Crear).

## <a name="configure-local-forwarder"></a>Configuración del reenviador local

1. Seleccione **Introducción** > **Información esencial** > copie la **clave de instrumentación** de la aplicación.

   ![Captura de pantalla de la clave de instrumentación](./media/opencensus-python/0003-instrumentation-key.png)

2. Modifique el archivo `LocalForwarder.config` y agregue la clave de instrumentación. Si ha seguido las instrucciones de los [requisitos previos](./../../azure-monitor/app/opencensus-local-forwarder.md), el archivo estará en `C:\LF-WindowsServiceHost`.

    ```xml
      <OpenCensusToApplicationInsights>
        <!--
          Instrumentation key to track telemetry to.
          -->
        <InstrumentationKey>{enter-instrumentation-key}</InstrumentationKey>
      </OpenCensusToApplicationInsights>
    
      <!-- Describes aspects of processing Application Insights telemetry-->
      <ApplicationInsights>
        <LiveMetricsStreamInstrumentationKey>{enter-instrumentation-key}</LiveMetricsStreamInstrumentationKey>
      </ApplicationInsights>
    </LocalForwarderConfiguration>
    ```

3. Reinicie el servicio **reenviador Local** de la aplicación.

## <a name="opencensus-python-package"></a>Paquete para Python de OpenCensus

1. Instale el paquete del censo abierto para Python y exportador con pip o pipenv desde la línea de comandos:

    ```console
    python -m pip install opencensus
    python -m pip install opencensus-ext-ocagent

    # pip env install opencensus
    ```

    > [!NOTE]
    > `python -m pip install opencensus` supone que tiene una variable de entorno PATH establecida para la instalación de Python. Si no la ha configurado, deberá proporcionar la ruta de acceso completa del directorio a la ubicación del archivo ejecutable de Python que desea que resulte en un comando: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus`.

2. Primero vamos a generar algunos datos de seguimiento localmente. En Python IDLE, o en el editor que prefiera, escriba el código siguiente:

    ```python
    from opencensus.trace.tracer import Tracer

    def main():
        while True:
            valuePrompt()

    def valuePrompt():
        tracer = Tracer()
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    if __name__ == "__main__":
        main()

    ```

3. Al ejecutar el código, se le solicitará varias veces que escriba un valor. Con cada entrada, el valor se imprimirá en el shell y se generará un fragmento correspondiente de **SpanData** en el módulo para Python de OpenCensus. El proyecto OpenCensus define un [ _seguimiento como un árbol de intervalos_](https://opencensus.io/core-concepts/tracing/).
    
    ```python
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=1f07f062ac394c50925f2ae61e635e14, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='5c17a4ad6ba14299', parent_span_id=None, attributes={}, start_time='2018-09-15T20:42:15.847292Z', end_time='2018-09-15T20:42:17.615664Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=c71b4e88a22a495da61df52ce3eee3e1, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='51547c0af5f046eb', parent_span_id=None, attributes={}, start_time='2018-09-15T20:42:17.615664Z', end_time='2018-09-15T20:48:11.160314Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=b4cdcc9e6df44a8fbb6e8ddeccc1351c, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f2caacf7892744d1', parent_span_id=None, attributes={}, start_time='2018-09-15T20:48:11.175931Z', end_time='2018-09-15T20:48:12.629178Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

4. Aunque esto resulta útil para fines de demostración, en última instancia deseamos emitir el SpanData de forma que pueda recogerse mediante nuestro **servicio de reenviador local** y ser enviado a Application Insights. Modifique el código del paso anterior con lo siguiente:

    ```python
    from opencensus.trace.tracer import Tracer
    from opencensus.trace import config_integration
    from opencensus.ext.ocagent.trace_exporter import TraceExporter
    from opencensus.trace import tracer as tracer_module

    import os

    def main():
        while True:
            valuePrompt()

    def valuePrompt():
        export_LocalForwarder = TraceExporter(
        service_name=os.getenv('SERVICE_NAME', 'python-service'),
        endpoint=os.getenv('OCAGENT_TRACE_EXPORTER_ENDPOINT'))

        tracer = Tracer(exporter=export_LocalForwarder)
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    if __name__ == "__main__":
        main()

    ```

5. Si guarda el módulo anterior e intenta a ejecutarlo, puede recibir un `ModuleNotFoundError` para `grpc`. Si esto ocurre, ejecute lo siguiente para instalar el [paquete grpcio](https://pypi.org/project/grpcio/) con:

    ```console
    python -m pip install grpcio
    ```

6. Ahora, al ejecutar el script de Python de antes, todavía se le seguirá pidiendo que especifique los valores, pero en el shell solo se imprime el valor.

7. Para confirmar que el **reenviador local** recoge los seguimientos, compruebe el archivo `LocalForwarder.config`. Si ha seguido los pasos descritos en los [requisitos previos](https://docs.microsoft.com/azure/application-insights/local-forwarder), estarán en `C:\LF-WindowsServiceHost`.

    En la imagen siguiente del archivo de registro, puede ver que, antes de ejecutar el segundo script donde se ha agregado un exportador, `OpenCensus input BatchesReceived` era 0. Una vez que se empezó a ejecutar el script actualizado `BatchesReceived`, se incrementó en el número de valores que se escribió:
    
    ![Formulario de nuevo recurso de Application Insights](./media/opencensus-python/0004-batches-received.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Inicio de la supervisión en Azure Portal

1. Ahora puede volver a abrir la página **Información general** de Application Insights en Azure Portal para ver los detalles de la aplicación en ejecución. Seleccione **Live Metrics Stream**.

   ![Captura de pantalla del panel de información general con Live Metrics Stream seleccionado en el cuadro rojo](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Si ejecuta el segundo script de Python de nuevo y empieza a escribir los valores, verá datos de seguimiento en tiempo real tal y como llegan a Application Insights desde el servicio de reenviador local.

   ![Captura de pantalla de Live Metrics Stream con datos de rendimiento](./media/opencensus-python/0006-stream.png)

3. Vaya de nuevo a la página **Información general** y seleccione **Mapa de aplicación** para obtener un esquema visual de las relaciones de dependencia y la temporalización de las llamadas entre los componentes de la aplicación.

    ![Captura de pantalla del mapa de aplicación](./media/opencensus-python/0007-application-map.png)

    Puesto que solo estábamos realizando el seguimiento de una llamada a un método, el mapa de aplicación no resulta tan interesante. Pero puede escalarlo para visualizar más aplicaciones distribuidas:

   ![Mapa de aplicación](media/opencensus-python/application-map.png)

4. Seleccione **Investigar el rendimiento** para realizar el análisis detallado del rendimiento y determinar la causa de un rendimiento lento.

    ![Captura de pantalla del panel de rendimiento](./media/opencensus-python/0008-performance.png)

5. Al seleccionar **Ejemplos** y, a continuación, hacer clic en alguno de los ejemplos que aparecen en el panel derecho, se iniciará la experiencia de los detalles de la transacción completa. Aunque nuestra aplicación de ejemplo solo nos mostrará un único evento, una aplicación más compleja podría permitirle explorar la transacción completa hasta el nivel de la pila de llamadas de un evento individual.

     ![Captura de pantalla de la interfaz de una transacción completa](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="opencensus-trace-for-python"></a>Seguimiento de OpenCensus para Python

Solo hemos tratado los conceptos básicos de cómo conectar OpenCensus para Python con el reenviador local y Application Insights. Las instrucciones oficiales de uso tratan temas más avanzados, como son:

* [Muestras](https://opencensus.io/api/python/trace/usage.html#samplers)
* [Integración con Flask](https://opencensus.io/api/python/trace/usage.html#flask)
* [Integración con Django](https://opencensus.io/api/python/trace/usage.html#django)
* [Integración con MySQL](https://opencensus.io/api/python/trace/usage.html#service-integration)
* [PostgreSQL](https://opencensus.io/api/python/trace/usage.html#postgresql)
  
## <a name="next-steps"></a>Pasos siguientes

* [Guía de uso de Python de OpenCensus](https://opencensus.io/api/python/trace/usage.html)
* [Mapa de aplicación](./../../azure-monitor/app/app-map.md)
* [Supervisión del rendimiento de un extremo a otro](./../../azure-monitor/learn/tutorial-performance.md)
