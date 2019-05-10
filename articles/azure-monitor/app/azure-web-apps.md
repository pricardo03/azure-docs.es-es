---
title: Supervisar el rendimiento de Azure App Services | Microsoft Docs
description: Supervisión del rendimiento de aplicaciones de Azure App Services. Gráfico de carga y el tiempo de respuesta, la información de dependencia y establecer alertas en el rendimiento.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: mbullwin
ms.openlocfilehash: ec5b3572cbf74bad9b82eb93a45d7a4664023b95
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65408237"
---
# <a name="monitor-azure-app-service-performance"></a>Supervisar el rendimiento de Azure App Service

Habilitar la supervisión en un .NET y .NET Core en función de las aplicaciones web que se ejecutan en [Azure App Services](https://docs.microsoft.com/azure/app-service/) ahora es más fácil que nunca. Mientras que antes, era necesario instalar manualmente una extensión de sitio, el agente de extensión más reciente y ahora está integrado en la imagen del servicio de aplicación de forma predeterminada. En este artículo se le guiará a través de habilitar la supervisión de Application Insights, así como proporcionar instrucciones preliminares para automatizar el proceso para implementaciones a gran escala.

> [!NOTE]
> Cómo agregar manualmente una extensión de sitio de Application Insights a través de **herramientas de desarrollo** > **extensiones** está en desuso. Este método de instalación de la extensión dependía de actualizaciones manuales para cada nueva versión. La última versión estable de la extensión es ahora [preinstalado](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) como parte de la imagen de App Service. Los archivos se encuentran en `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` y se actualizan automáticamente con cada versión estable. Si sigue las instrucciones en función del agente para habilitar la supervisión a continuación, quitará automáticamente la extensión en desuso para usted.

## <a name="enable-application-insights"></a>Habilitar Application Insights

Hay dos maneras de habilitar la supervisión de aplicaciones para las aplicaciones hospedadas de Azure App Services:

* **Supervisión de aplicaciones basada en agente** (ApplicationInsightsAgent).  
    * Este método es el más fácil habilitar, y se requiere ninguna configuración avanzada. A menudo se conoce como supervisión "en tiempo de ejecución". Para los servicios de aplicación de Azure se recomienda como mínimo la habilitación de este nivel de supervisión y, a continuación, en función del escenario específico que se puede evaluar si es necesaria una supervisión más avanzada a través de la instrumentación manual.

* **Instrumentación manual de la aplicación mediante código** al instalar el SDK de Application Insights.

    * Este enfoque es mucho más personalizable, pero requiere [agregar una dependencia en los paquetes de NuGet del SDK de Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Este método, también significa que tiene que administrar las actualizaciones a la versión más reciente de los paquetes usted mismo.

    * Si necesita realizar llamadas de API personalizadas a seguimiento de eventos y las dependencias no capturadas de forma predeterminada con la supervisión basada en agente, deberá utilizar este método. Consulte la [API para el artículo de eventos y métricas personalizado](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) para obtener más información.

> [!NOTE]
> SDK manual y sin agente en función de supervisión en la función de instrumentación se detecte que respetará solo la configuración de la instrumentación manual. Esto es para evitar datos duplicados de enviados. Para obtener más información sobre esta desprotección la [sección Solución de problemas](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting) a continuación.

## <a name="enable-agent-based-monitoring-net"></a>Habilite .NET supervisión basada en agente

> [!NOTE]
> no se admite la combinación de APPINSIGHTS_JAVASCRIPT_ENABLED y urlCompression. Para obtener más información, vea la explicación en el [sección Solución de problemas](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting).


1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    ![En Configuración, elija Application Insights](./media/azure-web-apps/settings-app-insights-01.png)

   * Elija crear un nuevo recurso a menos que ya haya configurado un recurso de Application Insights para esta aplicación. 

     > [!NOTE]
     > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

     ![Instrumentación de la aplicación web](./media/azure-web-apps/create-resource-01.png)

2. Después de especificar qué recurso se debe usar, puede elegir cómo quiere que Application Insights recopile los datos de cada plataforma para la aplicación. Supervisión de aplicaciones ASP.NET está en forma predeterminada con dos niveles diferentes de la colección.

    ![Opciones de elección para cada plataforma](./media/azure-web-apps/choose-options-new.png)

   * El nivel de recopilación **Básico** de .NET ofrece funcionalidades esenciales de APM de instancia única.

   * El nivel de recopilación **Recomendado** de .NET:
       * Agrega las tendencias de uso de CPU, memoria y E/S.
       * Correlaciona los microservicios entre los límites de solicitud y dependencia.
       * Recopila las tendencias de uso y habilita la correlación entre los resultados de disponibilidad y las transacciones.
       * Recopila las excepciones no controladas por el proceso de host.
       * Mejora la precisión de las métricas de APM con carga, cuando se usa el muestreo.

3. Para configurar los valores como muestreo, que anteriormente se puede controlar mediante el archivo applicationinsights.config ahora puede interactuar con esos mismos valores a través de la configuración de la aplicación con un prefijo correspondiente. 

    * Por ejemplo, para cambiar el porcentaje de muestreo inicial, puede crear una configuración de aplicación de: `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` y un valor de `100`.

    * Para obtener la lista de configuración de procesador de telemetría de muestreo adaptable compatibles, puede consultar el [código](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/master/src/ServerTelemetryChannel/AdaptiveSamplingTelemetryProcessor.cs) y [documentación asociada](https://docs.microsoft.com/azure/azure-monitor/app/sampling).

## <a name="enable-agent-based-monitoring-net-core"></a>Habilitar supervisión .NET Core basada en agente

Se admiten las siguientes versiones de .NET Core: ASP.NET Core 2.0, ASP.NET Core 2.1, ASP.NET Core 2.2

Como destino el marco de trabajo completa de .NET Core, implementación autocontenida y ASP.NET Core 3.0 están **no admite** con/extensión del agente en función de supervisión. ([Instrumentación manual](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) a través del código funcionará en todos los escenarios anteriores.)

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    ![En Configuración, elija Application Insights](./media/azure-web-apps/settings-app-insights-01.png)

   * Elija crear un nuevo recurso a menos que ya haya configurado un recurso de Application Insights para esta aplicación. 

     > [!NOTE]
     > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

     ![Instrumentación de la aplicación web](./media/azure-web-apps/create-resource-01.png)

2. Después de especificar qué recursos se deben usar, puede elegir cómo desea que Application Insights para recopilar datos por plataforma para la aplicación. .NET core ofrece **recomienda colección** o **deshabilitado** para .NET Core 2.0, 2.1 y 2.2.

    ![Opciones de elección para cada plataforma](./media/azure-web-apps/choose-options-new-net-core.png)

## <a name="enable-client-side-monitoring-net"></a>Habilite .NET de supervisión de cliente

Supervisión de cliente es participar en ASP.NET. Para habilitar la supervisión de cliente:

* Seleccione **configuración** > ** ** configuración de aplicación ***
   * En la configuración de la aplicación, agregue un nuevo **nombre de la configuración de aplicación** y **valor**:

     Nombre: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Valor: `true`

   * **Guarde** la configuración y **reinicie** la aplicación.

![Captura de pantalla de la aplicación de configuración de la interfaz de usuario](./media/azure-web-apps/appinsights-javascript-enabled.png)

Para deshabilitar la supervisión quite el par clave-valor asociado de la configuración de la aplicación de cliente, o establezca el valor en false.

## <a name="enable-client-side-monitoring-net-core"></a>Habilitar la supervisión de cliente .NET Core

Supervisión de cliente es **habilitado de forma predeterminada** para aplicaciones de .NET Core con **recomienda colección**, independientemente de si está presente el valor 'APPINSIGHTS_JAVASCRIPT_ENABLED' de la aplicación.

Si por algún motivo desea deshabilitar la supervisión de cliente:

* Seleccione **configuración** > **configuración de la aplicación**
   * En la configuración de la aplicación, agregue un nuevo **nombre de la configuración de aplicación** y **valor**:

     Nombre: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Valor: `false`

   * **Guarde** la configuración y **reinicie** la aplicación.

![Captura de pantalla de la aplicación de configuración de la interfaz de usuario](./media/azure-web-apps/appinsights-javascript-disabled.png)

## <a name="automate-monitoring"></a>Automatizar la supervisión

Con el fin de habilitar la recopilación de datos de telemetría con Application Insights, la configuración de la aplicación deberá configurarse:

   ![Servicio de configuración de la aplicación con las opciones disponibles de Application Insights](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Definiciones de configuración de aplicación

|Nombre del valor de configuración de la aplicación |  Definición | Valor |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Extensión principal, que controla la supervisión en tiempo de ejecución. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  De forma predeterminada en están habilitadas las características del modo únicamente, esenciales para garantizar un rendimiento óptimo. | `default` o `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Controla si el motor de reescritura binario `InstrumentationEngine` se activará. Esta configuración tiene implicaciones de rendimiento y afecta a la hora de inicio/inicio en frío. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Controla si el texto de la tabla SQL y Azure se capturará junto con las llamadas de dependencia. Advertencia de rendimiento: esta configuración requiere la `InstrumentationEngine`. | `~1` |

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

Para obtener un ejemplo de una plantilla de Azure Resource Manager con la configuración de la aplicación configurada para Application Insights, esto [plantilla](https://github.com/Andrew-MSFT/BasicImageGallery) puede resultar útil, concretamente, a partir de la sección [línea 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Automatizar la creación de un recurso de Application Insights y un vínculo a App Service recién creado.

Para crear una plantilla de Azure Resource Manager con toda la configuración de Application Insights predeterminado configurada, comenzar el proceso como si se va a crear una nueva aplicación Web con Application Insights habilitado.

Seleccione **opciones de automatización**

   ![Menú de creación de App Service web app](./media/azure-web-apps/create-web-app.png)

Esta opción genera la plantilla de Azure Resource Manager más reciente con toda la configuración necesaria configurada.

  ![Plantilla de aplicación web de App Service](./media/azure-web-apps/arm-template.png)

A continuación es un ejemplo, reemplace todas las instancias de `AppMonitoredSite` con el nombre del sitio:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

> [!NOTE]
> La plantilla generará la configuración de la aplicación en modo de "predeterminado". Este modo es rendimiento optimizado, aunque puede modificar la plantilla para activar cualquier características que prefiera.

### <a name="enabling-through-powershell"></a>Habilitación a través de PowerShell

Para permitir que la aplicación de supervisión a través de PowerShell, sólo la configuración de aplicación subyacente debe cambiarse. A continuación es un ejemplo, que habilita la supervisión de aplicaciones para un sitio Web denominado "AppMonitoredSite" en el grupo de recursos "AppMonitoredRG", y configura los datos se envíen a la clave de instrumentación "012345678-abcd-ef01-2345-6789abcd".

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
$app = Get-AzWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} #preserve non Application Insights Application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # enable the ApplicationInsightsAgent
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent"></a>Actualizar a agente/extensión de supervisión

### <a name="upgrading-from-versions-289-and-up"></a>Actualización desde versiones 2.8.9 y seguridad

Actualizar desde versión 2.8.9 realiza automáticamente, sin ninguna acción adicional. Los nuevos bits de supervisión se entregan en segundo plano para el servicio de aplicación de destino y, en el reinicio de la aplicación se recogerán.

Para comprobar qué versión de la extensión ejecuta visita `http://yoursitename.scm.azurewebsites.net/ApplicationInsights`

![Captura de pantalla de la ruta de acceso url http://yoursitename.scm.azurewebsites.net/ApplicationInsights](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>Actualizar desde versiones 1.0.0 - 2.6.5

Comenzando con la versión 2.8.9 se utiliza la extensión de sitio instalado previamente. Si es una versión anterior, puede actualizar a través de dos maneras:

* [Actualización habilitando a través del portal](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights). (Incluso si tiene la extensión Application Insights para Azure App Service está instalado, la interfaz de usuario muestra solo **habilitar** botón. En segundo plano, la extensión de sitio privado anterior se quitará.)

* [Actualización a través de PowerShell](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell):

    1. Establecer la configuración de la aplicación para habilitar la extensión de sitio instalado previamente ApplicationInsightsAgent. Consulte [habilitación a través de powershell](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell).
    2. Quite manualmente la extensión de sitio privado con el nombre de extensión de Application Insights para Azure App Service.

Si se realiza la actualización desde una versión anterior 2.5.1, compruebe que los archivos DLL de ApplicationInsigths se quitan de la carpeta bin de la aplicación [vea pasos para solucionar problemas](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting).

## <a name="troubleshooting"></a>solución de problemas

A continuación es nuestra guía de solución de problemas paso a paso para la supervisión en función de extensión/agente para .NET y .NET Core en función de las aplicaciones que se ejecutan en Azure App Services.

> [!NOTE]
> Solo se admiten aplicaciones de Java y Node.js en Azure App Services a través de la instrumentación manual de SDK que se basa y, por tanto, los pasos siguientes no se aplican a estos escenarios.

1. Compruebe que la aplicación se supervisa a través de `ApplicationInsightsAgent`.
    * Compruebe que `ApplicationInsightsAgent_EXTENSION_VERSION` configuración de la aplicación se establece en un valor de "~ 2".
2. Asegúrese de que la aplicación cumple los requisitos que se deben supervisar.
    * Vaya a `https://yoursitename.scm.azurewebsites.net/ApplicationInsights`

    ![Captura de pantalla de https://yoursitename.scm.azurewebsites/applicationinsights página de resultados](./media/azure-web-apps/app-insights-sdk-status.png)

    * Confirme que la `Application Insights Extension Status` es `Pre-Installed Site Extension, version 2.8.12.1527, is running.`
        * Si no se está ejecutando, siga el [habilitar la supervisión de las instrucciones de Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights)

    * Confirme que el origen de estado existe y el siguiente aspecto: `Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`
        * Si un valor similar no está presente, significa que la aplicación no se está ejecutando actualmente o no se admite. Para asegurarse de que la aplicación se está ejecutando, intenta visitar manualmente los puntos de conexión de dirección url o aplicación de aplicación, que permitirá que la información en tiempo de ejecución esté disponible.

    * Confirme que `IKeyExists` es `true`
        * Si es false, agregue ' APPINSIGHTS_INSTRUMENTATIONKEY con el guid de ikey para la configuración de la aplicación.

    * Confirme que no hay ninguna entrada para `AppAlreadyInstrumented`, `AppContainsDiagnosticSourceAssembly`, y `AppContainsAspNetTelemetryCorrelationAssembly`.
        * Si existe alguna de estas entradas, quitar los siguientes paquetes de la aplicación: `Microsoft.ApplicationInsights`, `System.Diagnostics.DiagnosticSource`, y `Microsoft.AspNet.TelemetryCorrelation`.

La tabla siguiente proporciona una explicación más detallada de lo que significan estos valores, sus subyacente provoca y las correcciones recomendadas:

|Valor de problema|Explicación|Corregir
|---- |----|---|
| `AppAlreadyInstrumented:true` | Este valor indica que la extensión ha detectado que algún aspecto SDK de ya está presente en la aplicación y se espera. Esto puede deberse a una referencia a `System.Diagnostics.DiagnosticSource`, `Microsoft.AspNet.TelemetryCorrelation`, o `Microsoft.ApplicationInsights`  | Quite las referencias. Algunas de estas referencias se agregan de forma predeterminada en ciertas plantillas de Visual Studio y las versiones anteriores de Visual Studio pueden agregar referencias a `Microsoft.ApplicationInsights`.
|`AppAlreadyInstrumented:true` | Si la aplicación tiene como destino .NET Core 2.1 o 2.2 y hace referencia a [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapaquete, a continuación, presenta en Application Insights y extensión se retroceso. | Los clientes con .NET Core 2.1,2.2 [recomienda](https://github.com/aspnet/Announcements/issues/287) usar Microsoft.AspNetCore.App metapaquete en su lugar.|
|`AppAlreadyInstrumented:true` | Este valor también puede deberse a la presencia de los archivos DLL anterior en la carpeta de aplicación de una implementación anterior. | Limpiar la carpeta de aplicación para asegurarse de que se han quitado estos archivos DLL.|
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Este valor indica que la extensión detectó referencias a `Microsoft.AspNet.TelemetryCorrelation` en la aplicación y se espera. | Quite la referencia.
|`AppContainsDiagnosticSourceAssembly**:true`|Este valor indica que la extensión detectó referencias a `System.Diagnostics.DiagnosticSource` en la aplicación y se espera.| Quite la referencia.
|`IKeyExists:false`|Este valor indica que la clave de instrumentación no está presente en la AppSetting, `APPINSIGHTS_INSTRUMENTATIONKEY`. Causas posibles: Los valores es posible que haya eliminado por accidente, ¿ha olvidado establecer los valores en el script de automatización, etcetera. | Asegúrese de que la configuración está presente en la configuración de la aplicación de App Service.

### <a name="appinsightsjavascriptenabled-and-urlcompression-is-not-supported"></a>No se admite APPINSIGHTS_JAVASCRIPT_ENABLED y urlCompression

Si usa APPINSIGHTS_JAVASCRIPT_ENABLED = true en casos donde el contenido está codificado, podría obtener errores como: 

- 500 error de reescritura de direcciones URL
- 500.53 error del módulo de reescritura de URL con el mensaje saliente de reglas de reescritura no se puede aplicar cuando el contenido de la respuesta HTTP está codificado ('gzip'). 

Esto es debido a la configuración de la aplicación APPINSIGHTS_JAVASCRIPT_ENABLED se establece en true y la codificación de contenido está presente al mismo tiempo. Aún no se admite este escenario. La solución consiste en Quitar APPINSIGHTS_JAVASCRIPT_ENABLED de configuración de la aplicación. Desafortunadamente, esto significa que si la instrumentación de JavaScript del lado cliente o explorador sigue siendo necesario, referencias del SDK de manuales son necesarios para las páginas Web. Siga el [instrucciones](https://github.com/Microsoft/ApplicationInsights-JS#snippet-setup-ignore-if-using-npm-setup) para la instrumentación manual con el SDK de JavaScript.

Para obtener la información más reciente en la extensión o el agente de Application Insights, consulte el [notas de la versión](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md).

## <a name="next-steps"></a>Pasos siguientes
* [Ejecute el generador de perfiles en la aplicación activa](../app/profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): supervisar Azure Functions con Application Insights
* [Diagnósticos de Microsoft Azure](../platform/diagnostics-extension-to-application-insights.md) para enviar este tipo de información a Application Insights.
* [Supervise las métricas del estado del servicio](../platform/data-platform.md) para asegurarse de que el servicio está disponible y responde adecuadamente.
* [Reciba notificaciones de alerta](../platform/alerts-overview.md) cada vez que se produzcan eventos de operaciones o las métricas traspasen un umbral.
* Use [aplicaciones y páginas web de Application Insights para JavaScript](javascript.md) para obtener la telemetría del cliente de los exploradores que visitan una página web.
* [Configure pruebas web de disponibilidad](monitor-web-app-availability.md) para recibir una alerta si el sitio deja de estar activo.
