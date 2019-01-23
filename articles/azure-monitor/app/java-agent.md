---
title: Supervisión de rendimiento de aplicaciones web de Java en Azure Application Insights | Microsoft Docs
description: Supervisión extendida del rendimiento y el uso de su sitio web de Java con Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: mbullwin
ms.openlocfilehash: dbca662f38f13833a4b9e642a4d8f690017d999a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262139"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Supervisión de dependencias, excepciones detectadas y tiempos de ejecución del método en aplicaciones web de Java


Si ha [instrumentado la aplicación web de Java con Application Insights][java], puede usar el agente de Java para obtener información más clara, sin tener que realizar cambios de código:

* **Dependencias:** datos sobre las llamadas realizadas por la aplicación a otros componentes, por ejemplo:
  * Las **llamadas REST** realizadas a través de HttpClient, OkHttp y RestTemplate (Spring) se capturan.
  * Las llamadas **Redis** realizadas a través del cliente de Jedis se capturan.
  * **[Llamadas JDBC](https://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**: los comandos de MySQL, SQL Server y Oracle DB se capturan automáticamente. En el caso de MySQL, si la llamada tarda más de 10 s, el agente notifica el plan de consulta.
* **Excepciones detectadas:** información sobre las excepciones que controla el código.
* **Tiempo de ejecución del método:** información sobre el tiempo necesario para ejecutar métodos específicos.

Para usar el agente de Java, debe instalarlo en el servidor. Las aplicaciones web deben instrumentarse con el [SDK de Application Insights para Java][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Instalación del agente de Application Insights para Java
1. [Descargue el agente](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest)en la máquina que ejecuta el servidor de Java. Asegúrese de descargar la misma versión del agente de Java que los paquetes web y principales del SDK de Application Insights para Java.
2. Edite el script de inicio del servidor de aplicaciones y agregue la siguiente JVM:
   
    `javaagent:`*ruta de acceso completa al archivo JAR del agente*
   
    Por ejemplo, en Tomcat en un equipo Linux:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Reinicie el servidor de aplicaciones.

## <a name="configure-the-agent"></a>Configuración del agente
Cree un archivo denominado `AI-Agent.xml` y colóquelo en la misma carpeta que el archivo JAR del agente.

Establezca el contenido del archivo XML. Edite el ejemplo siguiente para incluir u omitir las características que desee.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Debe habilitar la excepción de los informes y los intervalos de método para métodos individuales.

De forma predeterminada, `reportExecutionTime` es true y `reportCaughtExceptions` es false.

### <a name="spring-boot-agent-additional-config"></a>Configuración adicional de Spring Boot Agent

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

> [!NOTE]
> AI-Agent.xml y el archivo jar del agente deben estar en la misma carpeta. A menudo se colocan juntos en la carpeta `/resources` del proyecto. 

#### <a name="enable-w3c-distributed-tracing"></a>Habilitación del seguimiento distribuido de W3C

Agregue lo siguiente a AI-Agent.xml:

```xml
<Instrumentation>
        <BuiltIn enabled="true">
            <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
        </BuiltIn>
    </Instrumentation>
```

> [!NOTE]
> El modo de compatibilidad con versiones anteriores está habilitado de forma predeterminada, y el parámetro enableW3CBackCompat es opcional y debe usarse solo cuando quiera desactivarlo. 

Lo ideal sería cuando todos los servicios se han actualizado a la versión más reciente de los SDK compatibles con el protocolo W3C. Se recomienda encarecidamente pasar a la versión más reciente de los SDK con compatibilidad con W3C lo antes posible.

Asegúrese de que **las configuraciones (del agente) tanto [entrantes](correlation.md#w3c-distributed-tracing) como salientes** sean exactamente iguales.

## <a name="view-the-data"></a>Visualización de los datos
En el recurso de Application Insights, aparecen tiempos de ejecución agregados de métodos y dependencias remotos [en el icono Rendimiento][metrics].

Para buscar instancias individuales de informes de dependencia, excepción y método, abra [Buscar][diagnostic].

[Más información sobre diagnósticos de problemas de dependencia](../../azure-monitor/app/asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>¿Tiene preguntas? ¿Tiene problemas?
* ¿No hay datos? [Establezca excepciones del firewall](../../azure-monitor/app/ip-addresses.md)
* [Solución de problemas de Java](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
