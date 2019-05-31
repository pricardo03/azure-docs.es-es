---
title: Análisis de aplicaciones web de Java con Azure Application Insights | Microsoft Docs
description: 'Supervisión del rendimiento de aplicaciones web de Java con Application Insights. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: lagayhar
ms.openlocfilehash: 4cc1456aa9928cbd4a24c203eab62e3e75b4d7c1
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66256180"
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Introducción a Application Insights en un proyecto web de Java

[Application Insights](https://azure.microsoft.com/services/application-insights/) es un servicio de análisis extensible para desarrolladores web que ayuda a comprender el rendimiento y el uso de la aplicación activa. Úselo para [automáticamente la solicitud de instrumento, seguir las dependencias y los contadores de rendimiento recopilar](auto-collect-dependencies.md#java), diagnosticar problemas de rendimiento y excepciones, y [escribir código] [ api] para realizar un seguimiento de qué hacen los usuarios con la aplicación. 

![Captura de pantalla de información general con datos de ejemplo](./media/java-get-started/overview-graphs.png)

Application Insights es compatible con aplicaciones Java que se ejecutan en Linux, Unix o Windows.

Necesita:

* JRE versión 1.7 o 1.8
* Una suscripción a [Microsoft Azure](https://azure.microsoft.com/).

Si prefiere el marco Spring pruebe a [configurar una aplicación inicializadora del arranque de Spring para usar la guía de Application Insights](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Obtención de una clave de instrumentación de Application Insights
1. Inicie sesión en el [Portal de Microsoft Azure](https://portal.azure.com).
2. Cree un recurso en Application Insights. Establezca el tipo de aplicación a una aplicación web de Java.

3. Busque la clave de instrumentación del nuevo recurso. Pronto tendrá que pegarla en el proyecto de código.

    ![En la información general de nuevos recursos, haga clic en Propiedades y copie la clave de instrumentación.](./media/java-get-started/instrumentation-key-001.png)

## <a name="2-add-the-application-insights-sdk-for-java-to-your-project"></a>2. Incorporación del SDK de Application Insights para Java al proyecto
*Elija la forma adecuada para su proyecto.*

#### <a name="if-youre-using-maven-a-namemaven-setup-"></a>Si usa Maven... <a name="maven-setup" />
Si su proyecto ya se ha configurado para usar Maven para la compilación, combine el siguiente código en el archivo pom.xml.

A continuación, actualice las dependencias del proyecto, para obtener los archivos binarios descargados.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[2.0,)</version>
      </dependency>
    </dependencies>
```

* *¿Errores de validación en la suma de comprobación o la compilación?* Pruebe en su lugar una versión específica, como:`<version>2.0.n</version>`. La versión más reciente se encuentra en las [notas de la versión de SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) o en los [artefactos de Maven](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *¿Necesita actualizar a un nuevo SDK?*  Actualice las dependencias del proyecto.

#### <a name="if-youre-using-gradle-a-namegradle-setup-"></a>Si usa Gradle... <a name="gradle-setup" />
Si su proyecto ya se ha configurado para usar Grade para la compilación, combine el siguiente código al archivo build.gradle.

A continuación, actualice las dependencias del proyecto, para obtener los archivos binarios descargados.

```gradle

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '2.+'
      // or applicationinsights-core for bare API
    }
```

#### <a name="if-youre-using-eclipse-to-create-a-dynamic-web-project-"></a>Si usa Eclipse para crear un proyecto web dinámico...
Use el [complemento SDK de Application Insights para Java][eclipse]. Nota: Aunque con este complemento podrá comenzar a trabajar más rápido con Application Insights (suponiendo que no esté usando Maven o Gradle), no es un sistema de administración de dependencias. Por lo tanto, la actualización del complemento no actualizará automáticamente las bibliotecas de Application Insights en el proyecto.

* *¿Errores de validación en la suma de comprobación o la compilación?* Pruebe en su lugar una versión específica, como:`version:'2.0.n'`. La versión más reciente se encuentra en las [notas de la versión de SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) o en los [artefactos de Maven](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Para actualizar el SDK a uno nuevo*, actualice las dependencias del proyecto.

#### <a name="otherwise-if-you-are-manually-managing-dependencies-"></a>Por el contrario, si va a administrar manualmente las dependencias...
Descargue el [versión más reciente](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest), copie los archivos necesarios en el proyecto y reemplace todos los de las versiones anteriores.

### <a name="questions"></a>Preguntas...
* *¿Cuál es la relación entre los componentes `-core` y `-web`?*
  * `applicationinsights-core` le ofrece la API básica. Este componente se necesita siempre.
  * `applicationinsights-web` proporciona métricas que realizan el seguimiento de recuentos de solicitud HTTP y tiempos de respuesta. Este componente se puede omitir si no se desea que se recopilen automáticamente los datos de esta telemetría. Por ejemplo, si desea escribir la suya propia.
  
* *¿Cómo se debe actualizar el SDK a la versión más reciente?*
  * Si usa Gradle o Maven...
    * Actualice el archivo de compilación para especificar la versión más reciente, o bien use la sintaxis de carácter comodín de Maven o Gradle para incluir automáticamente la versión más reciente. Luego, actualice las dependencias del proyecto. La sintaxis del carácter comodín se puede ver en los ejemplos anteriores de [Gradle](#gradle-setup) o [Maven](#maven-setup).
  * Si va a administrar las dependencias manualmente...
    * Descargue el [SDK más reciente de Application Insights para Java](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) y sustituya los antiguos. Los cambios se describen en las [notas de la versión de SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-applicationinsightsxml-file"></a>3. Adición del archivo ApplicationInsights.xml
Agregue ApplicationInsights.xml a la carpeta de recursos del proyecto o asegúrese de que se agrega a la ruta de acceso de la clase de implementación del proyecto. Copie en ella el siguiente XML.

Sustituya la clave de instrumentación que obtuvo en el portal de Azure.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->
      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->
      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

Si lo desea, el archivo de configuración puede residir en cualquier ubicación a la que pueda acceder su aplicación.  La propiedad del sistema `-Dapplicationinsights.configurationDirectory` especifica el directorio que contiene ApplicationInsights.xml. Por ejemplo, un archivo de configuración ubicado en `E:\myconfigs\appinsights\ApplicationInsights.xml` se configuraría con la propiedad `-Dapplicationinsights.configurationDirectory="E:\myconfigs\appinsights"`.

* La clave de instrumentación se envía junto con todos los elementos de telemetría e indica a Application Insights que se muestre en el recurso.
* El componente de la solicitud HTTP es opcional. Envía automáticamente telemetría sobre las solicitudes y tiempos de respuesta en el portal.
* La correlación de eventos es una incorporación al componente de la solicitud HTTP. Asigna un identificador a cada solicitud recibida por el servidor y agrega este identificador como propiedad a todos los elementos de telemetría como la propiedad 'Operation.Id'. Le permite correlacionar la telemetría asociada a cada solicitud estableciendo un filtro en la [búsqueda de diagnóstico][diagnostic].

### <a name="alternative-ways-to-set-the-instrumentation-key"></a>Alternativas para establecer la clave de instrumentación
SDK de Application Insights busca la clave en este orden:

1. Propiedad del sistema:-DAPPLICATION_INSIGHTS_IKEY = your_ikey
2. Variable de entorno: APPLICATION_INSIGHTS_IKEY
3. Archivo de configuración: ApplicationInsights.xml

También se puede [configurar en el código](../../azure-monitor/app/api-custom-events-metrics.md#ikey):

```java
    String instrumentationKey = "00000000-0000-0000-0000-000000000000";

    if (instrumentationKey != null)
    {
        TelemetryConfiguration.getActive().setInstrumentationKey(instrumentationKey);
    }
```

## <a name="4-add-an-http-filter"></a>4. Adición de un filtro HTTP
El último paso de la configuración permite que el componente de la solicitud HTTP registre las solicitudes web. (No es necesario si solo desea la API básica).

### <a name="spring-boot-applications"></a>Aplicaciones de Spring Boot
Registre `WebRequestTrackingFilter` de Application Insights en la clase Configuration:

```Java
package <yourpackagename>.configurations;

import javax.servlet.Filter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.core.Ordered;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import com.microsoft.applicationinsights.TelemetryConfiguration;
import com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter;

@Configuration
public class AppInsightsConfig {

    @Bean
    public String telemetryConfig() {
        String telemetryKey = System.getenv("<instrumentation key>");
        if (telemetryKey != null) {
            TelemetryConfiguration.getActive().setInstrumentationKey(telemetryKey);
        }
        return telemetryKey;
    }

    /**
     * Programmatically registers a FilterRegistrationBean to register WebRequestTrackingFilter
     * @param webRequestTrackingFilter
     * @return Bean of type {@link FilterRegistrationBean}
     */
    @Bean
    public FilterRegistrationBean webRequestTrackingFilterRegistrationBean(WebRequestTrackingFilter webRequestTrackingFilter) {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(webRequestTrackingFilter);
        registration.addUrlPatterns("/*");
        registration.setOrder(Ordered.HIGHEST_PRECEDENCE + 10);
        return registration;
    }


    /**
     * Creates bean of type WebRequestTrackingFilter for request tracking
     * @param applicationName Name of the application to bind filter to
     * @return {@link Bean} of type {@link WebRequestTrackingFilter}
     */
    @Bean
    @ConditionalOnMissingBean

    public WebRequestTrackingFilter webRequestTrackingFilter(@Value("${spring.application.name:application}") String applicationName) {
        return new WebRequestTrackingFilter(applicationName);
    }


}
```

> [!NOTE]
> Si usa Spring Boot 1.3.8 o una versión anterior, reemplace FilterRegistrationBean con la línea siguiente:

```Java
    import org.springframework.boot.context.embedded.FilterRegistrationBean;
```

Esta clase configurará que `WebRequestTrackingFilter` sea el primer filtro de la cadena de filtros de http. También extraerá la clave de instrumentación de la variable de entorno del sistema operativo si está disponible.

> Estamos usando la configuración del filtro http web en lugar de la configuración de MVC de Spring porque se trata de una aplicación de Spring Boot y tiene su propia configuración de MVC de Spring. Consulte las secciones siguientes para la configuración específica de MVC de Spring.

### <a name="applications-using-webxml"></a>Aplicaciones que usan web.xml
Busque y abra el archivo web.xml en el proyecto y combine el siguiente código bajo el nodo web-app, donde se han configurado los filtros de aplicación.

Para obtener los resultados más precisos, el filtro debe asignarse antes de todos los demás filtros.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

   <!-- This listener handles shutting down the TelemetryClient when an application/servlet is undeployed. -->
    <listener>
      <listener-class>com.microsoft.applicationinsights.web.internal.ApplicationInsightsServletContextListener</listener-class>
    </listener>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Si utiliza Spring Web MVC 3.1, o cualquier versión posterior
Edite estos elementos en *-servlet.xml para incluir el paquete de Application Insights:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Si está usando Struts 2
Agregue este elemento al archivo de configuración Struts (denominado normalmente struts.xml o struts-default.xml):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

Si tiene interceptores definidos en una pila predeterminada, el interceptor se puede agregar a ella.

## <a name="5-run-your-application"></a>5. Ejecución de la aplicación
Ejecútela en modo de depuración en el equipo de desarrollo o bien publíquela en el servidor.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Visualización de la telemetría en Application Insights
Vuelva al recurso de Application Insights en el [Portal de Microsoft Azure](https://portal.azure.com).

Los datos de las solicitudes HTTP aparecen en la hoja de información general. (Si todavía no está ahí, espere unos segundos y, a continuación, haga clic en Actualizar).

![Captura de pantalla de información general con datos de ejemplo](./media/java-get-started/overview-graphs.png)

[Más información acerca de las métricas.][metrics]

Haga clic en cualquier gráfico para ver métricas agregadas más detalladas.

![Panel de errores de Application Insights con gráficos](./media/java-get-started/006-barcharts.png)

> Application Insights asume que el formato de las solicitudes HTTP para las aplicaciones de MVC es: `VERB controller/action`. Por ejemplo, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` y `GET Home/Product/sdf96vws` se agrupan en `GET Home/Product`. Esta agrupación permite agregaciones significativas de solicitudes, como el número de solicitudes y el tiempo medio de ejecución de las solicitudes.
>
>

### <a name="instance-data"></a>Datos de instancia
Haga clic en un tipo de solicitud específico para ver las instancias individuales.

![Profundizar en una vista de ejemplo concretos](./media/java-get-started/007-instance.png)

### <a name="analytics-powerful-query-language"></a>Análisis: Lenguaje de consulta eficaz
A medida que acumula más datos, puede ejecutar consultas tanto para agregar datos como para buscar instancias individuales.  [Analytics](../../azure-monitor/app/analytics.md) es una eficaz herramienta tanto para conocer el rendimiento y el uso, como para el diagnóstico.

![Ejemplo de Analytics](./media/java-get-started/0025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Instalación de la aplicación en el servidor
Ahora puede publicar la aplicación en el servidor, dejar que la utilicen los usuarios y ver la telemetría en el portal.

* Asegúrese de que el firewall permite que la aplicación envíe datos de telemetría a estos puertos:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Si el tráfico saliente debe enrutarse a través de un firewall, defina las propiedades de sistema `http.proxyHost` y `http.proxyPort`.

* En los servidores de Windows, instale:

  * [Microsoft Visual C++ Redistributable](https://www.microsoft.com/download/details.aspx?id=40784)

    (Este componente habilita los contadores de rendimiento.)

## <a name="azure-app-service-config-spring-boot"></a>Configuración de Azure App Service (Spring Boot)

Aplicaciones de Spring Boot que se ejecutan en Windows requieren configuración adicional para ejecutarse en Azure App Services. Modificar **web.config** y agregue lo siguiente:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified"/>
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\AzureWebAppExample-0.0.1-SNAPSHOT.jar&quot;">
        </httpPlatform>
    </system.webServer>
</configuration>
```

## <a name="exceptions-and-request-failures"></a>Excepciones y errores de solicitud
Las excepciones no controladas se recopilan automáticamente.

Para recopilar datos de otras excepciones, tiene dos opciones:

* [Insertar llamadas a TrackException en el código][apiexceptions].
* [Instalar el agente de Java en el servidor](java-agent.md). Debe especificar los métodos que desee ver.

## <a name="monitor-method-calls-and-external-dependencies"></a>Supervisión de llamadas a métodos y dependencias externas
[Instale el agente de Java](java-agent.md) para registrar los métodos internos especificados y las llamadas realizadas a través de JDBC, con datos de tiempo.

## <a name="w3c-distributed-tracing"></a>Seguimiento distribuido de W3C

El SDK de Java de Application Insights ahora admite el [seguimiento distribuido de W3C](https://w3c.github.io/trace-context/).

La configuración del SDK de entrada se explica más en detalle en nuestro artículo sobre [correlación](correlation.md#w3c-distributed-tracing).

La configuración del SDK de salida se define en el archivo [AI-Agent.xml](java-agent.md).

## <a name="performance-counters"></a>contadores de rendimiento
Abra **investigar**, **métricas**para ver un intervalo de contadores de rendimiento.

![Captura de pantalla del panel de métricas con los bytes privados del proceso seleccionado](./media/java-get-started/011-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Personalizar la recopilación de contadores de rendimiento
Para deshabilitar la recopilación del conjunto estándar de contadores de rendimiento, agregue el siguiente código bajo el nodo raíz del archivo ApplicationInsights.xml:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Recopilar contadores de rendimiento adicionales
Puede especificar contadores de rendimiento adicionales que se van a recopilar.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>Contadores JMX (expuestos por la máquina virtual de Java)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName` : el nombre mostrado en el portal de Application Insights.
* `objectName` : el nombre del objeto JMX.
* `attribute` : el atributo del nombre del objeto JMX que se va a capturar
* `type` (opcional): el tipo de atributo del objeto JMX:
  * Valor predeterminado: un tipo simple como int o long.
  * `composite`: los datos del contador de rendimiento tienen el formato 'Attribute.Data'
  * `tabular`: los datos del contador de rendimiento tienen el formato de una fila de tabla

#### <a name="windows-performance-counters"></a>Contadores de rendimiento de Windows
Cada [contador de rendimiento de Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) es un miembro de una categoría (de la misma manera que un campo es un miembro de una clase). Las categorías puede ser globales, o pueden tener instancias con nombre o numeradas.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName: el nombre mostrado en el portal de Application Insights.
* categoryName: la categoría de contador de rendimiento (objeto de rendimiento) con la que está asociada este contador de rendimiento.
* counterName: el nombre del contador de rendimiento.
* instanceName: el nombre de la instancia de categoría del contador de rendimiento o una cadena vacía (""), si la categoría contiene una sola instancia. Si categoryName es Proceso, y el contador de rendimiento que desea recopilar está en el proceso de JVM actual en que se ejecuta la aplicación, especifique `"__SELF__"`.

### <a name="unix-performance-counters"></a>Contadores de rendimiento de Unix
* [Instale collectd con el complemento de Application Insights](java-collectd.md) para obtener una amplia variedad de datos de red y del sistema.

## <a name="local-forwarder"></a>Reenviador local

Un [reenviador local](https://docs.microsoft.com/azure/application-insights/local-forwarder) es un agente que recopila datos de telemetría de Application Insights o [OpenCensus](https://opencensus.io/) procedentes de diversos SDK y plataformas y los enruta a Application Insights. Se puede ejecutar en Windows y Linux.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<DeveloperMode>false</DeveloperMode>
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>
<!-- The properties below are optional. The values shown are the defaults for each property -->
<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

Si usa el inicio de SpringBoot, agregue lo siguiente al archivo de configuración (application.properies):

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Los valores predeterminados son los mismos para la configuración de application.properties y applicationinsights.xml de SpringBoot.

## <a name="get-user-and-session-data"></a>Obtención de datos de usuario y sesión
Bien, va a enviar telemetría desde el servidor web. Ahora, para obtener la visión completa de 360 grados de la aplicación, puede agregar mayor supervisión:

* [Agregue telemetría a las páginas web][usage] para supervisar las vistas de páginas y las métricas de usuario.
* [Configure las pruebas web][availability] para comprobar que la aplicación efectivamente está activa y responde adecuadamente.

## <a name="capture-log-traces"></a>Captura de seguimiento de registros
Application Insights se puede utilizar para segmentar y desglosar los registros desde Log4J, Logback u otras plataformas de registro. Puede correlacionar los registros con solicitudes HTTP y otra telemetría. [Vea cómo][javalogs].

## <a name="send-your-own-telemetry"></a>Envío de su propia telemetría
Ahora que ha instalado el SDK, puede utilizar la API para enviar su propia telemetría.

* [Realice el seguimiento de eventos y métricas personalizados][api] para saber qué hacen los usuarios con su aplicación.
* [Busque eventos y registros][diagnostic] para ayudar a diagnosticar problemas.

## <a name="availability-web-tests"></a>Pruebas web de disponibilidad
Application Insights puede probar su sitio web a intervalos regulares para comprobar que está activo y que responde correctamente.

[Más información sobre cómo configurar pruebas web de disponibilidad.][availability]

## <a name="questions-problems"></a>¿Tiene preguntas? ¿Tiene problemas?
[Solución de problemas de Java](java-troubleshoot.md)

## <a name="next-steps"></a>Pasos siguientes
* [Supervisión de llamadas a dependencias](java-agent.md)
* [Supervisión de contadores de rendimiento de Unix](java-collectd.md)
* Incorporación de la [supervisión a las páginas web](javascript.md) para controlar los tiempos de carga, las llamadas de AJAX y la excepciones del explorador.
* Escritura de una [telemetría personalizada](../../azure-monitor/app/api-custom-events-metrics.md) para hacer un seguimiento del uso en el explorador o el servidor.
* Uso de [Analytics](../../azure-monitor/app/analytics.md) para realizar consultas eficaces sobre los datos de telemetría de la aplicación
* Para más información, visite [Azure para desarrolladores de Java](/java/azure).

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#trackexception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: ../../azure-monitor/learn/java-quick-start.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[usage]: javascript.md
