---
title: Seguimiento de Go en OpenCensus con Azure Application Insights | Microsoft Docs
description: Este artículo contiene instrucciones acerca de cómo integrar el seguimiento de Go en OpenCensus con el reenviador local y Application Insights.
ms.topic: conceptual
ms.date: 09/15/2018
ms.openlocfilehash: ddb8dfbf14f2c75e4d16693076e56a711d8861ad
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77669988"
---
# <a name="collect-distributed-traces-from-go-preview"></a>Recopilación de seguimientos distribuidos en Go (versión preliminar)

Ahora, Application Insights permite realizar seguimientos distribuidos de las aplicaciones de Go mediante la integración con [OpenCensus](https://opencensus.io) y el nuevo [reenviador local](./opencensus-local-forwarder.md). En este artículo, encontrará instrucciones paso a paso que le guiarán por el proceso de configuración de OpenCensus para Go y le ayudarán a llevar los datos de seguimiento a Application Insights.

## <a name="prerequisites"></a>Prerrequisitos

- Necesita una suscripción de Azure.
- Go debe estar instalado (en este artículo, se utiliza usa la versión 1.11; [Descarga de Go](https://golang.org/dl/)).
- Siga las instrucciones para instalar el [reenviador local como un servicio de Windows](./opencensus-local-forwarder.md).

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-application-insights-resource"></a>Creación de un recurso de Application Insights

En primer lugar, tiene que crear un recurso de Application Insights. Este recurso generará una clave de instrumentación (ikey). La ikey se usa para configurar el reenviador local de forma que envíe seguimientos distribuidos desde la aplicación instrumentada de OpenCensus a Application Insights.   

1. Seleccione **Crear un recurso** > **Herramientas de desarrollo** > **Application Insights**.

   ![Incorporación del recurso Application Insights](./media/opencensus-Go/0001-create-resource.png)

 > [!NOTE]
   >Si esta es la primera vez que crea un recurso de Application Insights, puede obtener más información en el artículo [Creación de recursos en Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

   Aparece un cuadro de configuración, use la tabla siguiente para rellenar los campos de entrada.

   | Configuración        | Value           | Descripción  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | Nombre que identifica la aplicación que se está supervisando |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el nuevo grupo de recursos que hospedará los datos de Application Insights |
   | **Ubicación** | Este de EE. UU. | Elija una ubicación cerca de usted o de donde se hospeda la aplicación |

2. Haga clic en **Crear**.

## <a name="configure-local-forwarder"></a>Configuración del reenviador local

1. Seleccione **Introducción** > **Información esencial** > copie la **clave de instrumentación** de la aplicación.

   ![Captura de pantalla de la clave de instrumentación](./media/opencensus-Go/0003-instrumentation-key.png)

2. Modifique el archivo `LocalForwarder.config` y agregue la clave de instrumentación. Si ha seguido las instrucciones de los [requisitos previos](./opencensus-local-forwarder.md), el archivo estará en `C:\LF-WindowsServiceHost`.

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

3. Reinicie el servicio **Reenviador Local** de la aplicación.

## <a name="opencensus-go-packages"></a>Paquetes de OpenCensus para Go

1. Instale los paquetes de Open Census para Go desde la línea de comandos:

    ```go
    go get -u go.opencensus.io
    go get -u contrib.go.opencensus.io/exporter/ocagent
    ```

2. Agregue el código siguiente a un archivo .go y después compílelo y ejecútelo. (Este ejemplo se ha elaborado siguiendo las instrucciones oficiales de OpenCensus sin ningún código agregado que facilite la integración con el reenviador local).

     ```go
        // Copyright 2018, OpenCensus Authors
        //
        // Licensed under the Apache License, Version 2.0 (the "License");
        // you may not use this file except in compliance with the License.
        // You may obtain a copy of the License at
        //
        //     https://www.apache.org/licenses/LICENSE-2.0
        //
        // Unless required by applicable law or agreed to in writing, software
        // distributed under the License is distributed on an "AS IS" BASIS,
        // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        // See the License for the specific language governing permissions and
        // limitations under the License.
        package main
        
        import (
        
            "bytes"
            "fmt"
            "log"
            "net/http"
            os "os"
            
            ocagent "contrib.go.opencensus.io/exporter/ocagent"
            "go.opencensus.io/plugin/ochttp"
            "go.opencensus.io/plugin/ochttp/propagation/tracecontext"
            "go.opencensus.io/trace"
        
        )
        
        func main() {
            // Register stats and trace exporters to export the collected data.
            serviceName := os.Getenv("SERVICE_NAME")
            if len(serviceName) == 0 {
                serviceName = "go-app"
            }
            fmt.Printf(serviceName)
            agentEndpoint := os.Getenv("OCAGENT_TRACE_EXPORTER_ENDPOINT")

            if len(agentEndpoint) == 0 {
                agentEndpoint = fmt.Sprintf("%s:%d", ocagent.DefaultAgentHost, ocagent.DefaultAgentPort)
            }
        
            fmt.Printf(agentEndpoint)
            exporter, err := ocagent.NewExporter(ocagent.WithInsecure(), ocagent.WithServiceName(serviceName), ocagent.WithAddress(agentEndpoint))
        
            if err != nil {
                log.Printf("Failed to create the agent exporter: %v", err)
            }
        
            trace.RegisterExporter(exporter)
        
            trace.ApplyConfig(trace.Config{DefaultSampler: trace.AlwaysSample()})
        
            client := &http.Client{Transport: &ochttp.Transport{Propagation: &tracecontext.HTTPFormat{}}}
        
            http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                var jsonStr = []byte(`[ { "url": "http://blank.org", "arguments": [] } ]`)
                r, _ := http.NewRequest("POST", "http://blank.org", bytes.NewBuffer(jsonStr))
                r.Header.Set("Content-Type", "application/json")
        
                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }
            })
        
            http.HandleFunc("/call_blank", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                r, _ := http.NewRequest("GET", "http://blank.org", nil)

                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
        
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }        
            })
        
            log.Fatal(http.ListenAndServe(":50030", &ochttp.Handler{Propagation: &tracecontext.HTTPFormat{}}))
        
        }
     ```

3. Una vez que se ejecuta la aplicación de go simple, vaya a `http://localhost:50030`. Cada actualización del explorador generará el texto "hello world" acompañado por los datos del intervalo correspondientes que recoge el reenviador local.

4. Para confirmar que el **reenviador local** recoge los seguimientos, compruebe el archivo `LocalForwarder.config`. Si ha seguido los pasos descritos en los [requisitos previos](https://docs.microsoft.com/azure/application-insights/local-forwarder), estarán en `C:\LF-WindowsServiceHost`.

    En la imagen siguiente del archivo de registro, puede ver que, antes de ejecutar el segundo script donde se ha agregado un exportador, `OpenCensus input BatchesReceived` era 0. Una vez que se empezó a ejecutar el script actualizado `BatchesReceived`, se incrementó en el número de valores que se escribió:
    
    ![Formulario de nuevo recurso de Application Insights](./media/opencensus-go/0004-batches-received.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Inicio de la supervisión en Azure Portal

1. Ahora puede volver a abrir la página **Información general** de Application Insights en Azure Portal para ver los detalles de la aplicación en ejecución. Seleccione **Live Metrics Stream**.

   ![Captura de pantalla del panel de información general con Live Metrics Stream seleccionado en el cuadro rojo](./media/opencensus-go/0005-overview-live-metrics-stream.png)

2. Si ejecuta la segunda aplicación de Go de nuevo y empieza a actualizar el explorador en `http://localhost:50030`, verá datos de seguimiento en tiempo real tal y como llegan a Application Insights desde el servicio de reenviador local.

   ![Captura de pantalla de Live Metrics Stream con datos de rendimiento](./media/opencensus-go/0006-stream.png)

3. Vaya de nuevo a la página **Información general** y seleccione **Mapa de aplicación** para obtener un esquema visual de las relaciones de dependencia y la temporalización de las llamadas entre los componentes de la aplicación.

    ![Captura de pantalla del mapa de aplicación](./media/opencensus-go/0007-application-map.png)

    Puesto que solo estábamos realizando el seguimiento de una llamada a un método, el mapa de aplicación no resulta tan interesante. Pero puede escalarlo para visualizar más aplicaciones distribuidas:

   ![Mapa de aplicación](media/opencensus-go/application-map.png)

4. Seleccione **Investigar el rendimiento** para realizar el análisis detallado del rendimiento y determinar la causa de un rendimiento lento.

    ![Captura de pantalla del panel de rendimiento](./media/opencensus-go/0008-performance.png)

5. Al seleccionar **Ejemplos** y, a continuación, hacer clic en alguno de los ejemplos que aparecen en el panel derecho, se iniciará la experiencia de los detalles de la transacción completa. Aunque nuestra aplicación de ejemplo solo nos mostrará un único evento, una aplicación más compleja podría permitirle explorar la transacción completa hasta el nivel de la pila de llamadas de un evento individual.

     ![Captura de pantalla de la interfaz de una transacción completa](./media/opencensus-go/0009-end-to-end-transaction.png)

## <a name="opencensus-trace-for-go"></a>Seguimiento de OpenCensus para Go

Solo hemos tratado los conceptos básicos de la integración de OpenCensus para Go con el reenviador local y Application Insights. La [Guía de uso oficial de Go de OpenCensus](https://godoc.org/go.opencensus.io) trata más temas avanzados.

## <a name="next-steps"></a>Pasos siguientes

* [Mapa de aplicación](./../../azure-monitor/app/app-map.md)
* [Supervisión del rendimiento de un extremo a otro](./../../azure-monitor/learn/tutorial-performance.md)
