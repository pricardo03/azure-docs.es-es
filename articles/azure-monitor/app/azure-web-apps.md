---
title: Supervisar el rendimiento de Azure App Services | Microsoft Docs
description: Supervisión del rendimiento de aplicaciones de Azure App Services. Carga y tiempo de respuesta de gráfico, información de dependencia y establecer alertas en el rendimiento.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: mbullwin
ms.openlocfilehash: 92a7c1a45655f8804aa1f81b1a77ebf7cd5197e8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58122172"
---
# <a name="monitor-azure-app-service-performance"></a>Supervisar el rendimiento de Azure App Service
En [Azure Portal](https://portal.azure.com), puede configurar la supervisión del rendimiento de aplicaciones de sus aplicaciones web, back-ends móviles y aplicaciones de API en [Azure App Service](../../app-service/overview.md). [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) instrumenta la aplicación para que envíe datos de telemetría sobre sus actividades al servicio Application Insights, donde se almacenan y analizan. En esta plataforma, se pueden usar los gráficos de métricas y las herramientas de búsqueda para ayudar a diagnosticar problemas, mejorar el rendimiento y evaluar el uso.

## <a name="runtime-or-build-time"></a>Tiempo de compilación o en tiempo de ejecución
Puede configurar la supervisión mediante la instrumentación de la aplicación de dos maneras:

* **En tiempo de ejecución** -puede seleccionar una extensión de supervisión cuando el servicio de aplicación ya está en funcionamiento de rendimiento. No es necesario volver a compilar ni instalar la aplicación. Tendrá a disposición un conjunto estándar de paquetes que supervisan tiempos de respuesta, tasas de éxito, excepciones, dependencias, etc.

* **Tiempo de compilación** : puede instalar un paquete en la aplicación que esté en desarrollo. Esta opción es más versátil. Además de los mismos paquetes estándares, puede escribir código para personalizar los datos de telemetría o enviar los suyos propios. Puede registrar las actividades específicas o grabar eventos según la semántica del dominio de la aplicación. Esto también le permite probar la versión más reciente del SDK de Application Insights como desee evaluar el SDK beta, mientras que la supervisión en tiempo de ejecución está restringido a la versión estable más reciente.

## <a name="runtime-instrumentation-with-application-insights"></a>Instrumentación en tiempo de ejecución con Application Insights
Si está ejecutando un servicio de aplicaciones en Azure, ya puede obtener cierta supervisión: tasas de solicitudes y errores de forma predeterminada. Agregue Application Insights para obtener más características, como tiempos de respuesta, supervisión de las llamadas a las dependencias, detección inteligente y tener acceso al eficaz lenguaje de consulta Kusto. 

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    ![En Configuración, elija Application Insights](./media/azure-web-apps/settings-app-insights.png)

   * Elija crear un nuevo recurso a menos que ya haya configurado un recurso de Application Insights para esta aplicación. 

     > [!NOTE]
     > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

     ![Instrumentación de la aplicación web](./media/azure-web-apps/create-resource.png)

2. Después de especificar qué recurso se debe usar, puede elegir cómo quiere que Application Insights recopile los datos de cada plataforma para la aplicación. Supervisión de aplicaciones ASP.NET está en forma predeterminada con dos niveles diferentes de la colección.

    ![Opciones de elección para cada plataforma](./media/azure-web-apps/choose-options-new.png)

   * El nivel de recopilación **Básico** de .NET ofrece funcionalidades esenciales de APM de instancia única.
    
   * El nivel de recopilación **Recomendado** de .NET:
       * Agrega las tendencias de uso de CPU, memoria y E/S.
       * Correlaciona los microservicios entre los límites de solicitud y dependencia.
       * Recopila las tendencias de uso y habilita la correlación entre los resultados de disponibilidad y las transacciones.
       * Recopila las excepciones no controladas por el proceso de host.
       * Mejora la precisión de las métricas de APM con carga, cuando se usa el muestreo.
    
     .NET core ofrece **recomienda colección** o **deshabilitado** para .NET Core 2.0 y 2.1.

3. **Instrumente el servicio de aplicaciones** después de haber instalado Application Insights.

   **Habilite la supervisión de cliente** para la vista de página y la telemetría de usuario.

    (Esta opción está habilitada de forma predeterminada para las aplicaciones de .NET Core con la **recopilación recomendada**, independientemente de si está presente el valor "APPINSIGHTS_JAVASCRIPT_ENABLED" de la aplicación. Actualmente, el soporte granular basado en la interfaz de usuario para deshabilitar la supervisión de cliente no está disponible para .NET Core).
    
   * Seleccione Configuración > Configuración de la aplicación.
   * En Configuración de la aplicación, agregue un nuevo par clave-valor:

     Clave: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Valor: `true`
   * **Guarde** la configuración y **reinicie** la aplicación.

4. Explorar los datos de supervisión de la aplicación seleccionando **Configuración** > **Application Insights** > **View more in Application Insights** (Ver más en Application Insights).

Posteriormente, si quiere, puede compilar la aplicación con Application Insights.

*¿Cómo puedo quitar Application Insights o cambiar para enviar a otro recurso?*

* En Azure, abra la hoja de control de la aplicación web y, en Configuración, abra **Application Insights**. Puede desactivar Application Insights al hacer clic en **Deshabilitar** en la parte superior o puede seleccionar un recurso nuevo en la sección **Cambiar el recurso**.

## <a name="build-the-app-with-application-insights"></a>Compilación de la aplicación con Application Insights
Application Insights puede proporcionar una telemetría más detallada instalando un SDK en la aplicación. En concreto, puede recopilar registros de seguimiento, [escribir telemetría personalizada](../../azure-monitor/app/api-custom-events-metrics.md), y obtener informes de excepción más detallados.

1. **En Visual Studio** (2013 Update 2 o posterior), configure Application Insights para el proyecto.

    Haga clic con el botón derecho en el proyecto web y seleccione **Agregar > Application Insights** o **Proyecto** > **Application Insights** > **Configurar Application Insights**.

    ![Haga clic con el botón derecho en el proyecto web y elija Agregar o Configurar Application Insights](./media/azure-web-apps/03-add.png)

    Cuando se la pide que inicie sesión, use las credenciales de su cuenta de Azure.

    La operación tiene dos efectos:

   1. Crea un recurso de Application Insights en Azure, donde se almacenan, analizan y muestran los datos de telemetría.
   2. Agrega el paquete NuGet de Application Insights al código (si todavía no está) y lo configura para enviar los datos de telemetría al recurso de Azure.
2. **Pruebe la telemetría** ejecutando la aplicación en la máquina de desarrollo (F5).
3. **Publique la aplicación** en Azure de la forma habitual. 

*¿Cómo cambio para enviar a un recurso de Application Insights diferente?*

* En Visual Studio, haga clic con el botón derecho en el proyecto, elija **Configurar Application Insights** y seleccione el recurso que quiera. Tiene la opción de crear un nuevo recurso. Vuelva a compilar e implementar.

## <a name="automate-monitoring"></a>Automatizar la supervisión

La configuración de la aplicación debe configurarse con el fin de habilitar la recopilación de datos de telemetría con Application Insights:

   ![Servicio de configuración de la aplicación con las opciones disponibles de Application Insights](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Definiciones de configuración de aplicación

|Nombre del valor de configuración de la aplicación |  Definición | Valor |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Extensión principal, que controla la supervisión en tiempo de ejecución. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  De forma predeterminada en están habilitadas las características del modo únicamente, esenciales para garantizar un rendimiento óptimo. | `default` o `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Controla si el motor de reescritura binario `InstrumentationEngine` se activará. Esta configuración tiene implicaciones de rendimiento y afecta a la hora de inicio/inicio en frío. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Controla si el texto de la tabla SQL y Azure se capturará junto con las llamadas de dependencia. Advertencia de rendimiento: Esto requiere la `InstrumentationEngine`. | `~1` |

### <a name="app-service-application-settings-with-azure-resource-manager"></a>Configuración de la aplicación de servicio con Azure Resource Manager

Configuración de la aplicación de servicios de aplicaciones puede administrarse y configurado con [plantillas Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). Este método puede usarse al implementar nuevos recursos de App Service con la automatización de Azure Resource Manager, o para modificar la configuración de los recursos existentes.

La estructura básica de la configuración de aplicación JSON para un servicio de aplicaciones está por debajo:

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]

```

Para obtener un ejemplo de un administrador de recursos de Azure plantilla con la configuración de la aplicación configurada para utilizar Application Insights esto [plantilla](https://github.com/Andrew-MSFT/BasicImageGallery) puede resultar útil, concretamente, a partir de la sección [línea 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Automatizar la creación de un recurso de Application Insights y un vínculo a App Service recién creado.

Para crear una plantilla de Azure Resource Manager con toda la configuración de Application Insights predeterminado configurada, comenzar el proceso como si se va a crear una nueva aplicación Web con Application Insights habilitado.

Seleccione **opciones de automatización**

   ![Menú de creación de App Service web app](./media/azure-web-apps/create-web-app.png)

Esto genera la plantilla de Azure Resource Manager más reciente con toda la configuración necesaria configurada.

  ![Plantilla de aplicación web de App Service](./media/azure-web-apps/arm-template.png)

> [!NOTE]
> La plantilla generará la configuración de la aplicación en modo de "predeterminado". Este modo es rendimiento optimizado, aunque puede modificar la plantilla para activar cualquier características que prefiera.

## <a name="more-telemetry"></a>Más telemetría

* [Datos de carga de página web](../../azure-monitor/app/javascript.md)
* [Telemetría personalizada](../../azure-monitor/app/api-custom-events-metrics.md)

## <a name="troubleshooting"></a>solución de problemas

### <a name="do-i-still-need-to-go-to-extensions---add---application-insights-extension-for-new-app-service-apps"></a>¿Sigo necesitando ir a las extensiones, agregar, extensión de Application Insights para aplicaciones de App Service?

No, ya no necesita agregar la extensión manualmente. Habilitación de Application Insights a través de la hoja configuración agregará toda la configuración de aplicación necesaria para habilitar la supervisión. Ahora es posible, dado que los archivos agregados anteriormente por la extensión son ahora [preinstalado](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) como parte de la imagen de App Service. Los archivos se encuentran en `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent`.

### <a name="if-runtime-and-build-time-monitoring-are-both-enabled-do-i-end-up-with-duplicate-data"></a>Si se activan en tiempo de ejecución y supervisión en tiempo de compilación ¿termino con datos duplicados?

No, de forma predeterminada si se detecta la supervisión en tiempo de compilación a través de la extensión de supervisión en tiempo de ejecución dejará de enviar datos y se respetará solo el tiempo de compilación configuración de supervisión. La determinación de si se debe deshabilitar la supervisión en tiempo de ejecución se basa en la detección de cualquiera de estos tres archivos:

* Microsoft.ApplicationInsights dll
* Microsoft.ASP.NET.TelemetryCorrelation dll
* System.Diagnostics.DiagnosticSource dll

Es importante tener en cuenta que en muchas versiones de Visual Studio, algunos o todos estos archivos se agregan de forma predeterminada a los archivos de plantilla ASP.NET y ASP.NET Core Visual Studio. Si el proyecto se creó en función de una de las plantillas, incluso si no ha habilitado explícitamente Application Insights, la presencia de la dependencia de archivo podría impedir la supervisión en tiempo de ejecución de activación.

### <a name="appinsightsjavascriptenabled-causes-incomplete-html-response-in-net-core-web-applications"></a>APPINSIGHTS_JAVASCRIPT_ENABLED da lugar a una respuesta HTML incompleta en aplicaciones web de .NET Core.

La habilitación de JavaScript a través de App Services puede hacer que se recorten las respuestas HTML.

* Solución alternativa 1: establezca la configuración de la aplicación APPINSIGHTS_JAVASCRIPT_ENABLED en false o quítela completamente y reinicie
* Solución 2: agregar sdk a través del código y quitar extensión (Profiler y Snapshot debugger no funcionará con esta configuración)

Para realizar un seguimiento de este problema, vaya a [extensión Azure causando la respuesta HTML incompleta](https://github.com/Microsoft/ApplicationInsights-Home/issues/277).

Para .NET Core, actualmente **no se admite** lo siguiente:

* Implementación autocontenida.
* Aplicaciones con .NET Framework como destino.
* Aplicaciones de .NET Core 2.2.

> [!NOTE]
> Se admiten .NET core 2.0 y .NET Core 2.1. Este artículo se actualizará cuando se agregue compatibilidad con .NET Core 2.2.

## <a name="next-steps"></a>Pasos siguientes
* [Ejecute el generador de perfiles en la aplicación activa](../../azure-monitor/app/profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): supervisar Azure Functions con Application Insights
* [Diagnósticos de Microsoft Azure](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) para enviar este tipo de información a Application Insights.
* [Supervise las métricas del estado del servicio](../../azure-monitor/platform/data-collection.md) para asegurarse de que el servicio está disponible y responde adecuadamente.
* [Reciba notificaciones de alerta](../../azure-monitor/platform/alerts-overview.md) cada vez que se produzcan eventos de operaciones o las métricas traspasen un umbral.
* Use [aplicaciones y páginas web de Application Insights para JavaScript](../../azure-monitor/app/javascript.md) para obtener la telemetría del cliente de los exploradores que visitan una página web.
* [Configure pruebas web de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md) para recibir una alerta si el sitio deja de estar activo.