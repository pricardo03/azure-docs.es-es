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
ms.date: 10/25/2018
ms.author: mbullwin
ms.openlocfilehash: 17d8eff39eabb2f7b4968bf74d2482b980fe8060
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54116626"
---
# <a name="monitor-azure-app-service-performance"></a>Supervisar el rendimiento de Azure App Service
En [Azure Portal](https://portal.azure.com), puede configurar la supervisión del rendimiento de aplicaciones de sus aplicaciones web, back-ends móviles y aplicaciones de API en [Azure App Service](../../app-service/overview.md). [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) instrumenta la aplicación para que envíe datos de telemetría sobre sus actividades al servicio Application Insights, donde se almacenan y analizan. En esta plataforma, se pueden usar los gráficos de métricas y las herramientas de búsqueda para ayudar a diagnosticar problemas, mejorar el rendimiento y evaluar el uso.

## <a name="run-time-or-build-time"></a>Tiempo de ejecución o de compilación
Puede configurar la supervisión mediante la instrumentación de la aplicación de dos maneras:

* **Tiempo de ejecución**: puede seleccionar una extensión de supervisión de rendimiento cuando la aplicación web ya esté activa. No es necesario volver a compilar o instalar la aplicación. Tendrá a disposición un conjunto estándar de paquetes que supervisan tiempos de respuesta, tasas de éxito, excepciones, dependencias, etc. 
* **Tiempo de compilación** : puede instalar un paquete en la aplicación que esté en desarrollo. Esta opción es más versátil. Además de los mismos paquetes estándares, puede escribir código para personalizar los datos de telemetría o enviar los suyos propios. Puede registrar las actividades específicas o grabar eventos según la semántica del dominio de la aplicación. 

## <a name="run-time-instrumentation-with-application-insights"></a>Instrumentación del tiempo de ejecución con Application Insights
Si ya está ejecutando un servicio de aplicaciones en Azure, ya goza de cierta supervisión: tasas de solicitudes y errores. Agregue Application Insights para obtener más características, como tiempos de respuesta, supervisión de las llamadas a las dependencias, detección inteligente y el eficaz lenguaje de consulta de Log Analytics. 

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    ![En Configuración, elija Application Insights](./media/azure-web-apps/settings-app-insights.png)

   * Elija crear un nuevo recurso a menos que ya haya configurado un recurso de Application Insights para esta aplicación. 

    > [!NOTE]
    > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

    ![Instrumentación de la aplicación web](./media/azure-web-apps/create-resource.png)

2. Después de especificar qué recurso se debe usar, puede elegir cómo quiere que Application Insights recopile los datos de cada plataforma para la aplicación.

    ![Opciones de elección para cada plataforma](./media/azure-web-apps/choose-options.png)

3. **Instrumente el servicio de aplicaciones** después de haber instalado Application Insights.

   **Habilite la supervisión de cliente** para la vista de página y la telemetría de usuario.

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

* En Visual Studio, haga clic con el botón derecho en el proyecto, elija **Configurar Application Insights** y seleccione el recurso que desea. Tiene la opción de crear un nuevo recurso. Vuelva a compilar e implementar.

## <a name="more-telemetry"></a>Más telemetría

* [Datos de carga de página web](../../azure-monitor/app/javascript.md)
* [Telemetría personalizada](../../azure-monitor/app/api-custom-events-metrics.md)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="troubleshooting"></a>solución de problemas

### <a name="appinsightsjavascriptenabled-causes-incomplete-html-response-in-net-core-web-applications"></a>APPINSIGHTS_JAVASCRIPT_ENABLED da lugar a una respuesta HTML incompleta en aplicaciones web de .NET Core.

La habilitación de JavaScript a través de App Services puede hacer que se recorten las respuestas HTML.

- Solución alternativa 1: establezca la configuración de la aplicación APPINSIGHTS_JAVASCRIPT_ENABLED en false o quítela completamente y reinicie
- Solución alternativa 2: agregue sdk a través de código y quite la extensión (en el caso de Profiler y Snapshot Debugger no será posible con esta configuración)

Hacemos un seguimiento de este problema [aquí](https://github.com/Microsoft/ApplicationInsights-Home/issues/277)

## <a name="next-steps"></a>Pasos siguientes
* [Ejecute el generador de perfiles en la aplicación activa](../../azure-monitor/app/profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): supervisar Azure Functions con Application Insights
* [Diagnósticos de Microsoft Azure](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) para enviar este tipo de información a Application Insights.
* [Supervise las métricas del estado del servicio](../../azure-monitor/platform/data-collection.md) para asegurarse de que el servicio está disponible y responde adecuadamente.
* [Reciba notificaciones de alerta](../../azure-monitor/platform/alerts-overview.md) cada vez que se produzcan eventos de operaciones o las métricas traspasen un umbral.
* Use [aplicaciones y páginas web de Application Insights para JavaScript](../../azure-monitor/app/javascript.md) para obtener la telemetría del cliente de los exploradores que visitan una página web.
* [Configure pruebas web de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md) para recibir una alerta si el sitio deja de estar activo.

